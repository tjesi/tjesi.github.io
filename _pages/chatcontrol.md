---
title: "Chat Control"
permalink: /chatcontrol/
description: "An introduction to the EU's proposed Chat Control regulation, why scanning private communication does not work and breaks end-to-end encryption, and a curated list of open letters, reports, research papers, and talks."
---

Since 2022, the European Union has been negotiating a [regulation to prevent and combat child sexual abuse](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A52022PC0209) (the CSA Regulation, or CSAR), widely known as "Chat Control". Protecting children from sexual abuse is a goal we all share. However, the proposal would require or encourage online services to automatically scan the private messages, photos, and videos of all their users, including in end-to-end encrypted services, and to verify the age of their users. Together with hundreds of scientists in security, cryptography, and privacy, I have argued that these measures would not work as intended, and that they would seriously undermine the security and privacy of everyone, including children.

This page gives a short introduction for readers who are new to the topic, explains the main technical objections, and collects the open letters, official assessments, research papers, talks, and websites that I find most useful. The views on this page are my own and do not represent those of NTNU.

*Last updated: September 2026.*

## What is Chat Control?

The European Commission [proposed the regulation](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=celex%3A52022PC0209) in May 2022. Its central element was *detection orders*: national authorities could require providers of messaging, email, cloud, and social media services to search all communication on their platforms for three kinds of content:

- **Known abuse material**, that is, images and videos that have previously been identified by investigators. This is typically done with *perceptual hashing*, which computes a short fingerprint of an image that should stay the same when the image is slightly modified, and compares it to a database of fingerprints of known material.
- **New, previously unknown abuse material**, which requires machine-learning classifiers that try to decide whether an image or video depicts abuse.
- **Grooming**, that is, adults soliciting children, which requires classifiers that analyze the text of private conversations.

Matches would be reported to a new EU Centre on Child Sexual Abuse, which would forward them to law enforcement. The proposal also introduced obligations for providers to assess and mitigate the risk that their services are misused, including by verifying the age of their users.

In end-to-end encrypted services such as Signal, WhatsApp, and iMessage, only the sender and the recipient can read the messages, so the provider cannot scan them on its servers. The only way to comply is *client-side scanning*: software on the user's own phone or computer inspects every message before it is encrypted and reports suspicious content. Chat Control is therefore incompatible with end-to-end encryption, even though the proposal claims to be "technology neutral".

A temporary regulation, [Regulation (EU) 2021/1232](https://eur-lex.europa.eu/eli/reg/2021/1232/oj/eng), often called "Chat Control 1.0", already allows providers such as Meta, Google, and Microsoft to *voluntarily* scan unencrypted communication, as an exception to the EU's ePrivacy rules. The CSA Regulation was meant to replace it with a permanent framework.

## Where does it stand?

| Date | Event |
| --- | --- |
| July 2021 | The interim regulation 2021/1232 on voluntary scanning is adopted. |
| May 2022 | The Commission proposes the CSA Regulation with mandatory detection orders. |
| July 2022 | The [EDPB and EDPS](https://edps.europa.eu/system/files/2022-07/22-07-28_edpb-edps-joint-opinion-csam_en.pdf) warn that the proposal may lead to general and indiscriminate scanning of all communication. |
| April 2023 | The Council's own [legal service](https://www.patrick-breyer.de/wp-content/uploads/2023/05/st08787.en23-leak.pdf) (leaked opinion) and the European Parliament's [complementary impact assessment](https://www.europarl.europa.eu/RegData/etudes/STUD/2023/740248/EPRS_STU(2023)740248_EN.pdf) both find that the detection orders are likely disproportionate. |
| November 2023 | The European Parliament adopts its position: end-to-end encrypted services are excluded, and detection is only allowed against specific suspects with judicial authorization. |
| April 2024 | The interim regulation is extended until April 2026. |
| 2024–2025 | Successive Council presidencies propose new variants, such as "upload moderation" under the Belgian presidency, where users must consent to scanning in order to send images and videos. The Belgian, Hungarian, and Polish proposals do not get a majority. In October 2025, the Danish presidency drops mandatory detection orders from its text. |
| November 2025 | The Council [agrees on its position](https://www.consilium.europa.eu/en/press/press-releases/2025/11/26/child-sexual-abuse-council-reaches-position-on-law-protecting-children-from-online-abuse/): mandatory detection orders are dropped, but "voluntary" scanning is made permanent, and providers get risk-mitigation obligations. |
| December 2025 | Trilogue negotiations between the Parliament, the Council, and the Commission start. |
| April 2026 | After the European Parliament rejects an extension in March, the interim regulation expires on April 3. |
| July 2026 | The interim regulation is reinstated as [Regulation (EU) 2026/1881](https://eur-lex.europa.eu/eli/reg/2026/1881/oj/eng) until April 2028, now explicitly excluding end-to-end encrypted communication. |
| September 2026 | As of this writing, trilogue negotiations on the permanent regulation continue under the Irish presidency. Whether to allow voluntary scanning or only targeted detection is the main open issue. |

See the Council's [timeline](https://www.consilium.europa.eu/en/policies/prevent-child-sexual-abuse/timeline-prevention-of-child-sexual-abuse/) and [Patrick Breyer's page](https://www.patrick-breyer.de/en/posts/chat-control/) for the latest news.

## Why is this a bad idea?

### Detection does not work at scale

Perceptual hashing is not robust against a motivated adversary. Researchers have shown that small, invisible changes to an image are enough to evade detection, and that one can also construct harmless images that match the fingerprint of a targeted image, for example [shortly after Apple's NeuralHash](https://github.com/AsuharietYgvar/AppleNeuralHash2ONNX/issues/1) was published in 2021. Criminals can therefore easily avoid detection, while innocent users can be framed or flooded with false matches.

Detecting new material and grooming is even harder. It relies on machine-learning classifiers, which are inherently susceptible to evasion and have significant error rates. Grooming conversations can look like ordinary conversations between friends or family, and even human experts struggle to distinguish abuse material from consensual exchanges between teenagers. Unlike malware scanning, which targets well-defined threats, there is no precise technical definition of the content to detect.

Applied to billions of messages every day, even a very small error rate leads to an enormous number of false reports. As the [May 2024 letter](https://csa-scientist-open-letter.org/May2024) points out, WhatsApp users alone send 140 billion messages per day. Even if only one in a hundred messages were checked by a detector with a false positive rate of 0.1%, there would be 1.4 million false positives every single day. This is not only a theoretical concern: the [Irish Council for Civil Liberties](https://www.iccl.ie/news/an-garda-siochana-unlawfully-retains-files-on-innocent-people-who-it-has-already-cleared-of-producing-or-sharing-of-child-sex-abuse-material/) reported that out of 4,192 reports the Irish police received from the US hotline NCMEC in 2020, only 409 were actionable, and 471 were not abuse material at all. False reports overwhelm investigators, take resources away from real cases, and expose innocent people, including teenagers, to investigations.

### Scanning breaks end-to-end encryption

End-to-end encryption guarantees that only the intended recipients can read a message. Client-side scanning inspects messages on the device before they are encrypted, and reports the results to a third party. This defeats the purpose of end-to-end encryption, regardless of how strong the encryption itself is. It also creates new, attractive targets: the scanning software, the database of fingerprints, and the reporting channel can all be attacked or abused by criminals and hostile states. This holds for "voluntary" scanning as well: once the content of a message can be reported to the provider, the communication can no longer be considered private.

Encryption protects the communication of journalists, lawyers, doctors, activists, companies, governments, and ordinary citizens, including children themselves. Messaging services such as Signal have stated that they would rather leave the EU than weaken their encryption. In [Podchasov v. Russia](https://ohrh.law.ox.ac.uk/the-ecthr-in-podchasov-v-russia-preserving-encryption-and-denying-backdoors/) (2024), the European Court of Human Rights found that requiring providers to weaken encryption for all users violates the right to privacy.

### Function creep

The scope of the proposal has repeatedly changed, from images and URLs to text and video, and back again. Once scanning infrastructure is deployed on every phone, it is technically trivial to extend it to other types of content, such as terrorism, copyright infringement, or political dissent, and it can be repurposed by less democratic regimes for surveillance and censorship. Researchers have even shown that a perceptual hashing algorithm can be trained with a [hidden second purpose](https://arxiv.org/abs/2306.11924), such as facial recognition, that is hard to detect by inspecting the algorithm.

### Age verification does not solve the problem

Age verification is easy to circumvent with VPNs, borrowed credentials, fake documents, or deepfakes, and it pushes users towards less secure and unregulated services. Age estimation based on AI and biometrics has high error rates and is biased against certain groups. Privacy-preserving age verification with zero-knowledge proofs is possible in principle, but deploying it at internet scale would require a global trust infrastructure that does not exist today, and current deployments collect large amounts of personal data. As [Celi, Den Hartog, and Haddadi](https://brave.com/blog/zkp-age-verification-limits/) explain, zero-knowledge proofs are also not a silver bullet in practice: implementations are complex and have had subtle vulnerabilities, proofs combined with metadata such as the credential issuer can still be used to track users across sites, revocation checks can let issuers observe where credentials are used, and relying on a few trusted issuers concentrates power and excludes the hundreds of millions of people worldwide who lack formal identification. Age verification also excludes people without identity documents or suitable devices, and there is no scientific evidence that banning minors from online services improves their safety or mental health.

### What would help instead

Eradicating abuse material relies on eradicating abuse. The scientists' letters recommend evidence-based measures: education on consent and digital literacy, trauma-informed and easy-to-use reporting mechanisms, better support for victims, and substantially more resources for law enforcement and social services to investigate and prevent abuse.

## Common counterarguments

**"It only looks for known abuse material."** Detection of known material is the least problematic part, but it is still easy to evade by slightly modifying images, and the database of fingerprints cannot be inspected by users, so nobody can verify what is actually being searched for. The Commission's proposal and the Council's position also cover new material and grooming, which require machine-learning classifiers with much higher error rates.

**"Privacy-preserving cryptography can do the scanning without anyone seeing the messages."** As a cryptographer, I find this one the most important to address. Techniques such as homomorphic encryption, multiparty computation, private set intersection, and zero-knowledge proofs can hide the *computation*, but they do not change *what* is computed: a classifier that makes mistakes on plaintext makes exactly the same mistakes on encrypted data, and every match must still be revealed and reported to someone, which is precisely the loss of confidentiality that end-to-end encryption is meant to prevent. The cryptography also hides the database and the detection model from users, which makes the system harder, not easier, to audit. This has been investigated in detail:

- The report [Outside Looking In: Approaches to Content Moderation in End-to-End Encrypted Systems](https://arxiv.org/abs/2202.04617) by Kamara et al. (Center for Democracy and Technology, 2021) surveys the proposed technical approaches and concludes that user reporting and metadata analysis are the approaches most likely to preserve the security and privacy guarantees of end-to-end encryption, while techniques that detect content in encrypted systems undermine them.
- [SoK: Content Moderation for End-to-End Encryption](https://petsymposium.org/popets/2023/popets-2023-0060.pdf) by Scheffler and Mayer (PETS 2023) systematizes the cryptographic proposals. Exact matching with homomorphic encryption or private set intersection has negligible false positives but only finds exact copies of known files, while perceptual hashing has false positive rates between 10<sup>−8</sup> and 10<sup>−3</sup>, and machine-learning classifiers between 10<sup>−2</sup> and 10<sup>−1</sup>.
- The UK government funded five prototypes for detecting abuse material in end-to-end encrypted environments through its Safety Tech Challenge Fund. The [independent evaluation by REPHRAIN](https://www.rephrain.ac.uk/safety-tech-challenge-fund/) concluded that none of the tools were fit to be deployed on end-to-end encrypted communication, pointing to unreliable detection, new security vulnerabilities, and no safeguards against repurposing.
- Matthew Green's blog post [On Ashton Kutcher and Secure Multi-Party Computation](https://blog.cryptographyengineering.com/2023/05/11/on-ashton-kutcher-and-secure-multi-party-computation/) explains why MPC does not solve the problem.

**"Apple already designed a privacy-preserving system."** Apple's 2021 design combined perceptual hashing with private set intersection and threshold secret sharing. Researchers found collisions in its NeuralHash function within weeks, and after widespread criticism from security researchers, Apple abandoned the plan in 2022.

**"The scanning is only voluntary."** It is voluntary for the provider, not for the user. Users have no way to opt out of having their messages scanned, and the risk-mitigation obligations in the regulation can put strong pressure on providers to scan. As long as detection results can be reported, the communication is not end-to-end encrypted.

**"We already scan for malware and spam."** Malware and spam filters look for well-defined threats, run to protect the user, and do not report the content of private messages to the authorities. Chat Control does the opposite.

**"If you have nothing to hide, you have nothing to fear."** Privacy of correspondence is a fundamental right, and with millions of false positives, innocent people, including teenagers and parents sharing pictures of their own children, will have their private messages read by strangers. Weakening encryption also makes everyone less secure against criminals and hostile states.

## Open letters from scientists

Scientists and researchers from around the world have published a series of joint statements at [csa-scientist-open-letter.org](https://csa-scientist-open-letter.org), each responding to a new version of the proposal. The table lists the first statement from July 2023 and the letters I have been part of.

| Date | Letter | Signatories | My role |
| --- | --- | --- | --- |
| July 2023 | [Joint statement of scientists and researchers on EU's proposed Child Sexual Abuse Regulation](https://csa-scientist-open-letter.org/Jul2023) | More than 300 from 32 countries | |
| May 2024 | [Joint statement of scientists and researchers on EU's new proposal for the Child Sexual Abuse Regulation](https://csa-scientist-open-letter.org/May2024) | 312 from 35 countries | Signatory |
| October 2024 | [Joint statement of scientists and researchers on the Proposal for the Child Sexual Abuse Regulation](https://csa-scientist-open-letter.org/Oct2024) | 379 from 36 countries | Signatory and press contact for Norway |
| September 2025 | [Joint statement of scientists and researchers on the EU Presidency's new proposal for the Child Sexual Abuse Regulation](https://csa-scientist-open-letter.org/Sep2025) | 807 from 37 countries | Co-author and press contact for Norway |
| November 2025 | [Comments on the EU Presidency's new proposal for the Child Sexual Abuse Regulation](https://csa-scientist-open-letter.org/Nov2025) | 17 senior researchers from 14 countries | Co-author |
| March 2026 | [Joint Statement of Security and Privacy Scientists and Researchers on Age Assurance](https://csa-scientist-open-letter.org/ageverif-Feb2026) | 438 from 32 countries | Co-author and press contact for Norway |

## Official assessments

- [Joint Opinion 4/2022](https://edps.europa.eu/system/files/2022-07/22-07-28_edpb-edps-joint-opinion-csam_en.pdf) of the European Data Protection Board (EDPB) and the European Data Protection Supervisor (EDPS), July 2022.
- [Opinion of the Council Legal Service](https://www.patrick-breyer.de/wp-content/uploads/2023/05/st08787.en23-leak.pdf) (leaked), April 2023.
- [Complementary impact assessment](https://www.europarl.europa.eu/RegData/etudes/STUD/2023/740248/EPRS_STU(2023)740248_EN.pdf) for the European Parliament, April 2023.
- [IAB Statement on Encryption and Mandatory Client-side Scanning of Content](https://datatracker.ietf.org/doc/statement-iab-statement-on-encryption-and-mandatory-client-side-scanning-of-content/), Internet Architecture Board, December 2023.
- [EDPB Statement 1/2024](https://www.edpb.europa.eu/system/files/2024-02/edpb_statement_202401_proposal_regulation_prevent_combat_child_sexual_abuse_en.pdf) on the Parliament's position, February 2024.
- [Report on the implementation of the interim regulation](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX%3A52025DC0740), European Commission, November 2025, which admits that the available data are insufficient to assess whether voluntary scanning is proportionate.

## Research papers

- H. Abelson, R. Anderson, S. M. Bellovin, J. Benaloh, M. Blaze, J. Callas, W. Diffie, S. Landau, P. G. Neumann, R. L. Rivest, J. I. Schiller, B. Schneier, V. Teague, and C. Troncoso. [Bugs in our Pockets: The Risks of Client-Side Scanning](https://academic.oup.com/cybersecurity/article/10/1/tyad020/7590463). Journal of Cybersecurity, 2024.
- R. Anderson. [Chat Control or Child Protection?](https://arxiv.org/abs/2210.08958) arXiv, 2022.
- S. Jain, A.-M. Crețu, and Y.-A. de Montjoye. [Adversarial Detection Avoidance Attacks: Evaluating the robustness of perceptual hashing-based client-side scanning](https://www.usenix.org/conference/usenixsecurity22/presentation/jain). USENIX Security, 2022.
- L. Struppek, D. Hintersdorf, D. Neider, and K. Kersting. [Learning to Break Deep Perceptual Hashing: The Use Case NeuralHash](https://doi.org/10.1145/3531146.3533073). ACM FAccT, 2022.
- J. Prokos, N. Fendley, M. Green, R. Schuster, E. Tromer, T. Jois, and Y. Cao. [Squint Hard Enough: Attacking Perceptual Hashing with Adversarial Machine Learning](https://www.usenix.org/conference/usenixsecurity23/presentation/prokos). USENIX Security, 2023.
- S. Jain, A.-M. Crețu, A. Cully, and Y.-A. de Montjoye. [Deep perceptual hashing algorithms with hidden dual purpose: when client-side scanning does facial recognition](https://arxiv.org/abs/2306.11924). IEEE S&P, 2023.

## Talks, panels, and interviews

- [An evaluation of the risks of client-side scanning](https://iacr.org/cryptodb/data/paper.php?pubkey=35496), panel with Matthew Green, Vanessa Teague, Bruce Schneier, Alex Stamos, and Carmela Troncoso at IACR Real World Crypto 2022.
- [The Point of No Return?](https://techcrunch.com/2023/10/24/eu-csam-scanning-edps-seminar/), seminar organized by the European Data Protection Supervisor with Matthew Green and others, October 2023.
- Bert Hubert's [testimony on client-side scanning](https://berthub.eu/articles/posts/client-side-scanning-dutch-parliament/) in the Dutch parliament, October 2023.
- Carmela Troncoso: [More monitoring, but not more protection](https://www.mpg.de/25788438/chat-control-eu-client-side-scanning), interview with the Max Planck Society, November 2025, and an [explainer video on Chat Control and client-side scanning](https://fair.tube/w/72DCPMByyS1hoSg7gQFHXw).
- Bart Preneel: [Why EU encryption policy needs technical and civil society input](https://www.helpnetsecurity.com/2025/05/19/bart-preneel-university-of-leuven-eu-encryption-policy/), interview with Help Net Security, May 2025.
- Matthew Green: [On Ashton Kutcher and Secure Multi-Party Computation](https://blog.cryptographyengineering.com/2023/05/11/on-ashton-kutcher-and-secure-multi-party-computation/), blog post, May 2023.
- Susan Landau: [Europe Doubles Down on Client Side Scanning](https://www.lawfaremedia.org/article/lawfare-podcast-europe-doubles-down-client-side-scanning), Lawfare Podcast, August 2022.

## Websites and campaigns

- [Chat Control](https://www.patrick-breyer.de/en/posts/chat-control/), by former Member of the European Parliament Patrick Breyer, with news, documents, and analysis of every step of the negotiations.
- [Fight Chat Control](https://fightchatcontrol.eu), with an overview of the positions of the member states and a tool for contacting your representatives.
- [Stop Scanning Me](https://stopscanningme.eu), a campaign by European Digital Rights (EDRi) and more than 60 organizations, and the [EDRi document pool](https://edri.org/our-work/csa-regulation-document-pool/) with the primary documents.
- The [Global Encryption Coalition](https://www.globalencryption.org), which has published several statements on the regulation.

## Norway

Norway is not a member of the EU, but the regulation is considered relevant for the EEA Agreement, so it will most likely also apply in Norway if it is adopted. The Norwegian government [held a public consultation](https://www.regjeringen.no/no/dokumenter/horing-eueos.-forslag-til-forordning-for-a-forebygge-og-bekjempe-seksuelle-overgrep-mot-barn/id2950344/) on the proposal in 2022–2023, and according to [NRK](https://www.nrk.no/urix/chat-control_-eu-vil-masseovervake-innbyggerne-1.17983785), it decided in July 2025 not to request any reservations or special adaptations. The Norwegian Data Protection Authority (Datatilsynet) has been clearly critical, arguing that surveillance of a person should require a concrete suspicion, and that the proposal goes too far.

## My contributions

I have followed the debate since Apple announced its plans for client-side scanning in 2021, and have commented on the proposal in Norwegian media since it was published in 2022. When the first joint statement from scientists was published in July 2023, I [told NRK](https://nrkbeta.no/2023/07/05/massivt-opprop-mot-a-skanne-mobiler-for-overgrepsmateriale/) that it is important that experts in digital security and privacy clearly explain the practical consequences of the proposal and the weaknesses of the technologies it relies on. I compared the proposal to installing surveillance cameras in every room of every home to make sure that no one is planning a crime.

I signed the joint statements in May and October 2024, and I have been the press contact for Norway since October 2024. Since September 2025, I have been a co-author of all the letters: the joint statement on the Danish presidency's proposal in September 2025, which was signed by 807 scientists from 37 countries; the short technical comment by 17 senior researchers from 14 countries in November 2025, explaining why "voluntary" scanning and risk mitigation still threaten end-to-end encryption; and the joint statement on age assurance in March 2026.

My main message has been the same throughout:

- The goal of stopping the spread of abuse material is one we all support, but the proposal requires a technical solution that neither exists nor can be built. The detection methods are inaccurate and lead to many false positives, and each false positive is a real person whose private messages are read by strangers.
- Scanning messages before they are encrypted means that confidential communication is no longer possible. In practice, it introduces a backdoor on every phone and computer that can be abused, and end-to-end encrypted services such as Signal and WhatsApp would in practice become illegal in their current form.
- Once the surveillance is in place, it is hard to guarantee that it will not be extended to other types of crime, or even to political, religious, or ideological expression.
- Targeted surveillance of people and groups under concrete suspicion already gives the police powerful tools, and privacy-preserving computation on encrypted data does not change the underlying accuracy problem.

I also discuss Chat Control and the history of the crypto wars with my students in the lecture [From Enigma to Crypto Wars](https://ttm4205.iik.ntnu.no/latest/slides/L-2.pdf) in [TTM4205 Secure Cryptographic Implementations](https://ttm4205.iik.ntnu.no).

### Op-ed

- [EU-kommisjonens forsvar for Chat Control 2.0 bygger på enten løgn eller inkompetanse](https://www.morgenbladet.no/ideer/kronikk/2023/09/05/eu-kommisjonens-forsvar-for-chat-control-20-bygger-pa-enten-logn-eller-inkompetanse), Morgenbladet, 05.09.2023.

### Interviews

- [EU vil overvåke chat-meldingene dine](https://www.nrk.no/urix/chat-control_-eu-vil-masseovervake-innbyggerne-1.17983785), NRK, 14.08.2026.
- [Politiet vil kunne skanne alle meldingene dine med kunstig intelligens](https://subjekt.no/2025/09/30/politiet-vil-kunne-skanne-alle-meldingene-dine-med-kunstig-intelligens/), Subjekt, 30.09.2025.
- [500 eksperter siger nej til EU's chatkontrol](https://www.version2.dk/artikel/500-eksperter-siger-nej-til-eus-chatkontrol), Version2, 10.09.2025.
- [500 eksperter sier nei til EUs overvåkingsforslag](https://www.digi.no/artikler/intervju-500-eksperter-sier-nei-til-eus-overvakingsforslag/562187), digi.no, 09.09.2025 ([PDF](/files/open-letter-digi.pdf)).
- [EU vil skanne mobilen din i jakten på nettovergripere](https://www.aftenposten.no/kultur/i/bgwX83/eu-vil-skanne-mobilen-din-i-jakten-paa-nettovergripere-naa-advarer-mer-enn-300-forskere-mot-forslaget), Aftenposten, 06.07.2023.
- [Massivt opprop mot å skanne mobiler for overgrepsmateriale](https://nrkbeta.no/2023/07/05/massivt-opprop-mot-a-skanne-mobiler-for-overgrepsmateriale/), NRK, 05.07.2023.
- [EU vil ta nettovergripere ved å overvåke oss alle](https://www.aftenposten.no/kultur/i/q1QK10/eu-vil-ta-nettovergripere-ved-aa-overvaake-oss-alle), Aftenposten, 27.03.2023.
- [Ny EU-lov kan føre til massiv overvåking](https://tv.nrk.no/serie/helgemorgen-tv/202205/DNRR62004122#t=4589s), Helgemorgen NRK1/P2, 14.05.2022.
- [Ny EU-lov mot overgrepsmateriale kan føre til omfattende overvåkning](https://nrkbeta.no/2022/05/11/ny-eu-lov-mot-overgrepsmateriale-kan-fore-til-omfattende-overvakning), NRK, 11.05.2022.
- [Ledende eksperter advarer mot å skanne mobiler for overgrepsmateriale](https://nrkbeta.no/2021/10/15/ledende-eksperter-advarer-mot-a-skanne-mobiler-for-overgrepsmateriale), NRK, 15.10.2021.
- [Apple skal skanne mobiler for overgrepsbilder. Eksperter frykter angrep på personvernet](https://www.aftenposten.no/kultur/i/g6PWRk/apple-skal-skanne-mobiler-for-overgrepsbilder-eksperter-frykter-angre), Aftenposten, 07.08.2021.
