
# Mastodon Privacy Guide V.1.0

  

A guide on data protection obligations, challenges & pitfalls for Mastodon Users & Instance owners / Admins.

  


<!-- TOC start -->
  * [Who Are You and Why Should I Trust You?](#who-are-you-and-why-should-i-trust-you)
  * [Scope](#scope)
  * [But I Thought the GDPR Doesn't Apply to Me?](#but-i-thought-the-gdpr-doesnt-apply-to-me)
  * ['Doing Stuff With Data'  ](#doing-stuff-with-data)
  * [Okay. The Data Protection Laws May Apply. Now What?](#okay-the-data-protection-laws-may-apply-now-what)
      - [1. General obligations of controllers](#1-general-obligations-of-controllers)
      - [2. Data residency/sovereignity issues](#2-data-residencysovereignity-issues)
      - [3. Using your instance for legally dubious purposes](#3-using-your-instance-for-legally-dubious-purposes)
      - [4. Building in privacy-enhancing tech](#4-building-in-privacy-enhancing-tech)
  * [Some Parting Thoughts](#some-parting-thoughts)
  * [About Me (expanded)](#about-me-expanded)
- [APPENDIX (Definitions, Details, Footnotes, etc.)](#appendix-definitions-details-footnotes-etc)
  * [Territoriality of the GDPR](#territoriality-of-the-gdpr)
  * [Some Relevant Definitions ](#some-relevant-definitions)
    + [Personal Data v. PII](#personal-data-v.-pii)
    + [Processing Data](#processing-data)
    + [Controllers & Processors](#controllers--processors)
    + [What's a Data Subject?](#whats-a-data-subject)
    + [What are Data Subject Rights?](#what-are-data-subject-rights)
    + [What is Lawful Basis](#what-is-lawful-basis)
    + [Responding to Data Breaches](#responding-to-data-breaches)
    + [What Does Establishment Mean?](#what-does-establishment-mean)
    + [Monitoring Behavior?](#monitoring-behavior)
  * [Footnotes](#footnotes)
<!-- TOC end -->

  

## Who Are You and Why Should I Trust You?

My name is Carey [@privacat@dataprotection.social](https://dataprotection.social/@privacat#) and I am an External Data Protection Consultant, researcher, and Mastodon instance admin (I run [dataprotection.social](https://dataprotection.social).

In my I also work for a small consulting outfit out of Dublin, Ireland, [Castlebridge](https://castlebridge.ie). You can read more about my background [here](#about-me-expanded).


## Scope

 
This is both a guide & a general backgrounder/crash course on data protection law. 

It's primarily targeted at Mastodon instance owners and admins, but much of the elements will likely also apply to other #Fediverse services (Misskey, Pleroma, Pixelfeed, etc.)

I am primarily considering this from the EU POV, so the citations will heavily skew towards the EU and UK General Data Protection Regulation [GDPR] (https://gdpr-info.eu) [^1]. That said, if there's interest, I'll attempt to supplement this document for territorial-specific rules. **If you have knowledge / insight on additional laws to consider, please file an issue, or ping me directly at admin @ dataprotection.social.**
  

## But I Thought the GDPR Doesn't Apply to Me?

Despite the various rumors and wishful thinking out there, for many instance operators, the GDPR probably does apply.

**Tl;Dr** the GDPR (and many data protection laws around the world) have wide [reach and applicability](#territoriality-of-the-gdpr), and apply to a whole host of activities and types of information sharing that don't always seem obvious.

Essentially, the GDPR applies to you if:
1. You are making decisions to **do stuff** (or [processing](#processing-data)) data; *and*
2. The data you're doing stuff with directly identifies or, with other information, can make a **person** (the law calls people '[data subjects](#whats-a-data-subject)').  This kind of data is referred to as '[personal data](#personal-data-v.-pii)' in the EU, and 'personally identifiable information' in the US; *and*
3. The people whose data you're doing stuff with are based in the EU or your processing is occuring in the EU.


## 'Doing Stuff With Data'  

**In plain English:** If you are collecting, using, storing, sharing, transferring, selling, or generally making use of personal data on a computer (or physically), you're probably 'doing stuff' with data in some way. So, legally speaking, you're [processing data](#processing-data).

Common Fedi examples of processing include:

- Your instance **collects and stores** user information such as email address, userid, IP address, biographical information, photos of data subjects, followers/following information, etc. of your instance's users and others interacting with users of your instance. 
- Your instance **collects and stores** posts, likes, boosts, bookmarks to posts, etc.
- Based on how Mastodon works, your servers automatically perform various operations on the data (checking for spam, logging access attempts and posting information) (**restricting data, storing data**)
- A user **deletes** their account/posts, or sets up the auto-delete function (**erasing or destroying data**)
- By virtue of being #federated, your instance **shares, transmits, and disseminates** personal data about users and their posts, likes, and boosts with _other_ users across systems (unless you defederate from everyone).
- By allowing users to boost, comment on, or link to profiles or posts, your instance is **adapting and altering** that personal information.
- If you've got any sort of analytics going (Google Analytics, or even Matomo), you're likely **collecting, storing, transmitting, and using** device IDs, advertising IDs, IP information and other details about visitors to your site.
- By storing any of this on a server somewhere that's not your own, you're also **transmitting** personal data.
- When users send you an email (for example, if they sign up for an account or they make a data subject request), you're **collecting, storing, transmitting, and using** their email address, potentially their name, their userid, and whatever other information they provide to you.

## Okay. The Data Protection Laws May Apply. Now What?

Things to Consider as a #fediverse Admin:

#### 1. General obligations of controllers

As a [controller](#controllers--processors) you've got many obligations, beyond just the standard boilerplate Mastodon Privacy notice (which you should not use, as it's exceedingly generic). Controllers must, at a minimum ensure that adequate 'technical and organisational' measures are in place to meet obligations under the GDPR. In simple terms, that means things like:

- securing [personal data](#personal-data-v.-pii) in transit and at rest;

- ensuring that access controls and authorisation are strong (strong passwords, limits on access by others to personal data of your users, using multi-factor / two-factor authentication);
- having appropriate auditing and logging of data on your systems;
- limiting (to the extent possible) what data is retained and stored on your system (including in logs) and for how long. This is especially true for things like IP addresses, userids and deleted content/accounts in backup, which are considered **personal data.** Essentially, if you don't need to keep it, treat it like hazardous waste and destroy it; [^2]

Examples here might including setting shorter log retention limits and changing your nginx-configuration scripts to block the collection of data. For example, this set of recommendations by [@chpietsch@digitalcourage.social](https://digitalcourage.social/@chpietsch/109375489065179800) 
				#proxy_set_header X-Real-IP $remote_addr; 
				#proxy_set_header X-Forwarded-For 
				# $proxy_add_x_forwarded_for
				
- clearly defining what types of personal data you collect about your users and why you collect it (and spelling this out in the Privacy Notice);
- identifying the legal grounds for collecting this data. This is referred to as the [lawful basis](#lawful-basis);
- identifying yourself as the controller, including providing contact information, as well as the reasons (purposes), categories of data collected, and [third party processors](#controllers--processors) you share data with. Examples here include your cloud hosting provider, CDN, and email provider;
- setting up processes to handle [people demanding things of you](#what-are-data-subject-rights), namely access to their data, correcting data if it's inaccurate, deletion, and objections to processing;
- setting up processes to handle and respond to [data breaches](#responding-to-data-breaches);
- setting up appropriate policies and procedures for complying with the law;
- ensuring that contracts are in place when transferring data to third parties (for example, if you host on AWS, GCP, Azure, Infomaniak, or even a bespoke Mastodon hosting provider like masto.host);
- ensuring confidentiality, availability, integrity, and even resiliency of data are considered in your processes.

#### 2. Data residency/sovereignity issues

Depending on where you're based (or where your #fedi server is hosted) you may have data residency/localisation or sovereignity requirements. Essentially, you may be limited by your own country's laws, which may include strict obligations to store information in that country (if targeting users of that country), or to permit government access.

As a separate concern, while the EU does not have strict requirements to host data in the EU/EEA, the GDPR adds a whole load of complexity if you process data about EU data subjects outside of the EU *or another adequate country*. **See**: [But I Thought the GDPR Doesn't Apply to Me](#but-i-thought-the-gdpr-doesnt-apply-to-me). [^3]

#### 3. Using your instance for legally dubious purposes

If your instance is large, engages in activities that are legally suspect in your jurisdiction (loli, child sexual abuse material (CSAM), drugs, warez, terrorism, etc.), you may need to think about how you will respond to a government request for data about users of your service. This is pretty unlikely for small instances, but it absolutely is a concern that shouldn't be wholly overlooked.[^4] 

Additionally, many folks have pointed out that issues around copyright infringement and dealing with CSAM may imperil instance admins. Recommendations include
* Registering and filing a notice with the [US Copyright Office](https://www.copyright.gov/dmca-directory/)
* Registering with the National Center for Missing and Exploited Children or your government's own version handling such issues to report abusive images. 
The bigger you get, and the more users you have, the greater the risk. Other guidance can be found [here](https://nitter.net/rahaeli/status/1593819064161665024).

#### 4. Building in privacy-enhancing tech

Advanced users (or the designers of the Mastodon Protocol / ActivityPub) should seriously evaluate and work towards E2EE systems, particularly with regard to individual and group DMs, as well as other privacy-enhancing technologies. It will solve many problems, and actually elevate Mastodon from a privacy-preserving perspective.
A few examples: 
* https://github.com/mastodon/mastodon/pull/13820 [E2EE proposal]
* https://github.com/mastodon/mastodon/issues/6474 [Limit storage of full IPs]
* Default encryption-at-rest of database instances
* An update to Mastodon's abysmal privacy notice

**Heads up**: For an idea of what a good privacy notice looks like with regard to Mastodon instances, you might consider the [EDPS' Privacy Notice](https://social.network.europa.eu/terms) or check out the [template I put together](https://codeberg.org/privacat/MastodonDataProtectionGuidance/src/branch/main/PrivacyNoticeTemplate.md). Use these as **guides** and don't just copypasta them into your instance! [^5]
* Future considerations around the [Digital Services Act](https://digitalpolicy.ie/the-digital-services-act-package-a-primer/) and Chat Control laws that have passed (or are being considered) in Europe, existing laws such as [Privacy and Electronic Communications Regulations](https://ico.org.uk/for-organisations/guide-to-pecr/what-are-pecr/) and the [ePrivacy Directive](https://edps.europa.eu/data-protection/our-work/publications/legislation/directive-2009136ec_en), as well as similar laws around the world. Also, the absolute insanity of [Texas and Florida's](https://techcrunch.com/2022/05/31/texas-social-media-law-supreme-court-hb20) 'social media viewpoint' laws.  
 
## Some Parting Thoughts

There's still a lot of unknowns when it comes to Mastodon, and a degree of regulatory uncertainty. Even just focusing on the EU, given that **regulators** like the [European Data Protection Supervisor](https://social.network.europa.eu/about) and the [Bavarian Data Protection Authority](https://social.bund.de/@BayLfD) are _both_ running Mastodon instances, operators should take some comfort that the big regulatory hammers are unlikely to fall ... at least for now. The point of this guide is to help operators and admins _think_ about data protection and privacy concerns, help the community improve on what's in place, and build a thriving, privacy-preserving system together.

Unlike FB or Twitter, or any of the centralized services, the law will need to figure out just how far 'consent' can be used as a [legal basis](#lawful-basis-how-does-it-work).[^6]

Users have no real way to ensure that their personal toots/bio information aren't being shared with dodgy instance operators or data brokers who don't care about privacy or user rights. Some of this may end up falling back on the instance admins (as controllers) to take action against abuses.

No individual instance admin can solve this problem, unless they defederate, but then, one asks what's the point of a fediverse that isn't connected? It's going to take a lot of community development and dialogue, and user awareness to square this circle.


## About Me (expanded)

Aside from being a person on the internet who is extremely online, I've been in this space for a few years now, and like to think I know my way around data protection and privacy. That said, I am a consultant, not a lawyer. **This is not, and should never be considered legal advice**. More like a helpful guide. 

A few caveats:
- I do not know all the laws around the world. Therefore, if you're outside of the EU/US, your local laws are controlling.
- This is designed to offer general guidance, not specific legal advice. If you've got some sort of exotic arrangement on your federated service, best to talk to a professional lawyer or solicitor about it, and not just trust some random person on the internet.
- I try to cite to sources as much as possible. But you should, when practical, also read those sources and not just trust me. 
- Unlike computer code, the law is rarely binary in nature. Things change, scope expands, and situations vary.
- I don't care what the #absolutists say, no one will ever be 100% compliant with data protection/privacy laws, no matter what they claim.
>  **Gonna say this again: I am not a lawyer. I am not your lawyer. This is not legal advice**.


# APPENDIX (Definitions, Details, Footnotes, etc.)


## Territoriality of the GDPR

The GDPR has what's known in legal circles as wide subject matter & territorial scope (Art. 1, Art. 3), and so it generally applies to a wider class of people and organisations than do other laws (like the California Consumer Privacy Act, or China's Personal Information Protection Law (PIPL)).

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

### Personal Data v. PII

Unlike in the States, 'personal data' has a very broad definition  [^7]:

> Personal Data means **any information relating to an identified or identifiable natural person (‘data subject’)**; an identifiable natural person is one who can be identified, **directly or indirectly**, in particular by reference to an identifier such as a name, an identification number, location data, an online identifier or to one or more factors specific to the physical, physiological, genetic, mental, economic, cultural or social identity of that natural person.

The key words here are in bold. Anything that identifies, or can identify (with other information) a person. A natural person is a legal term of art for "a living, breathing human being".[^8]

*Identifiability* is a core concept in the GDPR, and is quite broad. There are cases that note that even something like a dynamic IP address, Google Analytics IDs, or information about a person's spouse constitutes identifiable information about a data subject.[^9]

People share a lot in their posts and bios - details about their sexuality and sexual orientation, health information (including mental and physical health), contact information, others they associate with, their dates of birth, photos...

### Processing Data

First thing's first, let's talk about processing. Processing is a broad term. Under the GDPR it is defined as:

> any operation or set of operations which is performed on personal data or on sets of personal data, whether or not by automated means, such as collection, recording, organisation, structuring, storage, adaptation or alteration, retrieval, consultation, use, disclosure by transmission, dissemination or otherwise making available, alignment or combination, restriction, erasure or destruction. 

If you skipped to this section you can see common [fedi examples here](#doing-stuff-with-data).

### Controllers & Processors

Controllers and processors have legal meaning. A controller is:

> ... the **natural or legal person**, public authority, agency or other body which, alone or jointly with others, determines the **purposes and means of the processing** of personal data; where the purposes and means of such processing are determined by Union or Member State law, the controller or the specific criteria for its nomination may be provided for by Union or Member State law.
 
#### Heads Up: 
In most contexts, a data subject interacting with you (signing up, using your instance) is not considered a 'controller' of data, because they are not defining the 'means' (the how) and the 'purposes' (the why) of how their data is being used.[^10]

A processor is:

> ... a natural or legal person, public authority, agency or other body which processes personal data on behalf of the controller.  

Processors basically act on the instructions of the controllers.  As I noted above, because data subjects themselves are not considered controllers in most cases, so you are not acting as a *processor* on their behalf.

A few important notes:

1. This doesn't just apply to big behemoth companies. **Anyone** can be a controller and/or a processor (in fact, there's caselaw on this).
2. The concept of determining 'purposes and means' (controllership) is complicated, but in essence, a way to think about it is the person/body who is making decisions about **why** and **how** data is being processed. 
3. Controllership can be shared by multiple parties - it's not always a unitary decision-making process. If you run an instance, you make decisions about how personal data is processed for your instance, but you're also working with a third party regarding sharing of data, that may make you both controllers. I have cited the caselaw discussing this in FN 10. 

### What's a Data Subject?

As noted above, a data subject or natural person is a human being (usually one who's still alive), who is based in the EU. Even if the person is just visiting the EU, or isn't an EU citizen, that still counts. In the US, the CCPA and various other CCPA-like laws apply similar definitions to data subjects based in those respective jurisdictions.

### What are Data Subject Rights?

The GDPR (and CCPA and other laws) identify a number of rights data subjects have when it comes to their data. These include:

1. **Access** (aka, 'Download my Data'), but this also includes details about how the controller uses their data, who the controller shares the data with, details on retention, and other details. [Article 15](https://gdpr-info.eu/art-15-gdpr/)

2. **Correction** (or 'Rectification') of incorrect or inaccurate informational data. [Article 16](https://gdpr-info.eu/art-16-gdpr/)

3. **Erasure** (sometimes known as the 'Right to be Forgotten') of data in certain cases [Article 17](https://gdpr-info.eu/art-17-gdpr/). To a limited extent, deletion is already somewhat built into Mastodon (users can automatically have posts deleted, or they can delete their account), but it's not fool-proof. There are still challenges around logs, and of course, the federated nature of the system. 

4. **Restrictions on processing** This has some limits, and it generally applies when the controller is doing things with personal data and not being upfront with data subjects. [Article 18](https://gdpr-info.eu/art-18-gdpr/) This is why a good [privacy notice](https://codeberg.org/privacat/MastodonDataProtectionGuidance/src/branch/main/PrivacyNoticeTemplate.md) is so important. 

5. **Data Portability** This means sharing personal data with the data subject in a 'structured, commonly used and machine-readable format' that can be shared elsewhere. Interestingly, this right is already baked into the Mastodon protocol, at least with regard to exporting and moving profiles. [Article 20](https://gdpr-info.eu/art-20-gdpr/)

6. **Objection** Depending on the lawful basis you go with (notably, legitimate interests), or if you're processing data like cookies for marketing purposes, a data subject can object to that processing and you've gotta stop doing it. You can't kick them off the service either. [Article 21](https://gdpr-info.eu/art-21-gdpr/)

7. **Objection to automated decision-making, incluidng profiling** People have a right not to be subject to a decision about them that could produce a legal or other significant effect about them. [Article 22](https://gdpr-info.eu/art-22-gdpr/)

Data subject rights require that you communicate with the data subject in a timely manner (usually one month), and you can't generally object just because it's time-consuming or difficult. Many of these laws are carried over to other legal systems, including the CCPA and the new Virgina Data Protection laws. 

###  What is Lawful Basis

There are a number of lawful bases (basically, legally-acceptable reasons) for processing data. These are spelled out in [Article 6(1) GDPR](https://gdpr-info.eu/art-6-gdpr/)]. Examples a., b., and f. are most applicable here. You'll never get away with d. or e.

a. the data subject has given **consent** to the processing of his or her personal data for one or more specific purposes;
b. processing is necessary for the performance of a **contract to which the data subject** is party or in order to take steps at the request of the data subject prior to entering into a contract;
c. processing is necessary for compliance with a **legal obligation** to which the controller is subject
d. processing is necessary in order to protect the **vital interests** of the data subject or of another natural person;
e. processing is necessary for the performance of a task carried out in the public interest or in the exercise of official authority vested in the controller;
f. processing is necessary for the purposes of the **legitimate interests** pursued by the controller or by a third party, except where such interests are overridden by the interests or fundamental rights and freedoms of the data subject which require protection of personal data, in particular where the data subject is a child.

**A special note on consent:**

> ‘consent’ of the data subject means any **freely given**, **specific**, **informed and unambiguous** indication of the data subject’s wishes by which he or she, by a statement or by a clear affirmative action, signifies agreement to the processing of personal data relating to him or her. [Article 4(11)](https://gdpr-info.eu/art-4-gdpr/) 

Consent is tricky, as it can be revoked at any time, making future processing unlawful (unless you have a separate lawful basis you can rely on). **FYI**: Pre-ticked boxes, omnibus consents, unclear consent, or consent tied to (many) uses of the service do not equal valid consent.

### Responding to Data Breaches

A personal data breach is any breakdown of security leading to the

> accidental or unlawful destruction, loss, alteration, unauthorised disclosure of, or access to, personal data transmitted, stored or otherwise processed.

This isn't limited to external bad actors like hackers. It also involves cases where you forward off personal data to the wrong person, or where your servers go down for a prolonged period of time, or potentially, if you shut down a server entirely without giving data subjects the ability to export their data. 

When a personal data breach occurs, unless the breach is 'unlikely to result in a risk' to data subjects rights and freedoms, you've got to notify a relevant EU regulatory authority [Article 33 GDPR](https://gdpr-info.eu/art-33-gdpr/). In some cases, when the risk is **really high**, you must notify the data subjects as well. [Article 34 GDPR](https://gdpr-info.eu/art-34-gdpr/)

Controllers need to do certain things when responding to a data breach, including notifying the relevant regulators within 72 hours of discovery, taking action to mitigate the breach, and in some cases, letting data subjects/users know if there's a high risk to those users.

### What Does Establishment Mean?

Establishment is fancy legal term for where your organisation is based in the world. The jurisdiction you operate (even if you're just acting as an individual) is where you can be sued. 

### Monitoring Behavior?

Generally, this isn't applicable yet, so I'll update this when Mastodon gets exciting.

## Footnotes


[^1]: gdpr-info.eu is a very helpful source, but it is not the authoritative source. That said, I use it a lot because it's MUCH cleaner and easier to search compared to the legislation itself, which can be found here: [REGULATION (EU) 2016/679 OF THE EUROPEAN PARLIAMENT AND OF THE COUNCIL](https://eur-lex.europa.eu/eli/reg/2016/679/oj).

[^2]: I absolutely get that this is hard, because once data is shared with other servers/users, it may be effectively impossible to delete. In the [Privacy Notice Template for Mastodon Admins](https://codeberg.org/privacat/MastodonDataProtectionGuidance/src/branch/main/PrivacyNoticeTemplate.md), I have made note of this under the 'Retention' section. Full compliance with a deletion request is likely impossible for now within the #Fediverse and either allowances in the law, or changes to the ActivityPub standard will likely need to occur in order to address this gap.

[^3]: I could (and have) spent days on the subject of data transfers, and the additional complexity that controllers and processors face when sharing data outside of the EEA or another adequate country. For what it's worth, the European Commission has recognized the following countries as having adequate data protection laws that are comparable to the GDPR: [Andorra](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32010D0625), [Argentina](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32003D0490), [Canada](https://eur-lex.europa.eu/legal-content/en/TXT/?uri=CELEX%3A32002D0002) (commercial organisations), [Faroe Islands](https://eur-lex.europa.eu/legal-content/en/ALL/?uri=CELEX%3A32010D0146), [Guernsey](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32003D0821), [Israel](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32011D0061), [Isle of Man](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32004D0411), [Japan](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv:OJ.L_.2019.076.01.0001.01.ENG&toc=OJ:L:2019:076:TOC), [Jersey](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32008D0393), [New Zealand](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX%3A32013D0065), [Republic of Korea](https://ec.europa.eu/info/files/decision-adequate-protection-personal-data-republic-korea-annexes_en), [Switzerland](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32000D0518) , the United Kingdom under the [GDPR](https://ec.europa.eu/info/files/decision-adequate-protection-personal-data-united-kingdom-general-data-protection-regulation_en) and the [LED](https://ec.europa.eu/info/files/decision-adequate-protection-personal-data-united-kingdom-law-enforcement-directive_en), and [Uruguay](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32012D0484) as providing adequate protection. 

[^4]: Given how some locations like the US are turning ever hostile towards the rights of LGBTQ+ people and women seeking reproductive care, these concerns may also extend if you're operating a support/dating network, or an 'underground railroad' type service for women seeking abortions, or people seeking transgender care.

[^5]: **Really, don't.** My template has lots of [your.instance] and [bracketed sections for you to fill in]. The EDPS example relies on a [law](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv:OJ.L_.2018.295.01.0039.01.ENG&toc=OJ:L:2018:295:TOC) that is totally inapplicable to non-EU institutions and government bodies. You will look like an absolute clown if you just copypaste.

[^6]: As Jon Watson notes on [his blog](https://jonwatson.substack.com/p/privacy-and-the-fediverse-aka-mastodon-pleroma-and-friendicas-a0eca1f5301c), "my public posts on HT go to thousands of other instances that I do not have a relationship with. I don’t have any agreement with the admins of those other instances and those admins aren’t required to uphold the terms of service of the instance I do have a relationship with, Hackers.Town."

[^7]: Most GDPR definitions can be found under Article 4: [Article 4](https://gdpr-info.eu/art-4-gdpr/)

[^8]: But because lawyers, and individual country exceptions, sometimes, dead people also have data protection rights.

[^9]: CJEU, C‑582/14 [Patrick Breyer](https://curia.europa.eu/juris/document/document.jsf;jsessionid=81A297BFD1E745828E0FC1AE12D58CED?text=&docid=184668&pageIndex=0&doclang=en&mode=lst&dir=&occ=first&part=1&cid=671357) (19.10.2016) (Dynamic IP address); CJEU, C‑184/20 - [Vyriausioji Tarnybinės Etikos Komisija](https://curia.europa.eu/juris/document/document.jsf?text=&docid=263721&pageIndex=0&doclang=en&mode=lst&dir=&occ=first&part=1&cid=671595) (01.08.2022) (sexual orientation inferred from disclosure of inhabitant/spouse/partner); [Garante per la protezione dei dati personali (Italy)](https://gdprhub.eu/index.php?title=Garante_per_la_protezione_dei_dati_personali_%28Italy%29_-_9668051) (Case No. 9668051, 09.06.2022) (Google Analytics, GDPRHub case summary) [There are a half dozen of these cases].

[^10]: I say most cases, because the European Court of Justice have issued decisions in a few cases, determining that a controller (or joint controller) relationship may exist between data subjects and other parties depending on how data is used. For example, in Case C-210/16, [Wirtschaftsakademie Schleswig-Holstein](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=ecli:ECLI:EU:C:2018:388), the Court decided that operators of a Facebook fan page were joint controllers together with Facebook, because they exerted influence over the data that Facebook collected from visitors to their page. In C‑25/17, [Tietosuojavaltuutettu (Jehovah’s Witnesses)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=ecli:ECLI:EU:C:2018:551) case, the Court found that the Jehovah’s Witnesses community was a joint controller (together with the individuals doing door-to-door preaching) of collected data as it determined how the data was collected, organised, & used. Most recently, in C‑40/17, [Fashion ID](https://curia.europa.eu/juris/document/document.jsf?text=&docid=216555&doclang=EN), the Court determined that “a natural or legal person may be a controller, ... jointly with others only in respect of operations involving the processing of personal data for which it determines jointly the purposes and means. By contrast, [...] that natural or legal person cannot be considered to be a controller, within the meaning of that provision, in the context of operations that precede or are subsequent in the overall chain of processing for which that person does not determine either the purposes or the means." 
