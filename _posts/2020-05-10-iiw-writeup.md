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

#### On the use cases of email

There were comments about mixing two main email use cases:

- Person-to-person communication: Encryption and privacy becomes more important
- Unsolicited communication: Veracity of the sender becomes more important

While wide-spread use of public-key cryptography can help in both encryption of content and verification of the sender, this is not the case today (see next section).

Problems stemming from these different use cases can be solved separately. One such proposal is [the Notif protocol](https://www.slideshare.net/jim_fenton/notifs-2018), which tries to separate notifications into a abuse-resistant protocol that can be easily controlled at the receiver's end.

#### Isn't it "Better PGP?"

Yes, I think every sophisticated cryptographic protocol should have a user experience that hides the difficult mechanisms under the hood. That is what PGP has failed, and many secure messaging apps these days have succeeded (I believe [Keybase](https://keybase.io/) is doing this very elegantly). And I'm also aware that there were many attempts of building a "better PGP".

#### "You can still spoof your email address!"

This is also true, and even with an identity layer with Distributed Identifiers and Verified Credentials, there is still a risk of being spammed by "throwaway accounts".

As one comment pointed out, using VCs/DIDs to identify the sender can be thought of as replacing “pre-established trust” with a trust mechanism that can be globally used. This does not prevent spamming from a one-time-use account, but these one-time-use account have no "track record" that can be used by the receiving end to measure its trustworthiness. In use cases where you want your email to be really trusted, you will need to use an account that can be trusted by others, and that cannot be done by a throwaway account.

### Going forward

After attending Day 3, I now think that DIDComm is the way to move forward with secure messaging. Adjustments to email can still be made, but the fact that email lacks a proper identity layer cannot be solved by adding optional extensions, and even with this, this cannot be solved without major email providers (yes, the ones capitalizing off of email being centralized!) integrate these changes into their infrastructure. From my understanding of the DIDComm session (["The Future of Telecommunications is DIDComm"](https://iiw.idcommons.net/The_Future_of_Telecommunications_is_DID_Comm)), DIDComm solves the identity problem and encryption problem elegantly. Current concerns I have with DIDComm is how they first establish the connection. One of email's key usability feature is the email address, which is very easy to remember. If we have to rely on remembering very long strings to connect to someone for the first time, DIDComm-based messaging cannot be widely adopted by the public. Hopefully, current DIDComm-based messaging apps seems to have user-friendly introduction mechanisms, as I heard from a comment during my session, so it might not be a concern after all. Going forward, I will be looking into DIDComm and explore its capabilities.

One more thing I will be looking into particularly is the [JSON Web Messaging(JWM)](https://github.com/mattrglobal/jwm), which is a format that aims to describe a standard format for secure messaging using an extension of JOSE. This is designed to be used in combination with other delivery mechanisms such as HTTP(S), MLS, and DIDComm.

## The IIW experience itself

This is the first time attending any IIW and any remote "unconference" (I have attended some sessions in the past IETF meeting, which also went completely virtual).

Unconferences are a challenge, especially attending one that is not in my native language. Being able to ask questions in chat was very helpful for this reason.

Time zones ("virtual jetlag" as people say) was not much of an issue for me, because I was able to adjust my sleep schedule during the IIW. I took 2 days off work and slept during daytime in JST. One thing that was difficult for me was my living environment. With very thin walls and sessions being held during the night, I was risking bothering my neighbors with sound (I get frequently banged on the walls/floor for making too much noise when listening to music).

I learned a lot, especially about the landscape of Self-Sovereign Identity, which I think is not talked as much in Japan. Also the community was very warm and welcoming. I wish to attend one again in the future, and if there is any chance that IIW is going virtual again, I will surely attend that one.
