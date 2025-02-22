# DIF Wallet Security WG – Rolling Agenda & Minutes

[![hackmd-github-sync-badge](https://hackmd.io/6hUuZIjGQpeLVAqcxdakZA/badge)](https://hackmd.io/6hUuZIjGQpeLVAqcxdakZA)

[**WG projects**](https://github.com/decentralized-identity/wallet-security) | [ DIF page](https://identity.foundation/working-groups/wallet-security.html) | [Mailing list](https://lists.identity.foundation/g/wallet-security) | [Meeting Recordings](https://docs.google.com/spreadsheets/d/1wgccmMvIImx30qVE9GhRKWWv3vmL2ZyUauuKx3IfRmA/edit#gid=194851117) 

_For this call, you are encouraged to turn your video on. This is a good way to build rapport given we are a large, disparate group experiencing a lot of churn._

_This document is live-edited DURING each call, or shortly after the call, and stable/authoritative copies live on our github repo under /agenda.md .
Please note that we might not notice a pullrequest in time, but you are free to propose agenda items for future meetings via hackmd._

<details>
<summary> Meeting information - <b>Mondays 1300 ET</b></summary>

- Before your contribute - [**join DIF**](https://identity.foundation/join) and [sign the WG charter](https://bit.ly/DIF-WG-select1) (both are required!)
- Time: Every Tuesday, 13:00-14:00 ET
- [Calendar entry](https://calendar.google.com/event?action=TEMPLATE&tmeid=ZjE5YmJqMnA0Z25zMjkyNGNtMzI5YnVnNnNfMjAyMTEyMTRUMTcwMDAwWiBkZWNlbnRyYWxpemVkLmlkZW50aXR5QG0&tmsrc=decentralized.identity%40gmail.com&scp=ALL)
- [Zoom room](https://us02web.zoom.us/j/85747197140?pwd=RmJBcXd6blJidS9rZm9Fd2puclFhdz09), Meeting ID: 85747197140,
Password: 661333
</details>

## Meeting - 5 April 2022 - (6 PM CET, 1pm EST, 10am PST)

### Agenda

1. Review sequence diagram for secure credential lifecycle
2. Wallet Authentication VC Scheme
3. Challenge-response Aries RFC Draft
4. Attestation formats

### Attendees

- Paul Bastian
- Ian Bailey
- Christian (Jolocom)
- Sebastian Bickerle
- Russeell Castagnaro
- Oliver Lauer
- Bernard Joly

### Notes

- discussion on reviewed sequence diagram 
    - ![](https://i.imgur.com/gdDxvZB.png)
    - no major concerns
    - option for verifier to request on-demand attstation?
- attestion formats
    - Option 1 : https://w3c.github.io/webauthn/#sctn-attestation
    - Option 2 : https://datatracker.ietf.org/doc/html/draft-ietf-rats-eat-12
    - Option 3 : define new stuff?
        - the existence of the wallet authentication VC issued by the certifying entity gives assurance by the trust framework?
        - details on attestations are maybe left out
- is VC a preferred container for attestation?
    - buildin support
    - get all the relevant information in one transaction
    - transform complexity of attestation into simplicity of VC
- Schema of Wallet Authentication VC:
    - content
        - identity of the certifying entity (issuer of this VC)
        - wallet name and version 
        - hardware public key
        - status/expiration
        - holder authentication mechanisms (no reference data itself)
    - question to discuss: Should the Wallet Authentication VC reveal the wallet name and version? (suggested by David Chadwick)
        - classical security vs privacy tradeoff
        - prevent market force/centralization?
        - could add complexity
        - wallets could be identified on other metadata in the Wallet Authentication VC
- Who is the Certifying entity?
    - Option 1 : the Wallet issuer backend service
    - Option 2 : Trusted Third Party
        - faster bootstrap, one service could support multiple wallets
        - more centralisation
- Next Steps:
    - Aries RFC Draft
    - Whats the best Option on attestation formats?
    - Whats the best Option on Certifying entity?


## Meeting - 29 March 2022 - (6 PM CET, 12am EST, 9am PST)

### Agenda

1. Specify requirements for Wallet Authentication issuance
2. Wallet Authentication VC Scheme
3. Challenge-response Aries RFC Draft
4. Differences to cloud scenario

### Attendees

- Paul Bastian
- Caspr Roelofs
- Bernard Joly
- Christian Lungu (Jolocom)
- Lance Byrd
- Ian Bailey

### Notes

![](https://i.imgur.com/m8Atrfw.png)
- really doog discussion on the sequence diagramm
    - after the first issuance the device key is used
        - reuse hardware key -> bad privacy/super cookie -> you need keys for each party
        - ideally we don't want to put multiple keys into one wallet authentication credential
        - wallet authentication process upfront or on-demand?
            - upfront: simpler, regular refresh needed
            - on-demand: scalability, freshness
        - discussion result: wallet authentication VC on-demand creation seems like the best solution
        - move "Wallet Issuance" process of app&key attesation inside "Issuance" process after reuqesting the wallet authentication VC (therefore on demand)
        - one per issuer, anchor issuer's DID into the VC
        - verifiers could be served the very same VC as the issuer saw
    - wallet issuer needs to handle and limit access to app attestations
- discuss attestation formats
    - learn from or use https://w3c.github.io/webauthn/#sctn-defined-attestation-formats ?
- protocols between wallet issuer and wallet do not need to be standardized as they are one legal entity
    - but provide optional design for security/reference (not mandatory)
- next steps: 
    - fix sequence diagramm
    - standardize crednetial presentation with challenge response most important


## Meeting - 22 March 2022 - (6 PM CET, 1pm EST, 10am PST)

### Agenda

1. review device binding lifecycle from last session
2. discussion on embedding keys or dids of keys into the VC
3. identify building blocks and match existing components
4. discussion to standardize in Aries RFC

### Attendees

- Paul Bastian
- Ian Bailey
- Christian (Jolocom)
- Tim Bloomfield
- Anne Göllnitz
- Romuald Barbe
- Lance Byrd

### Notes

- review and discuss the lifecycle (commenting in google docs)
    - https://docs.google.com/document/d/1iJAB7VRe1P4wEaBOAb-g8Fn4JvyMXpkpEMkK06U1Qh4/edit#heading=h.65alklkg59te
- whats (in) the trust registry?
    - list of multiple wallet issuers and their certification status
    - is data stored in a (global) trust registry and/or a verifiable data registry?
        - which verifiable data registry if wallet supports multiple DID methods?
        - is there a possiblity for a global wallet trust registry?
    - country-specific wallet trust registry?
    - trust registry bound to the legal framework?
        - e.g. Pan Canadian Trust Framework specifies a list of wallets that are eligible and issuers can trust all wallets from this list
        - separate trust registry in eIDAS etc..
    - content
        - wallet isser and his legal representation
        - contact information
        - versions
        - certification body
        - certification status/expiration
- discussion on embedding keys or dids of keys into the VC
    - goal to rotate key in "lost phone" case
    - neccessary for high-security usecases - you need to start again
    - did document layer adds lots of complexity
    - migration to new phone scenario that proves the old credential in automatic exchange for a new one contacting the issuer
        - fear of adding complexity for little benefit
- next steps:
    - Specify requirements for Wallet Authentication issuance
    - Wallet Authentication VC Scheme
    - Challenge-response Aries RFC Draft
    - Differences to cloud scenario?

## Meeting - 15 March 2022 - (6 PM CET, 1pm EST, 10am PST)

### Agenda

1. continue discussion and planning for device binding & wallet authentication
    - Aries RFC standardization path : https://hackmd.io/j7tSL_QPTPiaPhZh_xLDvA
    - possible combination of both
2. roadmap to standardize in Aries RFC
3. OpenID Connect outlook

### Attendees

- Paul Bastian
- Lance Bryd
- Ian Bailey
- Oliver Lauer
- Bernard Joly
- Sebastian Bickerle

### Notes

- Document shared and presented by Sebastian: https://hackmd.io/@sbickerle/SyLsYsmZq
    - challenges and solutions on device binding presentation as attachments
    - attachments for Present Proof Protocol 1.0 & 2.0
- discussion requires a full lifecycle/full walkthrough(mobile now, cloud later):
    - 0. Wallet Authentication scheme is defined
        - wallet implementer goes through and audit / certfication process
        - wallet implementers register in the trust registry
        - issuers requests and specify list of acceptable wallets for his purposes
    - 1. Wallet Authentication VC
      - starts with the user installing the wallet
      - wallet contacts the wallet implementer backend server
      - perform Wallet Authentication (Google SafetyNet and iOS DeviceChecker)
      - generate hardware-backed keys
      - check key attestation (only Android unfortunately?)
      - Wallet implementer issues a VC to the Wallet including:
          - wallet name, version, etc..
          - device hardware, OS version
          - wallet authentication status
          - hardware public key
          - optional: key attestation
          - holder authentication mechanisms (no reference data itself)
      - data included is a tradeoff between privacy and security
      - the wallet implementer can choose to exclude some information
      - later reissuance / refresh planned
    - 2. Issuer are issuing a device-bound credential
      - request/discover wallet security capabilities
      - Issuer requests Wallet Authentication VC presentation
      - Wallet presentents proof of Wallet Authenticaiton VC
      - challenge-response for the hardware-backed public key (to be [included in the presentation](https://hackmd.io/@sbickerle/SyLsYsmZq) or [separate DIDComm protocol](https://hackmd.io/j7tSL_QPTPiaPhZh_xLDvA))
      - check wallet authentication matches my requirements
      - issuer issues his own VC that is bound to the hardware-backed key 
    - 3. Verifier proofs a device-bound credential from a trustworthy device
      - request/discover wallet security capabilities
      - Verifier requests hardware-backed VC presentation
      - Wallet presents proof of hardware-backed VC
      - challenge-response for the hardware-backed public key (to be included in the presentation or separate DIDComm protocol)
      - optional: Verifier requests the Wallet Authentication VC presentation
      - optional: Wallet presentents proof of Wallet Authenticaiton VC
  - lateron: wallet detects changes and requests a refreshed wallet authentication VC
      - time passed
      - OS version update
      - change of hardware keys?
- further discussion:
    - does the Verifier need to check the Wallet Authetnaction VC as the Issuer already did this?
    - one Wallet Authentication VC per secure credential?
    - semantic context

## Meeting - 1 March 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. discussion on device binding
    - Aries RFC standardization path : https://hackmd.io/j7tSL_QPTPiaPhZh_xLDvA
    - Issuance Option [1a](https://hackmd.io/j7tSL_QPTPiaPhZh_xLDvA) or [1b](https://hackmd.io/@sbickerle/rka12zWe5)
    - Verification mechanism discussion
2. discussion on wallet authentication
    - Issuance Option [1, 2 or 3](https://hackmd.io/0vFZNmD7R-2NTJbiQ2e3sA#Wallet-Authentication)
3. roadmap to standardize in Aries RFC
4. OpenID Connect outlook

### Attendees

- Paul Bastian
- Kai Wagner
- Sebastian Bickerle
- Ian Bailey
- Lance Byrd
- Paul Dunphy
- Bernard

### Notes

- represent Device Binding Issuance Options 1a and 1b
    - Sam Curran form Aries WG tends to Option 1a, can be reused for other purposes
    - Ian is favoring 1a
    - Sebastian is favoring 1b for having less messages
- verification discussion is similar, Option 2a and 2b with separate DIDComm protocol vs decorator
- don't exclude any option for now but start implementation/testing with only one
- represent wallet authentication (Christian Bormann proposed Option 3, renamed to 2b)
    - Option 1 legal issues?
    - Option 2a might not be secure enough as the Wallet Auth VC could be extracted and used elsewhere -> Wallet Authenticaiton only works in combination with device binding
    - so therefore Wallet Authentication must probably used in combination with Device Binding anyway
    - -> combine the protocols
    - followup questions: create multiple Wallet Auth VC with device binding keys, one for each device-bound credentials
    - to ensure the best privacy(we lose some anyway) Wallet Authentication should be used cautoiusly, as it possibly correlates all presented credentials?
    - tradeoff between privacy and security
- next steps: combine device binding 1a & wallet authentication 2b to combined protocol, propose as Aries RFC PR to extend the discussion in the broader community
- get insights and views from W3C / non-AnonCreds perspective

## Meeting - 22 Feburary 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. cochair setup information
2. device binding
    - Aries RFC standardization path : https://hackmd.io/j7tSL_QPTPiaPhZh_xLDvA
    - Verification mechanism discussion
    - implementers interop group
3. wallet authentication
    - discussion between direct and VC option : https://hackmd.io/0vFZNmD7R-2NTJbiQ2e3sA
    - roadmap to standardize in Aries RFC

### Attendees

- Paul Bastian
- Fabien Petitcolas
- Oliver Lauer
- Ian Bailey
- Darrell O'Donell
- Sebastian Bickerle
- Tim Bloomfield

### Notes

- Bernard steps down as co-chair as he leaves OneSpan
    - OneSpan will figure out if they want to replace him and will observe for now
    - open for new cochair applications, Paul+Oliver for now
- device binding:
    - Paul presents Option 1a to use a separatre DIDComm protocol
    - Sebastian presents Option 1b to extend existing Credential Issuance protocols
    - questions:
    - reuse libindy nonce or use separate one?
        - new one is probably the better idea
    - does request credential contain raw attributes or only blinded link secret?
    - does the wallet check/match the credential preview from the offer credential step with the actual attributes received in the credential issuance step
    - DIDComm specifics: inline, embedded or appended?
        - best to PR to aries-rfc and discuss in the Aries Group Meeting
    - opinions between 1a and 1b:
        - Darrell: pro option 1a as its cleaner
        - Tim: probably combined with wallet authentication, 1 a is cleaner but 1b is faster?
        - Paul: 1a to avoid multiple credentialm issue protocols
        - Sebastian: 1b to avoid additional messages
- wallet authentication
    - we briefly started the discussion on wallet authentication standardization and the technical solutions, continue here next week
    - https://hackmd.io/0vFZNmD7R-2NTJbiQ2e3sA#Wallet-Authentication

## Device Binding Meeting - 15 Feburary 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. cochair setup
2. interest for sample implementation
3. discussion on integration, e.g. AnonCreds/Aries

### Attendees

- Paul Bastian
- Ian Bailey
- Anne Göllnitz
- Oliver Lauer

### Notes

- Bernard is moving away from OneSpan and cannot fulfil the co-chair role
    - 2 cochairs remaining for Wallet Security (Oliver still part, was inactive)
    - focus on wallet security call next week
- discussion on interest for implementing the device binding feature
    - achieving interoparability tests with multiple implementations
    - Wallets:
        - BCGov: Aries/AnonCreds with iOS/Android/SE
        - eSatus: Aries/AnonCreds with iOS/Android
        - ID-Wallet: Aries/AnonCreds with iOS/Android
    - Issuer/Verifier:
        - acapy, .NET
    - talk to Lissi Team, Stephen Curran
- Aries -> create a new Aries RFC to specify the DIDComm protocol for device binding
    - two separate protocols for device binding/wallet authentication or same protocol?
- start a workshop with technical experts for first proposal focusing Aries/Anoncreds

## Meeting - 08 Feburary 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. wallet implementers survey results
2. discussion on wallet authentication
3. discussion on verifier authenticity

### Attendees

- Paul Bastian
- Bernard Joly
- Ian Bailey
- Kai Wagner
- Sebastian Bickerle
- Anne Göllnitz
- Jorge Flores
- Russell Castagnaro
- Oliver Lauer
- Chris Kelly
- keith Kowal

### Notes

- Wallet implementers survey:
    - results: https://docs.google.com/spreadsheets/d/1ao9DSDhtn0dvOTAFp-Uzs_UlLDzwVnrg9eXm4L5syiY
    - Output PDF: https://github.com/decentralized-identity/wallet-security/blob/main/contributions/wallet_implementers_survey_results_v1.pdf
    - in short: most wallet implementers rely on mobile and do not support multiple security levels yet
- Wallet authentication discussion from last week
    - who should be able to check the wallet authenticaiton?
        - optional in any way
        - available only to issuers or both issuers and verifiers
        - wallet authentication is less intreseting to verifiers in the offline usecase(photo comparison)
        - thinking about the usecases..
    - how to implement that?
        - A: information stored in a VC
            - good integration
            - works offline
            - old information/no freshness
        - B: interactive protocol between wallet and issuer/verifier
            - freshness check
            - missing a protocol/implementation effort needed
    - does online/offline usecases make a difference?
    - should be aligned to the stack
    - hybrid mobile/cloud wallets: wallet authentication mechanism usable
    - pure cloud wallet: wallet could rely on HSM mechanisms in the backend
- excursion of non-trust wallet architectures
    - required to prevent copy of credential for high security
    - put holder authentication into the credential itself or third party
- Trusted Verifier - UX and security checks performed by the wallet
    - concept screens from german project: https://github.com/decentralized-identity/wallet-security/blob/main/images/Trusted_Verifier_Slide.png
        - requirement from regulatory viewpoint
        - similar approaches in existing wallets
        - similar mechanism for issuers
        - harmonization possible?

## Meeting - 01 Feburary 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. wallet authentication

### Attendees

- Paul Bastian
- Bernard Joly
- Ian Bailey
- Sebastian Bickerle

### Notes
- wallet authetnication -> neccessary because user authentication happens locally between user and wallet
- therefore need to trust the wallets implementation
- who should be able to check the wallet authenticaiton?
    - only issuer
    - both issuer and verifier
- use smartphone OS mechanisms like AndroidSafetyNet/DeviceChecker
- how to implement that?
    - 1. information stored in a VC
        - wallet implementer issues a VC to the wallet 
        - wallet implementer needs additional backend service to issue VCs
        - information might be old/request new VCs on-the-fly neccessary
        - do you hide that from the user?
        - who is able to request it?
        - could scale better
        - wallet issuer could revoke VCs for "broken" app versions
    - 2. seperate (DIDComm) protocol
        - requesting party could provide a nonce
        - allows fresh check
    - prevent correlation

## Meeting - 25 January 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. review device binding kickoff
2. wallet implementers survey
3. implications of eIDAS results (Architecture and Reference Framework 0.1)
4. outlook for differential credential security
5. roadmap 2022

### Attendees

- Paul Bastian
- Bernard Joly
- Kai Wagner
- Chris Kelly
- Sid Sharma
- Oliver Lauer
- Shaked Vax
- Stephen Curran
- Torsten Lodderstedt
- keith Kowal

### Notes
2. wallet survey: review for the next meeting in detail. more answers are coming by european persons.

- wallet survey: https://docs.google.com/spreadsheets/d/1HjSnF1TRoFdQJNT68HlqZtOaqxGBytAr02cw7WPb6LE/edit#gid=1868476464

3.eIDAS/framework 0.1. Oliver will share the draft version 0.1. after the meeting.  there are three credentials type. There is more a consolidation of all requests comming from all european members. Document is in early stage. intoduction of new term "electronic attestation attribut". 

4. outlook for differential credential security.
We need to work a new work item proposal. We are looking for workitem owner. Kai Wagner will check for the next call ( action).Need contributors for this work item.

5. roadmap 2022
expectation for 2022: for the next meeting, all members should propose some work items for the next meeting ( two weeks).


## Device Binding Kickoff - 18 January 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. expectations
2. organisational stuff
    - repo & place for notes, document, etc..
    - meeting intervals
    - GoogleDocs or Markdown?
3. review for the draft
4. building a document structure
5. goals & roadmap 2022

### Attendees

- Paul Bastian
- Kai Wagner
- Ian Bailey
- Bernard Joly
- Oliver Lauer

### Notes
- expectations:
    - Bernard: binding for KYC credentials, bind credentials to the wallet and user, mobile and cloud
    - Ian: high assurance wallets and credentials, credentials bound to the wallet, preventing fraud, stealing, W3C VCs have no binding to wallet, three open source funding projects to build a wallet, relation to key management
    - Oliver: german banking wallets, eIDAS 2.0 banking wg, policy, need for hardware binding in the banking sector with high reach, compromise security<>reach
    - Kai: compromise between hardware binding <>credentials with backup possibilities, differentiate from smartphone vendors
    - Paul: enable high security for regulatory use cases, have standardized
- orga:
    - [work item landing page](https://github.com/decentralized-identity/wallet-security/blob/main/work_items/device_binding.md)
    - [calendar entry](https://www.google.com/calendar/event?eid=NXRpM25jdWYyZTk2YXZycmJyMGY0dHIwMWRfMjAyMjAxMThUMTcwMDAwWiBkZWNlbnRyYWxpemVkLmlkZW50aXR5QG0)
    - biweekly call
    - move to github markdown document
- additional items for the draft
    - mobile and cloud solutions
        - it seems most people require mobile solutions first
    - share knowledge and guidance for implementing wallet in mobile OS
        - Andorid
        - iOS
    - difference between W3C vs AnonCreds
        - enable both
    - relation to AnonCreds Linksecret
- document structure
    - https://docs.google.com/document/d/1iJAB7VRe1P4wEaBOAb-g8Fn4JvyMXpkpEMkK06U1Qh4/edit#
    - reviewed summary, motivation, existing components, requirements sections
    - todos:
        - review&extend summary section
        - add regulatory frameworks to the motivation section
        - add information/links/knowledge to each component in the existing components section, e.g. to security, market share, personalization, key mangement, (dis)advantages
        - add information/links/knowledge on Android implementing guidance
        - add information/links/knowledge on iOS implementing guidance
        - write something to W3C/AnonCreds relation
- goals & roadmap
    - draft by end of march


## Meeting - 11 January 2022 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. organisational stuff
    - device binding kickoff
    - wallet implementers survey
2. review comments for differential credential security
3. review comments for device binding
4. updates from eIDAS consultations
5. roadmap 2022

### Attendees

- Paul Bastian
- Bernard Joly
- Ian Bailey
- Kai Wagner
- Oliver Lauer
- Darell O Donell
- Sebastien Bickerie
- Torsten
- Keith Kowal
- Shaked Vax

### Notes

- organsiational stuff
    - [kickoff doodle](https://doodle.com/poll/bttcdxcqdn9cpziu) for device binding work item will close tomorrow
    - please share and fill out the [wallet implementers survey](https://docs.google.com/forms/d/e/1FAIpQLSeZE3sL7g-ry2l_iRbjr3PsMRdguznw5jBvx1vha0c6LSzy4Q/viewform) to get input from the community
- review comments for differential credential security
    - required LoA should be encoded in the verification/presentation request directly for the best UX
    - continung discussion on LoAs
        - overall LoA -> LoA of the weakest link
        - LoA evalutation consists usually of different subsections(Identity proof, Storage security, Holder authentication, Revocation, etc..)
        - what is the verifiers opinion/role here? verifier accepts the LoA for his usecase?
        - verifier has its own rules/business logic in any case
        - LoA implicitly defined by the Issuer(i.e in the schema) vs LoA is encoded as standardized metadata
        - encoded metadata about LoA should be preferred
        - how does that integrate into W3C VC model?
        - encoded LoA acts as a useable building block but verifier can still make his own decision anyway
        - How to deal with many LoA/Trust Frameworks?
        - LoA solving in the wallet alone is not possible -> needs a better scope
        - -> Wallet Securit yWG should provide wallet-specific building blocks to be used for LoA evaluation (first)
        - solving the whole LoA riddle might not be the desired goal (for now)
    - focus on device/wallet binding and holder binding is very important for success of regulated use cases
    - continue here next week on device binding work item


## Meeting - 14 December 2021 - (6 PM CET, 12pm EST, 9am PST)

### Agenda

1. organisational stuff
    - restructed github
    - document summary
    - holidays
2. new work item for differential credential security
    - [PR#7](https://github.com/decentralized-identity/wallet-security/pull/7)
3. new work item proposal for device binding
    - [PR#6](https://github.com/decentralized-identity/wallet-security/pull/6)
4. presentation of Wallet Security concepts at german SDI project ([slides](https://nextcloud.idunion.org/s/D2cbMi6w8t3nPYj))
5. wallet implementers consultation, questionnaire and spreadsheet
6. wallet security implementation document from Canada

### Attendees
- Paul Bastian
- Ian Bailey
- Keith Kowal
- Kai Wagner
- Darell O
- Sebastian Bickerle
- Shaked Vax

### Notes

- organsiational stuff
- new work items
    - started device binding
    - differential credential security pending
    - Paul is sending out doodle for kickoffs in the new year
- updates on regulatory requirements:
    - german BSI publishes first paper on SSI with blockchain: https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Krypto/Eckpunkte_SSI_DLT.pdf?__blob=publicationFile&v=2
        - focussing more on DLT and less on wallets
    - fast iterations on eIDAS 2.0 Toolbox, also focussing on wallet security
        - the proposed building blocks are all relevant & neccessary to upcoming eIDAS proposal
    - guidance/starting document for security analysts for SSI from Canada: https://github.com/hyperledger/aries-mobile-agent-react-native/tree/main/docs/design
        - pool knowledge on PIN/Biometrics authentication and other ideas from different use case experiences in Canada/Germany and draft Wallet Secuirty documents from them
        - common SSI-stack idnependent concept in DIF WAllet Security WG
        - having an Aries-specific implementation in Appendix or Aries RFC
- wallet security presentation
    - link to slides: https://nextcloud.idunion.org/s/D2cbMi6w8t3nPYj
    - works well as a summary and vision for the DIF WAllet Security WG
    - free to share
- wallet security comparison
    - goal: contact wallet implementers, get status quo, motivate
    - proposed google forms:
        - https://docs.google.com/forms/d/e/1FAIpQLSeZE3sL7g-ry2l_iRbjr3PsMRdguznw5jBvx1vha0c6LSzy4Q/viewform




## Meeting - 30 November 2021 - (6 PM CET)

### Agenda

1. work item proposal for differential credential security
2. work item proposal for device binding
3. wallet implementers consultation, questionnaire and spreadsheet
4. discussion on how to proceed with the big picture/vision

### Attendees
- Paul Bastian
- Oliver Lauer
- Sebastian Bickerle
- Dirk Thatmann
- Sid Scharma
- Kai Wanger
- Chris Kelly
- Keith Kowal
- Shermined Salehi Esmati

### Notes

- presenting the differential credential security draft:
    - https://docs.google.com/document/d/1M0XWGmOOpK3JY9lIMvgytpRHYcDiBN1yIqd1-2HnK9w/edit?usp=sharing
    - please read, give feedback in comments
    - filing the work item proposal this week
- presenting the device binding draft:
    - https://docs.google.com/document/d/1iJAB7VRe1P4wEaBOAb-g8Fn4JvyMXpkpEMkK06U1Qh4/edit?usp=sharing
    - feedback: binding to peerDIDs instead of seperate attribute
    - please read, give feedback in comments
    - filing the work item proposal this week
- started work on wallet implementers consultation
    - trying to create a questionnaire to reach out

## Meeting - 16 November 2021 - (6 PM CET)

### Agenda

1. restructing of the working group specification
2. advancing wg output by forming potential new work items with subgroups of common interest
 - differential credential security concept (security levels)
 - device binding
 - trusted verifier
 - user authentication, 2FA
 - wallet security information flow
 - your input
3. discussion on how to proceed with the big picture/vision

### Attendees
- Paul Bastian
- Ian Bailey
- Sebastian Bickerle
- Dirk Thatmann
- Keith Kowal
- Kai Wagner
- Thierry Thevenet
- Stephen Curran

### Notes
    
- Bernard might not be able to participate on the next meetings, asking for help on the notes
- Keith: the group is too small to tackle these many work items
- Kai&Dirk: work items are best to get forward with diffrent opinion in the group
- chicken&egg problem, when to advertise, get wallet implementers on board?
- Keith: comparable security between wallets? customer needs..
- Ian: 2FA, key management
- agree that we can start on 1-2 work items and split work there
- how to advertise to/motivate wallet implementers?
    - gather requirements in a document?
    - comparison table of all wallets?
    - gather wallet implementers and contacts
    - google spreadsheet
- vote 1: its good to start at least one work item (agreed)
- vote 2: start with work item differential credential security concept (agreed)
    - work item owners: Paul, Keith
    - start with a strawman proposal, continously working on it
    - google docs for easy commenting
- Dirk, Paul want to work on device binding

## Meeting - 2 November 2021 - (6 PM CET)

### Agenda

1. wg organization from DIF
2. Trusted verifiers - updates and sequence diagrams
3. Discussing wallets supporting multiple security levels for the issuer to chose

### Attendees
- Paul Bastian
- Bernard Joly
- Chris Kelly
- Kristina Yasuda
- Balazs Nemethi
- Ian Bailey
- Sebastian Bickerle
- Pieter Kasselman
- Keith Kowal
- Kai Wagner

### Notes

#### WG organization

- Wallet security document is now available in gitub. https://github.com/decentralized-identity/wallet-security/blob/main/wallet-security-criteria.md
- Get more regulators on board
- Get more wallet implementers on board
- Get more active participation
- increase visibility of the group
- **quickly understable core vision of the group**
- document needs more written text, self-readable/understandable
- ask Oliver to step down

#### vision of the group

- vision of the group similar enough for the summary document?
- scope too broad? narrow scope for first iteration?
    - example: tusted verifier off/on scope - people undecided
- what to focus on? seperate work items?
- goal?
    - vote on new work items - undecided
    - what are the motivations for issuers and verifiers?
        - understand requirements from the industry
    - discussion between levels and existing LoAs, e.g. ISO -> drifting to content discussion
    - group is undecided wether new work items will be beneficial

#### Trusted Verifier
time was over

## Meeting - 26 October 2021 - (6 PM CET)

### Agenda

1. IIW#33 review - relevant sessions
2. wg organization - attendance , cochairs, contributions, country-specific views
3. Trusted verifiers - authenticating and communicating security levels of verifier identities to the user in the wallet(UI&UX)
4. Discussing wallets supporting multiple security levels for the issuer to chose - gaining the best of all worlds

### Attendees
- Paul Bastian
- Bernard Joly
- Ian Bailey
- Dirk Thatmann
- Kai Wagner
- Tom Jones
- Thierry Thevenet
- Keith Kowal

### Notes

#### IIW recap - relevant sessions:
 - 3A - Need for hardware backed crypto on the web!
 - 11J - The future of Governance
 - 14B - UX: continuing the mid-2021 IIW UX conversation

#### WG organisation
 - shift to bi-weekly (voted pro)
 - other tooling (voted contra - not for now)
 - still lacking input of contributors
     - maybe its only the start?
     - difficult topic / SSI open-ness does not matchcurrent world?
     - target more wallet implementers?
     - another round of advertisement @DIF
 - adress also visions instead of products/projects
 - stack-indepedence?
 - more concise summary on what to achieve (not the charter)?
 - focus on specific problems to solve, e.g. german gov project?

#### Trusted verifiers (and also issuers?)
 - proposing different levels
     - how many make sense?
 - level 0: non-authenticated
     - minimum requirements (above bare minimum)
     - disallow HTTP
     - **display warning for untrusted connections**/requests (phishing)
     - display service-endpoint URL
     - do not display self-attested names? (e.g. Aries RFC0160 label)
 - level 1: authenticated
     - reusing HTTPS certificate attributes of service endpoints
     - show company name from [EV](https://en.wikipedia.org/wiki/Extended_Validation_Certificate) and [QWAC](https://en.wikipedia.org/wiki/Qualified_website_authentication_certificate) certificates
     - Public DIDs from a ledger?
     - DIDs shall be linked to service endpoints using well-known DID configuration, did:web, did:dns...
 - level 2: trusted
     - like Twitter checkmark
     - trust registries?
     - advanced trust from a trust service provider
         - as VC or inside the DID document?
 - UX challenge on how to communicate more than 2 levels
 - 2way authentication
     - this is usually handled by the proof request itself
     - awareness for MITM, but this is probably a separate topic
 - issues:
     - handle did-method specific stuff
     - handle network/ledger specific stuff
     - handle non-HTTPS protocols

#### Discussion on multiple security levels supported by the wallet
time was over 

## Meeting - 12 October 2021 - cancelled
due to IIW

## Meeting - 05 October 2021 - (6 PM CET)

### Agenda

1. Matching individual security capabilities values to the 3 proposed wallet security levels(L1,L2,L3)
2. Discussing wallets supporting multiple security levels for the issuer to chose - gaining the best of all worlds
3. Trusted verifiers - authenticating and communicating security levels of verifier identities to the user in the wallet(UI&UX)

### Attendees
- Ian Bailey
- Michael Lodder
- Kai Wagner
- Thierry Thevenet
- Bernard joly 
- Kristina Yasuda
- Tomj
- Keith Kowal

### notes
Trusted Verifiers. In germany launched the drivers licence in the new wallet. There are 300k download. There are some issues regarding the UX wallet . Trusted the verifier is an isssue. There are  different levels for the "verify". The verifiers need to improve the authentication ( not done today). Thi point will be add in the google doc for the final specifications. is the wallet will support multiple levels of security ?




## Meeting - 28 September 2021 - (1300 ET)

### Agenda

- Next meeting base on the survey ( decision by the group)
- Overview on eidas regulation/ EU wallet framework
- Review comments on "Keith  presentation"
- Review the continue discussion and work on terms, security architecture, level assurance 
- any new topics 

### Attendees
- Thierry Thevenet 
- Kai Wagner
- Bernard Joly 
- Chris Kelly
- Ralf Knobloch


### notes
Base on survey and the members presents we will move the meeting @6PM CET 

Presentation of the EU wallet status( see the slides).For the deployment There are 3 possibilties : each european members can select the model to build and distribute   the wallet  
It should be interested for all members to have a vision on the different regulations in the word for  the wallet framework. i request some volunteer to present the status for USA, ASIA, UK or others.
 
High security level, after some discussions on the "key recovery" .no recovery possibility should be the solution for the government. 
action:  Ralph will present an use case ( key recovery needs) in the next 3 weeks.

Cross border verification/Cross wallet usage : need to align the vision between all EU countries. some EU countries are more restrited today for the security requirements.



## Meeting - 7 September 2021 - (1300 ET)

### Agenda

- continue discussion and work on terms, security architecture, communication
- drafting an example workflow with credential lifecycle and the describe the wallet security aspects
- discussion on a questionnaire
- feel free to address any new topics

### Attendees

- Paul
- Ian
- Keith
- Dirk Thatmann

### notes

- we continue back to regular weekly meeting from here on
- discussion on individual capabilites vs groups/level of assurances
    - individual capabilities seem to detailed to list them/negotiate them specifically
    - rather list or link to existing standards, avoid duplication of requirements
    - group capabilites properties together
- Keith supports the discussion with presentation:
    - https://docs.google.com/presentation/d/1tk1AlUMthjIzIJJmF-ZSNZ7J3DmM1fxQztLukF5AoZQ
- Examples for diffrent Wallet Security Levels:
    - Level 1: German government issues Digital Passport to recipient
    - Level 2: AWS issues “Solution Architect” certification to recipient
    - Level 3: Home Depot issues free membership card to recipient
- Examining typical capabilites for given security levels, roughly:
    - Level 1: HW like TEE, credentials bound to device, strong user authentication, no revocery
    - Level 2: Keys are allowed in remote backup allowed with storng user encryption,  recovery allowed with complex knowledge/biometrics
    - Level 3: Keys are allowed in remote backup with platform encryption, simple user authenticaiton like username/password, platform manages credentials
- most secure relevant aspect like signature schemes, DID methods, protocols are given and already decided by the issuer
- wallet usually doesn't have too much possiblities(take it or leave it)
- verifier usually doesn't have too much choices either, maybe user authentication sometimes?
- most high security relevant questions (especially in regulated use cases) are handled upfront in the design/certification phase, therefore certification/assertion of the wallet will be highly relevant
- Main points/questions:
    -  do we apply to existing LoAs directly or define intermediate levels for groups of capabilities described here and match existing LoAs to these
    -  Does a wallet support L1,L2 and L3 ? Can we have L1 and L3 credentials at the same time?
    -  Wallets could fulfill LoA on a global/device base or on a per-credential base decided by the issuer
    -  Distinguish Wallet Security LoA and IT System Security LoA, attention that we don’t mix in data input quality LoA







## Meeting - 20 July 2021 - (1300 ET)

### Agenda

- working group meeting time slot status
- working group charter signing status
- review current organization status of the WG
- review vacation summer status
- presenting the working group document and continue work on security archtiectures/capabilities
- split prior art review as homework?

### Attendees

- Paul Bastian
- Bernard Joly
- Kristina 
- Martin Ingram
- Ian Bailey
- Keith
- Rogelio

### notes
- working group meeting time discussion
  - Bernard provide poll/thoughts about biweekly or topic-specific workshops
- summer break for next three weeks
    - starting back on 17. August
- current organization situation
    - no work items/owners of these
    - more workshop oriented/longer sessions on specific topic
- starting work on spec-like group document:
    - https://docs.google.com/document/d/1c8v7Jg0ZthvoRESeVZayTPE5xt5P_MAKIA9nz2qrCDo/edit#
    - new sections/discussion for the document:
        - terminology/vocubalary
        - common security architecture
        - capabilities criteria
        - capabilities groups/levels
        - attestation
        - communication overview
- discussion about how to continue


## Meeting - 6 July 2021 - (1300 ET)

### Agenda

- suggestion to move one hour earlier - 6pm CET
- gathering feedback/desire from wallet providers
- discussing and working on definition of security architectures

### Attendees

- Paul Bastian
- Bernard Joly
- Dirk Thatmann
- Kai Wagner
- Ian Bailey
- Rogelio Morrell
- Sebastien Bickerle
- Keith Kowal

### notes

- Sebastian Bickerle(Lissi): native apps developer: what measures can we do to participate in diffrent/regulated usecases, keeping UX as easy as possible, access to hardware is difficult, secureing indy sql-database, only starting yet
- Kai(Jolocom): similar, working with aries 2.0, starting on SSI-specific applet, figuring out how to use them, difficult crypto-alignment, german&euro market, regulated market, aligning with international standards
- Rogelio: documents & crypto wallets, consumer wallet, how to secure/store VCs etc, database with encrypotoion layer on top for now, no standardized wallet security, moved to web wallet, wallet-connect
- Keith: mobile wallet, arch changed to web wallet, diffrent key management, customer-driven, key management shifted from secure enclave/TEE, lost-password support issues, this lead to focus on recovery
- security architecture:
    - differentiation between user-managed/native-generated keys(software vs hardware) vs platform managing/cloud(similar to password management)
    - hot-wallet vs cold-wallet
    - edge vs cloud

- Keith: requirements to move to platform-based
    - browser can't store the keys securly
    - recovery
    - instability in w3c vc model, easier reissuance
- cloud wallets are hard to link to mobile secure storage, also changing the phone is difficult
- security architecture #1
    - cloud-wallet/platform-managed
    - keys are stored/lifecycle by the platform
    - authentication is towards the platform (e.g. username/password)
    - specific features: strong recovery, web&mobile possible, yubikeys
- security architecture #2
    - mobile/native
    - keys are generated the edge(software or hardware)
    - keys are stored/lifecycle on filesystem/secure enclave
    - authentication is PIN/mobile OS provided mechanism
    - specific features: device binding, automated backup to cloud possible
- mix of VC with diffrent security architectzure in one wallet?


## Meeting - 28 June 2021 - (1300 ET)

### Agenda

1. Welcome and introductions
2. Working Group signing status and meeting notes organization
3. gather/brainstorm a potential list for terminology
4. gather/brainstorm a potential list for use cases
5. continue the creation and discussion on the subcriteria excel sheet from last session
6. start discussion on security architectures

### Attendees

- Paul
- Kristina
- Bernard
- Ian Bailey
- Joachim Lohkamp
- Michael Lodder
- Kai Wagner

### notes

- signing the charter is possible now, ongoing...
- @nembal please check the link for signing up
    - @pwlb the link is [here](https://bit.ly/DIF-WG-select1). (to sign it a company must be a DIF member first) 

Michael 3 pillars:
- storage
- keys (key management and assertions)
- authentication (of the connection / access from another device)
※ user authentication out of scope

authentication of the wallet
 - user (PIN)
 - proof of possession(secure key, smartcard)
 - biometric (for mobile app)
     - mainly talking about 1:1(authetnication) rather than 1:N(identification)
     - mobile OS mechanisms
 
Design principles for higher security:
- where authentication factor belongs to: device or native app (PIN, biometrics, etc.)
- combine device & native app protection  
- key contains information only about the key - NOT about where it is used.
- Do not use non-modifiable data like biometrics alone

device authentication

device attestation
- e.g. TEE attested keys

wallet attestation
- wallet is implemented from proven/attested provider
- device checking/ app genuity
- conditional to device
- Michael mentions protocols to prove wallet, e.g. Oberon

Government Requirements
- Want to issue only to wallets provided by attested providers

Issues
- How does the issuer know it is talking to the attested app?
    - some sort of token exchange to pass the attestation/certification info
    - https://github.com/mikelodder7/oberon



## Meeting - 22 June 2021 - (1300 ET)

### Agenda

1. Welcome and introductions


### Attendees

- Paul
- Bernard
- Kristina
- Oliver
- Keith
- Michael
- Balasz
- Sebastian Bickerle

### notes

 - Sebastian from Lissi team new on the call
 - Agenda more up-to-note form next week
 - TSC approved the charter
     - join the wg by signing the charter
     - https://bit.ly/DIF-WG-select1 
 - organizing meeting notes via HackMD and sync to DIF github
 - We have done an exercise regarding prior art:

    1. We spent time breaking up high-leve criteria into sub-criterias (vertical axis)
        - key managemen into 
            - key lifecycle of private-public key pair (creation, signing, encryption, and decryption)
            -  key rotation	
            - key entrophy
            - key storage (HW/SW)
        - Credential storage into 
            - Remote / local (native app, browser)
            - Control - user / custodial 
        - Back-up into
            - back-up	Cloud back-up
            - personal vs centralized storage
        - Recovery system into
            - Key recovery (Certificate?)
            - Credential recovery
            - Wallet recovery (additional personalization metadata)
    2. and map them across prior art in various organizations: W3C, DIF, OIDF, Aries, Kantara, and implementations
    - the source is here: https://docs.google.com/spreadsheets/d/1ZvQm6zlGqrRmvSG2siAXnV824sHhVyH0qRDjmunr4p8/edit#gid=0



