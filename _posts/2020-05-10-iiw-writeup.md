---
layout: page
title: Writeup for IIW30
data: 2020-05-10 00:00:00 +09:00
---

I'm finally writing an English blog entry for the first time!

So, this is going to be a writeup for my experience in [Internet Identity Workshop #30](https://internetidentityworkshop.com/). I've wanted to attend this event for a long time, and as the event went virtual in response to the COVID-19 crisis, there was no reason that I was not attending this time.

## On my session "Email, Messaging, and SSI/DID"

<script async class="speakerdeck-embed" data-id="611b9926d37b401981b4792367d7fb7e" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

I held a session at the end of the second day (4/30 22:30 JST, so 6:30 PT on the same day). This was an attempt to avoid conflicts with the major topics, as I decided to go for this session after the first day ended.

Many of the questions listed here are taken from my slides I used at a different place, and I added some additional ideas based on what I heard through sessions on Day 1 and 2.

Notes I wrote after the session can be found on [the IIW wiki](https://iiw.idcommons.net/Open_Discussion_on_Email,_Messaging,_and_SSI/DID).

### Motivations

After years of running my own SMTP server and building multiple apps that send email notifications, I was frustrated with how email works, especially the lack of a proper identity layer and proper encryption support. And as secure messaging apps are on the rise, I reached a belief that email should have these features given by these apps, or give its way to these rising messaging apps.

### Summary of my slides

- Email is still relevant because it enables users to send email to people without pre-established trust
- Email has spam because of its inherent anonymity
- Email used to be "self-sovereign", but now it is centralized by mail service providers
- Solutions
  - Verified Credentials extension for verifying sender identity?
  - DIDComm? (I was not sure about this at the time of writing the slides)

### Key input from other sessions

I had some major editing/addition done after attending some sessions.

One major realization was that the loss of email's self-sovereign characteristics can be explained by the [Principles of User Sovereignty](https://iiw.idcommons.net/Principles_of_User_Sovereignty_(1/3)) and [Fundamental Problems of Distributed Systems](https://iiw.idcommons.net/Fundamental_Problems_of_Distributed_Systems_(2/3)). Taking ideas from these sessions, we can say that email's corporate centralization as we see now was inevitable because email failed to address the fundamental problems that every distributed systems have in common. Yes, email has a standard protocol and is interoperable, but the difficulty of operating it correctly made it vulnerable against abuse, giving the service providers an opportunity to centralize email. These sessions gave reasoning behind why email lost its user sovereignty and a clearer understanding of the problem space I was dealing with.

### Responses

#### "You can still spoof your email address!"



### Going forward

After attending Day 3, I now think that DIDComm is the way to move forward. Adjustments to email can still be made, but the fact that email lacks a proper identity layer cannot be solved by adding optional extensions, and even with this, this cannot be solved without major email providers (yes, the ones capitalizing off of email being centralized!) integrate these changes into their infrastructure.

The [JSON Web Messaging(JWM)](https://github.com/mattrglobal/jwm) format aims to describe a standard format for secure messaging using an extension of JOSE. This is designed to be used in combination with other delivery mechanisms such as HTTP(S), MLS, and DIDComm.
