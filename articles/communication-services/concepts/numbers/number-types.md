---
title: Number Type concepts in Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Learn about Number Type concepts.
author: sadas
manager: rcole
services: azure-communication-services

ms.author: sadas
ms.date: 03/04/2022
ms.topic: conceptual
ms.service: azure-communication-services
---

# Number types

Azure Communication Services allows you to use phone numbers to make voice calls and send SMS messages with the public-switched telephone network (PSTN). In this document, we review the phone number types, region availability, and use cases for planning your telephony and SMS solution using Communication Services.

## Available options

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

Azure Communication Services offers three types of Numbers: Toll-Free, Local, Alphanumeric Sender IDs and Short Codes.

- **To send or receive an SMS**, choose a Toll-Free Number, local number or a Short Code
- **To make or receive phone calls**, choose a Toll-Free Number or a Local Number

This table summarizes the number types and supported capabilities:

| Type                                                                  | Example           | Send SMS | Receive SMS | Make Calls | Receive Calls | Typical Use Case                             | Restrictions   |
| :-------------------------------------------------------------------- | :---------------- | :------: | :---------: | :--------: | :-----------: | :------------------------------------------- | :------------- |
| [Toll-Free](../../quickstarts/telephony/get-phone-number.md)          | +1 (8AB) XYZ PQRS |   Yes    |     Yes     |    Yes     |      Yes      | Receive calls on IVR bots, SMS Notifications | SMS in US and CA only|
| [Local (Geographic)](../../quickstarts/telephony/get-phone-number.md) | +1 (ABC) XYZ PQRS |    Yes    |     Yes      |    Yes     |      Yes      | Geography Specific Number                  | SMS in US only  |
| [Mobile](../../quickstarts/telephony/get-phone-number.md) | +AB CDEFG PQRST|    Yes    |     Yes      |    No     |      No      | Geography Specific Number, SMS Notifications                | SMS Only   |
| [Short-Codes](../../quickstarts/sms/apply-for-short-code.md)          | ABC-XYZ           |   Yes    |     Yes     |     No     |      No       | High-velocity SMS                            | SMS only       |
| [Alphanumeric Sender ID](../../quickstarts/sms/enable-alphanumeric-sender-id.md#enable-dynamic-alphanumeric-sender-id)   | CONTOSO          |   Yes    |     Yes     |     No     |      No       | High-velocity SMS                            | SMS only       |

## Next steps

For more information about getting or managing phone numbers, see the following topics:

- Get a [Toll-Free or Local Phone Number](../../quickstarts/telephony/get-phone-number.md)
- Get a [Short-Code](../../quickstarts/sms/apply-for-short-code.md)
- [Quickstart: Look up operator information for a phone number](../../quickstarts/telephony/number-lookup.md)
