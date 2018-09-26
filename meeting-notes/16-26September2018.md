# dat protocol WG (Meeting #16)

*Sept 26, 2018 via IRC (#datprotocol)*

[Agenda discussion](https://github.com/datprotocol/working-group/issues/31)

## People

* pfrazee
* bnewbold
* dcrobins
* mafintosh

## Agenda

 - Review last meeting/action items
 - AGENDA ITEM: Discuss new Hyperswarm project https://pfrazee.hashbase.io/blog/hyperswarm
 - AGENDA ITEM: Should we deploy hyperdrive-v2 with a simplified trie structure, or with hyperdb single-writer?
 - AGENDA ITEM: "Follow redirects in DNS?" datprotocol/DEPs#40 TL;DR: should we allow HTTP redirects of .well-known/dat? In particular in handling of www.example.tld vs. example.tld. Alternative is to require direct response on both like Let's Encrypt.
 - AGENDA ITEM: "Added functions for sign/verify to DEP 0002" datprotocol/DEPs#41. TL;DR: include more Javascript+libsodium code in a DEP as a clarification. My dispensation is to merge this PR, and also expand the english language with reference to specific (crypto) RFCs and what is getting hashed signed.

## Summary/Review of Previous Meeting

* [Meeting notes](https://github.com/datprotocol/working-group/blob/master/meeting-notes/15-12September2018.md)
* [Action Items](https://github.com/datprotocol/working-group/issues/32)

#### Action Item Review:

* ACTION ITEM: Submit a DEP for hyperdrive/v2 (the trie-structure update) (@mafintosh, @pfrazee)
    * pfrazee: I ended up punting this for further discussion, which is now an agenda item
* ACTION ITEM:  get hyperdb update ready for vote (@bnewbold)
    * bnewbold: I started digging back in to updating hyperdb DEP this morning, but not ready yet
* ACTION ITEM: respond to datproject/security-discussion#1 (@mafintosh)
    * Maf has summarized a response at https://github.com/datproject/security-discussion/issues/1#issuecomment-424504749


## Meeting Notes

* AGENDA ITEM: Discuss new Hyperswarm project https://pfrazee.hashbase.io/blog/hyperswarm
    * There was great rejoicing
* AGENDA ITEM: Should we deploy hyperdrive-v2 with a simplified trie structure, or with hyperdb single-writer?
    * mafintosh: I "productionized" the hyperdb trie as a single writer one in the hypertrie module, in an effort to get that part out
    * hypertrie module is currently not compatible with hyperdb spec due to a couple of small differences
    * bnewbold and pfrazee raised concerns about added effort of another implementation (specwork, multiple migrations)
    * decided: we'll integrate hypertrie's work with hyperdb and aim to deploy hyperdb singlewriter
* AGENDA ITEM: "Follow redirects in DNS?" datprotocol/DEPs#40 TL;DR: should we allow HTTP redirects of .well-known/dat? In particular in handling of www.example.tld vs. example.tld. Alternative is to require direct response on both like Let's Encrypt.
    * overall response was, we should do the simplest thing
    * concern: ties us to behaviors which may only be possible with wellknown DNS lookup, which may not be a longterm solution
    * concern: adds complexity and hard to predict risks
    * decided: "soft no" - we'll leave open to more commentary but keep simple for now
* AGENDA ITEM: "Added functions for sign/verify to DEP 0002" datprotocol/DEPs#41. TL;DR: include more Javascript+libsodium code in a DEP as a clarification. My dispensation is to merge this PR, and also expand the english language with reference to specific (crypto) RFCs and what is getting hashed signed.
    * decided: accept the PR

## ACTION ITEMS

* respond to the email thread behind datproject/security-discussion#1 (@mafintosh)
* write up some notes on spec changes for hyperdb (simplifications & single-writer) (@mafintosh)
* respond to datprotocol/DEPs#40 with "soft no" (@pfrazee)
* merge datprotocol/DEPs#41 (@bnewbold)
