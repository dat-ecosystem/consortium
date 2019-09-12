# dat protocol WG (#31)

*11 September 2019 via IRC (#datprotocol)*

[Agenda discussion](https://github.com/datprotocol/working-group/issues/67)

## People

* okdistribute
* mafintosh
* jhand
* andrewosh
* rangermauve
* pfrazee

## Agenda

* Catch up with what everyone has been doing
* Review last meeting/action items
- AGENDA ITEM: ...

## Catch up

* okdistribute: I'm still in tech policy fellowship and warapping it up this week!
* mafintosh:
  - rangermauve: Whatcha been up to? I see there's the new protocol changes that are being PR'd.
  - mafintosh: yep, just finalising that
* jhand: nothing to report here
* andrewosh:
  - andrewosh: working through many things gearing up for v10 to be stable. want to finalize everything corestore so that there won't be any changes to the on-disk format. new release of corestore (3.0.0) has the dependency graph stuff I talked about, and also key derivation from a single master key. so that, and updating the data structures to take advantage of that.
  - rangermauve: What does the derivation look like? `const keypair = generate(originalPair, somename)`?
  - mafintosh: it uses the derive-key module
  - andrewosh: pushing it into the storage has also let us do fewer things asynchronously (we can do caching better, since we always know the keys)
  - rangermauve: That's so cool! Is this stuff being used in the new corestore?
  - mafintosh: yep fully implemt. We’re just fixing edge cases
* pfrazee:
  - pfrazee: it’s shiptember over here. (baadeeya, coding in shiptember) conference went well, lots of positive interest. goal is to start shipping internal weekly beta builds. aiming for the first one next monday.
* rangermauve: I've been digging more into getting corestore working in the new Dat stack with hyperswarm and the such. I'm getting some bugs, but I think that's just me. :P

## Summary/Review of Previous Meeting

* [Meeting notes](https://github.com/datprotocol/working-group/blob/master/meeting-notes/FILE_NAME)
* [Action Items](https://github.com/datprotocol/working-group/issues/ISSUE_NUMBER)

#### Action Item Review:

**Skipped**

## Meeting Notes

* CLI roadmap:
  * rangermauve: okdistribute did you end up figuring out what to talk about with relation to the CLI roadmap?
  * pfrazee: yeah so I guess we need a 2.0 plan
  * okdistribute: I am excited about all of the movement with new dat 2.0 and there needs to be a champion stewarding the process
  * pfrazee: agree. I’d assume the right play is to make it a daemon client
  * rangermauve: Would anybody be interested in championing the Dat2 release? :P
  * okdistribute: yeah I agree. We talked a bit about it here and there but it'd be nice to figure out what the requirements will be and write them down more formally. readme first development 😂
  * pfrazee: I think we’re going to need to plan around the limited dev resources for it
  * mafintosh: it could just be the fuse cli. it’s basically a client
  * andrewosh: there's a bunch of CLI stuff in the client as it stands now, much of which could prob be extended. need better importing/exporting commands, for those not using fuse
  * mafintosh: Ya makes sense
  * pfrazee: yeah the trick with the dat cli is that it needs to operate outside of fuse
  * mafintosh: more than import/export?
  * pfrazee: I suspect that import/export is basically what it'll be. If you `dat .` it'll create/load a hyperdrive associated with the dir and then import
  * okdistribute: yeah
  * mafintosh: then i think we should just add that to the deamin cli
  * pfrazee: and then `dat {key} {dir}` is the same but export
  * rangermauve: If nobody else has time to do it, I'm up for submitting a PR
  * pfrazee: in that case would we deprecate the `dat` cli?
  * rangermauve: I'd rather rename the hypredrive daemon CLI to the dat CLI or something, tbh. Or make the dat CLI a thin wrapper over the daemon CLI
  * okdistribute: I had created an issue many months ago on how we could move towards a Daemon approach. that's much simpler but still largely backwards compat in terms of commands. https://github.com/datproject/dat/issues/1104 Also, basically depreciating sync/share/create and clone but they can still be there if you want to be explicit I guess
  * rangermauve: So how about this: adding a folder sync option to the daemon and it's RPC API, then starting on implementing the new CLI with those commands? I've been messing with folder sync a lot in dat-store and the SDK, so it shouldn't be too hard to figure out.
  * pfrazee: sounds right to me
  * rangermauve: Cool, I can take the lead on that unless someone else wants to. :)
  * pfrazee: I'd volunteer if I had the capacity but I dont D:
  * andrewosh:ya I see the dat CLI as an import/export/file watching wrapper for the daemon CLI, so the command structure in that issue looks really nice
  * mafintosh: is that live import /export basically?
  * rangermauve: Ideally live. So it'd need code in the daemon. I've found that live folder sync in a daemon (via dat-store) has been super nice for UX. Also, Paul recently wrote some new code for handling that use case.
  * pfrazee: you mean the diff-file-tree code?
  * rangermauve: yeah! https://github.com/pfrazee/diff-file-tree It's been working great in dat-store.
  * pfrazee: yeah that might work, might need some updates, I'd also check if mafintosh or andrewosh has code they think would be better. it's pretty solid tho
  * rangermauve: So how does a PR which adds that to the daemon sound?
  * mafintosh: why does it need to live in the deamon?
  * pfrazee: diff-file-tree def doesnt need to
  * mafintosh: A little worried about resource managwment as file watching lots is tricky
  * rangermauve: Because it should be running in the background and ideally should have access to the hyperdrive API
  * pfrazee: historically the dat cli didnt run in the bg
  * mafintosh: Instead of just running in the cli?
  * rangermauve: Yes. Do you propose having two daemons and relying on the RPC API for updating the dat?
  * mafintosh: no, just run in the fg
  * rangermauve: And not running in the BG is a pain compared to running in the BG. Especially when you want to watch a bunch of folders
  * mafintosh: Except for all the pain introduced by that
  * pfrazee: I might still suggest doing it in the fg
  * rangermauve: It's like saying the FUSE thing should be running in the FG
  * mafintosh: It’s more you can at max watch 1000 files on linux per process
  * pfrazee: yeah the FUSE is slightly different in terms of resource usage
  * mafintosh: so running it all in the bg is a lot of work potentially
  * pfrazee: yeah youd have to start/stop the watch & sync using APIs. plus PR that code into the daemon
  * rangermauve: Fair enough. Then we won't use the daemon for file sync.
  * mafintosh: understand why you’d want it, just trying to limit debugging
  * rangermauve: I'll try to reach out to the community to see what sort of features they want to have
  * okdistribute: I think it'd be good to start with simplistic updates. basically the dat 2.0 stuff is already by itself going to fix a ton of user issues. the ways we can reduce the number of commands total will also help folks
  * pfrazee: agree w/that
  * okdistribute: And then we can do another round of feature requests.
  * rangermauve: Fair enough. :) So then should I skip the folder sync and just look at having the dat CLI be a small wrapper over the daemon?
  * pfrazee: the folder sync is the dat cli, right? it's basically two commands: live import, live export
  * rangermauve: What about ditching that to start even
  * pfrazee: I figure those are the 2 commands, and like before we use a `.dat` folder to store what key it's syncing to. dat {path}` for export and `dat {key} {path}` for import`
  * rangermauve: kk. I guess all that'd be needed is creating a callbacks based wrapper over the hyperdrive RPC client? Does the daemon provide any network options?
  * pfrazee: uhh I think current diff-file-tree works with the object that hyperdrive-daemon gives
  * andrewosh: yep, can pass in bootstrapping options for hyperswarm
  * rangermauve: Well, in the CLI one of the thing that's been helpful is knowing when the sync has replicated with a peer in order to know to close the process. I guess the first version would need to ship without that? Cool, so I think that makes sense for now. I'll read into it later today or tomorrow. :)
  * andrewosh: there's a stats API that will give you lots of progress information (at the block level)
  * rangermauve: Oh good!
  * andrewosh: also added a lot in a recent push for download, so i'll add docs for all that before v10 is made stable
  * rangermauve: So that would just require polling every few MS? Or is there a push based API?
  * andrewosh: it's a push-based API using grpc streams
  * rangermauve: Perfect! :D
  * andrewosh: that part's not quite done yet, but that's the plan
  * rangermauve: Cool, I'll start with syncing from the FS and the look at what should be done for the stats
- Overview of protocol
  * rangermauve: mafintosh: Were you still up for giving a deep dive on the protocol changes?
  * mafintosh: I have 5 mins left, But I can try! tldr, updated protocol uses Noise to setup forward secure encryption. It also uses the noise session to update the capability system. By requiring each peer to share a crypto proof that they had the key. whilst fixing these security features we went through the protocol to see what we wanted to change. Since we were doign a breaking change anyway. it’s was surprisingly solid all of it. But we got rid of the weird handshake message
  * pfrazee: yeah new API is super clean
  * mafintosh: and instead made a new options message that can be used to configure per channel extensions and ack modes etc. oh and the new protocol has fully authed peers. So you can know exactly who you are talking to crypto wise
  * rangermauve: Nice. 😤 Can options be sent multiple times?
  * mafintosh: Yes!
  * rangermauve: The change in auth is due to the previous protocol using a random ID for the peer instead of a keypair, right?
  * mafintosh: yep
* Funding / future work
  * okdistribute: I'd like to apply to nlnet security & privacy. Maybe it can fund cli/core development. it's 50k
  * pfrazee: that'd be sweet
  * okdistribute: The foundation has about 30k unallocated now for the next few months, do we want to spend some of this on a new version of "how dat works"? (if anything significant has changed..)
  * pfrazee: I definitely would love for that to happen, is there anything else that we might need the money for?
  * rangermauve: oooooooo. I think so. Having docs on how the new protocol works is essential. And I think a pretty large amount has changed.
  * mafintosh: do we have a buffer after jan?
  * okdistribute: 30k is the buffer
  * pfrazee: hmm
  * mafintosh: then i would prefer to not spend it
  * pfrazee: does rangermauve's current contract end jan?
  * rangermauve: Last I heard I think it's ending around October
  * okdistribute: RangerMauve's contract ends end of October.
  * rangermauve: I've potentially got a contract with some VR folks lined up before then though. :)
  * pfrazee: were you interested in extending the contract, or are you wanting to hit the VR world?
  * rangermauve: I'd be interested. Might depend slightly on what my work would be and where stuff is heading. The VR stuff is mostly an avenue to grow P2P and Dat stuff, tbh.
  * okdistribute: it seems like the needs are around the cli deployment & making developer experience smooth by updating documentation
  * rangermauve: Yeah, I think I'd be pretty good for that.
  * pfrazee: yeah as badly as I want the "how dat works" guide updated I think ranger's work is higher value
  * mafintosh: combined with not crashing and burning 1st of jan
  * rangermauve: A potential option would be for me to go down to 10 hours a week to stretch out more. Not sure if that makes sense
  * mafintosh: maybe this is a good take home excercise for the group. to think about for next time and hit that first. these are all valid suggestions and points
  * rangermauve: Yeah, good idea. :) I'll chew on it. Worst case for me is I'll be doing Dat stuff for free in my spare time as I did before. Which isn't the end of the world.

## Decisions Made

* ...

## ACTION ITEMS

* rangermauve will get started on CLI
* everyone will think about funding and talk about it first next time
