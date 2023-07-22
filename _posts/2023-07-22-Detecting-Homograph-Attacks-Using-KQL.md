---
title: Detecting Homograph Attacks Using KQL
date: 2023-07-22 11:30:00 +0000
categories: [Keep Calm and KQL, Alert Rules]
tags: [kql, sentinel, dfe]
pin: true
comments: true
math: true
mermaid: true
image:
  path: /assets/img/CoverImages/KCKQLURLs.png
  alt: Detecting Homograph attacks with KQL
---

## Introduction

Let’s play a game of spot the difference! Spot the difference in the URLs below:

> 
 www.facebook.com
 www.fаcebook.com
> 

If you got it, well done. You have a keen eye, and I bet you never get caught out by your company's phishing simulations. The difference is that in link number 2, the a is actually from the Cyrillic alphabet. Adversaries have been using this technique (referred to as a homograph attack) since the early 2000’s, attempting to deceive users by impersonating legitimate domains. There are far more characters than the Cyrillic lowercase ‘a’ which resemble characters in the latin alphabet, many looking almost identical. So, how do we detect it?

## Detection Principal

All of the characters in all of the alphabets take many forms, not through divine intervention, but through encoding. Encoding is the process of transforming a set of unicode characters into a sequence of bytes. Take UNICODE for example, unicode is a character encoding standard that provides a unique decimal value for every character, emoji or symbol in written language. So, we have essentially three forms for the characters using encoding: the character itself, the decimal value and the hex value. Lets look at our example from above in the decimal form:

> 
 119 119 119 46 102 97 99 101 98 111 111 107 46 99 111 109.
 119 119 119 46 102 208 176 99 101 98 111 111 107 46 99 111 109.
>

The difference is noticeable, even to the human eye. This is the premise for the alert rule. 

In UTF-8 every latin symbol’s decimal character representation is lower than 127. So, if we convert URLs to UTF-8 and the decimal value is higher than 127 then we know that the character is not from the latin alphabet, and potentially disguised. 

## The KQL

As always I will be using KQL to write the detection rules…

```sql
//Stripe OLT SOC - https://www.stripeolt.com
//Created by LJ
let suspiciousCharacters = range(127,1327,1);
//First search for suspicious characters in the RemoteUrl field of the DeviceNetworkEvents table
let NetworkLogDetections = (
    DeviceNetworkEvents
    | extend utfChars = to_utf8(RemoteUrl)
    | where utfChars has_any (suspiciousCharacters)
    //set intersect returns a dynamic array of utf decmimals where there is overlap in the criteria
    | extend SuspiciousCharactersDetected = set_intersect(suspiciousCharacters, utfChars)
    | project
            TimeGenerated,
            RemoteUrl,
            SuspiciousCharactersDetected,
            DeviceName,
            InitiatingProcessFileName,
            InitiatingProcessAccountUpn,
            ActionType
);
let EmailLogDetections = (
    EmailUrlInfo
| extend utfChars = to_utf8(Url)
    | where utfChars has_any (suspiciousCharacters)
    | join UrlClickEvents on $left.Url == $right.Url
    | extend SuspiciousCharactersDetected = set_intersect(suspiciousCharacters, utfChars)
    | project
        SuspiciousCharactersDetected,
            TimeGenerated,
            NetworkMessageId,
            Url,
            UrlLocation,
            AccountUpn,
            IPAddress,
            IsClickedThrough
);
union EmailLogDetections, NetworkLogDetections
```

1. The KQL determines suspicious characters as letters between 127 and 1327. 
    
    `let suspiciousCharacters = range(127,1327,1);`
    
2. We create a column which is a list of decimal numbers. We retrieve this list from converting a URL to UTF-8.
    
    `extend utfChars = to_utf8(RemoteUrl)` 

3. We identify the suspicious characters by creating a column using the `set_intersect` method. This method returns characters which are present in two sets. Therefore, we can return decimal values for only the suspicious characters, by using the `suspiciousCharacters` set as one of the parameters.

4. We apply the same logic for the `EmailUrlInfo` table from the Defender for Endpoint logs. By joining this with the `UrlClickEvents` table, we can see where any of these URLs have been clicked.

## Conclusion

In this article we looked at detecting homograph attacks by encoding characters to UTF-8. We then identified two log tables where we could search for the suspicious characters, but there are other tables as well. You could search through DNS tables, or any other logs that contain URLs just use your imagination. One thing to consider is that at some point your URL will converted into ASCII for URL encoding. So, at some point your Cyrillic character is no longer going to exist. 

Thanks for reading, and remember… Keep calm and KQL!

## References

[https://en.wikipedia.org/wiki/IDN_homograph_attack](https://en.wikipedia.org/wiki/IDN_homograph_attack)
[https://www.w3schools.com/charsets/ref_utf_basic_latin.asp](https://www.w3schools.com/charsets/ref_utf_basic_latin.asp)
