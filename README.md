# Mastodon Privacy Guide V.1.0

A guide on data protection obligations, challenges & pitfalls for Mastodon Users & Instance owners / admins.

# Table of Contents

- [Mastodon Privacy Guide V.1.0](#mastodon-privacy-guide-v10)
- [Table of contents](#table-of-contents)
  - [Who Are You and Why Should I Trust You?](#who-are-you-and-why-should-i-trust-you)
  - [Scope](#scope)
  - [But I Thought the GDPR (etc) Doesn't Apply to Me](#but-i-thought-the-gdpr-etc-doesnt-apply-to-me)
  - [Some Relevant Definitions](#some-relevant-definitions)
    - [Isn't Personal Data Just PII and Stuff? I'm not Collecting Any of That...](#isnt-personal-data-just-pii-and-stuff-im-not-collecting-any-of-that)
    - [WTF is Processing?](#wtf-is-processing)
    - [Controllers & Processors, how do they work?](#controllers--processors-how-do-they-work)
      - [A few examples](#a-few-examples)
    - [What's a Data Subject?](#whats-a-data-subject)
    - [What the hell does Establishment mean?](#what-the-hell-does-establishment-mean)
    - [Monitoring Behavior?](#monitoring-behavior)
  - [Okay. The Data Protection Laws May Apply. Now What?](#okay-the-data-protection-laws-may-apply-now-what)
    - [It's way more than just a privacy policy ...](#its-way-more-than-just-a-privacy-policy-)
      - [1. General obligations of controllers](#1-general-obligations-of-controllers)
      - [2. Data residency/sovereignity issues](#2-data-residencysovereignity-issues)
      - [3. Using your instance for legally dubious purposes](#3-using-your-instance-for-legally-dubious-purposes)
      - [4. Data portability](#4-data-portability)
      - [5. Building in privacy-enhancing tech](#5-building-in-privacy-enhancing-tech)
   - [Some Parting Thoughts](#some-parting-thoughts)

  
## Who Are You and Why Should I Trust You?
Name is Carey (@privacat@freeradical.zone / @privacat on the Birdsite) and I am an External Data Protection Consultant, researcher, and general buzzkill who focuses on helping organisations and individuals understand and comply with data protection laws. I work for a small outfit out of Dublin, Ireland, [Castlebridge](castlebridge.ie).

I've been in this space for a few years now, and like to think I know my way around data protection and privacy. That said, I am a consultant, and not a lawyer. **This is not, and should never be considered legal advice**. More like a helpful guide. A few caveats:
- I do not know all the laws around the world. Therefore, if you're outside of the EU/US, your local laws are controlling.
- This is designed to offer general guidance, not specific legal advice. If you've got some sort of exotic arrangement on your federated service, best to talk to someone about it, and not just trust some rando on the internet.
- I try to cite to sources as much as possible. But you should, when practical, also read those sources and not just trust some rando on the internet.
- Unlike computer code, the law is rarely binary in nature. Things change, scope expands, and situations vary.
- I don't care what the #absolutists say, no one will ever be 100% compliant with data protection/privacy laws, no matter what they claim.

> **Gonna say this again: I am not a lawyer. I am not your lawyer. This is not legal advice**.

## Scope

This guide is primarily targeted at Mastodon instance owners (the folks who run the instances) and users, but much of the elements will likely also apply to other #fediverse services (Matrix, Misskey, Pleroma, Pixelfeed, etc.)

I am primarily considering this from the EU POV, so the citations will heavily skew towards the EU and UK General Data Protection Regulation [GDPR] (gdpr-info.eu) [^1]. 

The GDPR has what's known in legal dork circles as wide subject matter & territorial scope (Art. 1, Art. 3), and so it generally applies to a wider class of people and organisations than do other laws (like the California Consumer Privacy Act, or China's Personal Information Protection Law (PIPL)).

That said, if there's interest, I'll attempt to supplement this document for territorial-specific rules (maybe).

## But I Thought the GDPR (etc) Doesn't Apply to Me
As I mentioned in the Scope section, I hate to say it, but it probably does. Let me explain.

[Article 1 GDPR](https://gdpr-info.eu/art-1-gdpr/) defines the subject matter of what's protected under the GDPR. Namely:
> This Regulation lays down rules relating to the protection of **natural persons** with regard to the **processing** of **personal data** and rules relating to the free movement of personal data.

> This Regulation protects fundamental rights and freedoms of natural persons and in particular their right to the protection of personal data.

All of that is legal speak for the fact that the law covers how data is handled (processed) when it concerns people (data subjects or natural persons). 

[Article 3 GDPR](https://gdpr-info.eu/art-3-gdpr/) clarifies that the regulation applies 
> ... to the processing of personal data in the context of the activities of an **establishment** of a **controller or a processor** in the Union, regardless of whether the processing takes place in the Union or not.

> This Regulation applies to the processing of personal data of **data subjects who are in the Union** by a **controller or processor not established in the Union**, where the processing activities are related to:
> - the offering of goods or services, **irrespective of whether a payment of the data subject is required**, to such data subjects in the Union; or
> - the **monitoring of their behaviour** as far as their behaviour takes place within the Union.

## Some Relevant Definitions 
[^2] 

### Isn't Personal Data Just PII and Stuff? I'm not Collecting Any of That... 
Unlike in the States, 'personal data' has a very broad definition: 
> Personal Data means **any information relating to an identified or identifiable natural person (‘data subject’)**; an identifiable natural person is one who can be identified, **directly or indirectly**, in particular by reference to an identifier such as a name, an identification number, location data, an online identifier or to one or more factors specific to the physical, physiological, genetic, mental, economic, cultural or social identity of that natural person. 

The key words here are bolded. Anything that identifies, or can identify (with other information) a person. A natural person is fancy lawyer for "a living, breathing human being".[^3]

Identifiability is a core concept in the GDPR, and is quite broad. There are cases that note that even something like a dynamic IP address, Google Analytics IDs, or information about a person's spouse constitutes identifiable informaiton about a data subject.[^4] 

People share a lot in toots - details about their sexuality and sexual orientation, health information (including mental and physical health), contact information, others they associate with, their dates of birth, photos... 

### WTF is Processing?
First thing's first, let's talk about processing. Processing is a broad term, and it means 
> any operation or set of operations which is performed on personal data or on sets of personal data, whether or not by automated means, such as collection, recording, organisation, structuring, storage, adaptation or alteration, retrieval, consultation, use, disclosure by transmission, dissemination or otherwise making available, alignment or combination, restriction, erasure or destruction. 

In plain English: If you are doing something with personal data, on a computer (or physically), you're probably processing data in some way. Common Fedi examples of processing: 
1. Your instance **collects and stores** user information such as email address, userid, IP address, biographical information, photos of data subjects, etc.
2. Your instance **collects and stores** all those lovely toots. 
3. Based on how Mastodon works, your servers automatically perform various operations on the data (checking for spam, blocking Nazis) (**restricting data, erasing data**)
4. By virtue of being #federated, your instance **shares, transmits, and disseminates** personal data about users and their toots with _other_ users across systems (unless you defederate from everyone).
5. By allowing users to Boost, comment on, bookmark or link to profiles or toots your instance is **adapting and altering** that personal information. 
6. If you've got any sort of analytics going (Google Analytics, or even Matomo), you're likely **collecting, storing, transmitting, and using** device IDs, advertising IDs, IP information and other details about visitors to your site. 
7. By storing any of this on a server somewhere that's not your own, you're also **transmitting** personal data.

So yeah, you're 100% absolutely processing data. Sorry champ. 

### Controllers & Processors, how do they work?
Controllers and processors have legal meaning. A controller is: 
> ... the **natural or legal person**, public authority, agency or other body which, alone or jointly with others, determines the **purposes and means of the processing** of personal data; where the purposes and means of such processing are determined by Union or Member State law, the controller or the specific criteria for its nomination may be provided for by Union or Member State law. 

A processor is: 
> ... a natural or legal person, public authority, agency or other body which processes personal data on behalf of the controller.

Processors basically act on the instructions of the controllers. 

So, a few important notes: 1) This doesn't just apply to big behemoth companies. **Anyone** can be a controller and/or a processor (in fact, there's caselaw on this). 2) the concept of determining 'purposes and means' (controllership) is complicated, but in essence, a way to think about it is the person/body who is making decisions about how data is being used/stored/collected/shared/disseminated etc. 3) Controllership can be shared - it's not always a 100% decisionmaking thing. So, if you run an instance, you make decisions about how personal data is processed for your instance, but other instance admins make similar processing decisions for their instance,  that would make you both controllers (along with the users!). 

#### A few examples  
1. You decided that you wanted to run a Mastodon instance and allow random users to join.[^5] 
2. You decide what servers you want to federate with or defederate from. That is making a decision about the means of processing. 
3. You set policy that requires individuals provide additional information about themselves (e.g., an LGBTQ+ site requiring disclosure of sexual orientation, a country-based site requiring informaiton on where a person is based)
4. You derive something from the data maintained on your instance

### What's a Data Subject? 
As noted above, a data subject or natural person is a human being, usually one who's still alive who is based in the EU. Even if the person is just visiting the EU, or isn't an EU citizen, that still counts. In the US, the CCPA and various other CCPA-like laws apply similar definitions to data subjects based in those respective jurisdictions.  

### What the hell does Establishment mean? 
Establishment is fancy legal term for where your organisation is based in the world. Or in scary legal, it's where you say you're operating from such that a court could sue you there. 

### Monitoring Behavior? 
Generally, this isn't applicable yet, so I'll update this when Mastodon gets exciting. 


## Okay. The Data Protection Laws May Apply. Now What?
### It's way more than just a privacy policy ... 

Things to Consider

#### 1. General obligations of controllers
As a controller you've got many obligations, beyond just the standard boilerplate Mastodon Privacy Notice. Controllers must, at a minimum you need to ensure that adequate 'technical and organisational' measures are in place to meet obligations under the GDPR. In simple terms, that means things like
  - securing data in transit and at rest; 
  - ensuring that access controls and authorisation are strong (strong passwords, limits on access by others to personal data of your users);
  - appropriate auditing and logging of data; 
  - limiting (to the extent possible) how long data is retained and stored on your system and for how long. This is especially true for things like IP addresses, deleted content/accounts, cookie data, etc. Essentially, if you don't need to keep it, treat it like hazardous waste and destroy it; [^6]
  - clearly defining what types of data you collect about your users and why you collect it; 
  - identifying the legal grounds for collecting this data (arguably, consent of users who register on your site, or potentially compliance with contractual obligations [See: [Article 6(2)](https://gdpr-info.eu/art-6-gdpr/)];
  - providing details about who you are as the controller, including some contact information and categories of services you're sharing data with; 
  - setting up processes to handle Data Subject Requests (including access, rectification, deletion and objections to processing) under Articles 15-22; 
  - setting up processes to handle and respond to data breaches under Articles 33-34, including notifying relevant regulators and data subjects;
  - setting up appropriate policies and procedures for complying; 
  - ensuring that contracts are in place when transferring data (for example, if you host on AWS, GCP, Azure, etc.);
  - ensuring confidentiality, availability, integrity, and even resiliency of data. 
  
#### 2. Data residency/sovereignity issues
Depending on where you're based (or where the server is hosted) you may have data residency/localisation or sovereignity requirements. Essentially, you may be governed by your own country's laws, which may include strict obligations to store information in that country (if targeting users of that country), or to permit government access. 
#### 3. Using your instance for legally dubious purposes
If your instance is large, engages in activities that are legally suspect in your jurisdiction (loli, CSAM, drugs, warez, terrorism, etc.), you may need to think about how you will respond to a government request for data about individuals on your service. This is pretty unlikely for small services, but it absolutely is a concern that shouldn't be wholly overlooked.[^7]  The bigger you get, and the more users you have, the greater the risk. 
#### 4. Data portability & automated deletion of posts
Interestingly, two data subject rights, portability and deletion, are already built into the Mastodon protocol, at least with regard to moving profiles and allowing users to set up deletion of posts in an automated fashion, so you can tick that box. Honestly, these features are delightful, and I wish the other sites incorporated this by default.  
#### 5. Building in privacy-enhancing tech
Advanced users (or the designers of the Mastodon Protocol/ActivityPub) should seriously evaluate and work towards E2EE systems, particularly with regard to individual and group DMs. It will solve many problems, and actually elevate Mastodon from a privacy-preserving perspective. 

For an idea of what a good privacy notice looks like with regard to Mastodon instances, you might consider the [EDPS' Privacy Notice](https://social.network.europa.eu/terms). I wouldn't advise relying on it entirely, as much of this will be unique to the EDPS, but it is a good starting point![^8] 

## Some Parting Thoughts
There's still a lot of unknowns when it comes to Mastodon, and a degdree of regulatory uncertainty. Even just focusing on the EU, given that **regulators** like the [European Data Protection Supervisor](https://social.network.europa.eu/about) and the [Bavarian Data Protection Authority](https://social.bund.de/@BayLfD) are _both_ running Mastodon instances, operators should take some comfort that the big regulatory hammers are unlikely to fall ... at least for now. The point of this guide is to help operators and admins _think_ about data protection and privacy concerns, help the community improve on what's in place, and build a thriving, privacy-preserving system together. 

Unlike FB or Twitter, or any of the centralized services, the law will need to figure out just how far 'consent' can be used as a legal basis. As Jon Watson notes on [his blog](https://jonwatson.substack.com/p/privacy-and-the-fediverse-aka-mastodon-pleroma-and-friendicas-a0eca1f5301c), 
> _my public posts on HT go to thousands of other instances that I do not have a relationship with. I don’t have any agreement with the admins of those other instances and those admins aren’t required to uphold the terms of service of the instance I do have a relationship with, Hackers.Town._

Users have no real way to ensure that their personal toots/bio information aren't being shared with dodgy instance operators who don't care about privacy or user rights. Some of this may end up falling back on the instance admins (as controllers) to take action against abuses. 

No individual instance admin can solve this problem, unless they defederate, but then, one asks what's the point of a fediverse that isn't connected? It's going to take a lot of community development and dialogue, and user awareness to square this circle.



[^1]: gdpr-info.eu is a very helpful source, but it is not the authoritative source. That said, I use it a lot because it's MUCH cleaner and easier to search compared to the legislation itself, which can be found here: [REGULATION (EU) 2016/679 OF THE EUROPEAN PARLIAMENT AND OF THE COUNCIL](https://eur-lex.europa.eu/eli/reg/2016/679/oj). 
[^2]: Most GDPR definitions can be found under Article 4: [Article 4](https://gdpr-info.eu/art-4-gdpr/)
[^3]: But because lawyers, not always! Sometimes, dead people also have data protection rights, whee!!!
[^4]: CJEU, C‑582/14 Patrick Breyer (19.10.2016) (Dynamic IP address); CJEU, C‑184/20 - Vyriausioji Tarnybinės Etikos Komisija (01.08.2022) (sexual orientation inferred from disclosure of inhabitant/spouse/partner); Garante per la protezione dei dati personali (Italy) (Case No. 
9668051, 09.06.2022) (Google Analytics) [There are a half dozen of these cases].
[^5]: I highlight this here for one reason: Under the GDPR and other laws, there are usually exceptions for purely 'household or personal use'. It's why email providers like Protonmail are generally considered processors, but not controllers and the data they manage -- and the controller is the data subject [Recital 18](https://gdpr-info.eu/recitals/no-18/). They're not generally making decisions on the means and purposes of processing -- that's being decided by the data subjects themelves. By contrast, Google does all sorts of stuff with email - all that smart suggestions nonsense, and targeted advertising, and bubbling up helpful suggestions. They're firmly in the controller camp. 

Based on the kinds of leeway that admins of instances get, and some recent decisions, notably the 2019 _Fashion ID_ case (CJEU, C-40/17 - Fashion ID), instance admins are _likely_ to be seen as controllers or at least joint controllers.  

[^6]: I absolutely get that this is hard, because once data is shared with other servers/users, it may be effectively impossible to claw it back or delete it. This will require further development in the standard in order to effectively implement deletion.  //**note** I would love some additional insights here on the standards to provide a better set of suggestions. 
[^7] Given how some locations like the US are turning ever hostile towards the rights of LGBTQ+ people and women seeking reproductive care, these concerns may also extend if you're operating a support/dating network, or an 'underground railroad' type service for women seeking reproductive care. 
[^8]: For example, they rely on a law that is totally inapplicable to non-EU institutions and government bodies (https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv:OJ.L_.2018.295.01.0039.01.ENG&toc=OJ:L:2018:295:TOC) 
