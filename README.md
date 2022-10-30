# MastodonPrivacyGuide

A guide on data protection obligations, challenges & pitfalls for Mastodon Users & Instance owners / admins.

[MastodonPrivacyGuide](#mastodonprivacyguide)
  * [Who Are You and Why Should I Trust You?](#who-are-you-and-why-should-i-trust-you)
  * [Scope](#scope)
  * [But I Thought the GDPR (etc) Doesn't Apply to Me](#but-i-thought-the-gdpr--etc--doesn-t-apply-to-me)
    + [What is Processing?](#what-is-processing)
  * [Well This is Harshing My Mellow](#well-this-is-harshing-my-mellow)
  * [Fine, Fine. The Data Protection Laws May Apply. Now What?](#fine--fine-the-data-protection-laws-may-apply-now-what)
  * 
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

## Some Relevant Definitions [^2] 

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

So, a few important notes: 1) This doesn't just apply to big behemoth companies. Anyone can be a controller (in fact, there's caselaw on this). 2) the concept of determining 'purposes and means' is complicated, but in essence, a way to think about it is the person/body who is making decisions about how data is being used/stored/collected/shared/disseminated etc. 

**A few examples**: 
1. You decided that you wanted to run a Mastodon instance and allow random users to join.[^5] 
2. You decide what servers you want to federate with or defederate. That is making a decision about the means of processing. 
3. 

### What's a Data Subject? 
As noted above, a data subject or natural person is a human being, usually one who's still alive who is based in the EU. Even if the person is just visiting the EU, or isn't an EU citizen, that still counts. In the US, the CCPA and various other CCPA-like laws apply similar definitions to data subjects based in those respective jurisdictions.  

### What the hell does Establishment mean? 

### Monitoring Behavior? 




## Well This is Harshing My Mellow

## Fine, Fine. The Data Protection Laws May Apply. Now What?
Things to Consider
1. It's more than just a Privacy Notice/Policy
2.

[^1]: gdpr-info.eu is a very helpful source, but it is not the authoritative source. That said, I use it a lot because it's MUCH cleaner and easier to search compared to the legislation itself, which can be found here: [REGULATION (EU) 2016/679 OF THE EUROPEAN PARLIAMENT AND OF THE COUNCIL](https://eur-lex.europa.eu/eli/reg/2016/679/oj). 
[^2]: Most GDPR definitions can be found under Article 4: [Article 4](https://gdpr-info.eu/art-4-gdpr/)
[^3]: But because lawyers, not always! Sometimes, dead people also have data protection rights, whee!!!
[^4]: CJEU, C‑582/14 Patrick Breyer (19.10.2016) (Dynamic IP address); CJEU, C‑184/20 - Vyriausioji Tarnybinės Etikos Komisija (01.08.2022) (sexual orientation inferred from disclosure of inhabitant/spouse/partner); Garante per la protezione dei dati personali (Italy) (Case No. 
9668051, 09.06.2022) (Google Analytics) [There are a half dozen of these cases].
[^5]: I highlight this here for one reason: Under the GDPR and other laws, there are usually exceptions for purely 'household or personal use'. It's why email providers like Protonmail are generally considered processors, but not controllers and the data they manage -- and the controller is the data subject [Recital 18](https://gdpr-info.eu/recitals/no-18/). They're not generally making decisions on the means and purposes of processing -- that's being decided by the data subjects themelves. By contrast, Google does all sorts of stuff with email - all that smart suggestions nonsense, and targeted advertising, and bubbling up helpful suggestions. They're firmly in the controller camp. 

Based on the kinds of leeway that admins of instances get, and some recent decisions, notably the 2019 _Fashion ID_ case (CJEU, C-40/17 - Fashion ID), instance admins are _likely_ to be seen as controllers or at least joint controllers.  
