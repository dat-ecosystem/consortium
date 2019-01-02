# dat protocol WG (#18)

*DATE via IRC (#datprotocol)*

[Agenda discussion](https://github.com/datprotocol/working-group/issues/36)

## People

* mafintosh
* pfrazee
* jhand
* yoshuawyts
* bnewbold

## Agenda

* Review last meeting/action items
- [Agenda discussion](https://github.com/datprotocol/working-group/issues/36)

## Summary/Review of Previous Meeting

* [Meeting notes](https://github.com/datprotocol/working-group/blob/master/meeting-notes/17-24October2018.md)
* [Action Items](https://github.com/datprotocol/working-group/issues/35)

#### Action Item Review:

* respond to DEPS#45 to explore the .well-known/dat solution (@pfrazee)
    * pfrazee: "that's done, I basically replied that we'll give this another look. I haven't done any work on it but I expect we'll revisit the spec when the timing is right"
* respond to the email thread behind datproject/security-discussion#1 (@mafintosh)
    * pfrazee: "mafintosh: you wrote up the majority of a response, didn't you? Is there some way you can just point to that response?"
    * mafintosh: "yep"
* write up some notes on spec changes for hyperdb (simplifications & single-writer) (@mafintosh)
    * Decided to drop this since the changes are taking a while, will get a status update in the meeting.
* get hyperdb update ready for vote (@bnewbold)
    * voted, merging
* prioritize having beaker leave a swarm when you leave the page (@pfrazee)
    * pfrazee: "not done yet"
* security/privacy docs update (@joehand)
    * jhand: "not done yet"
* OTF red team audit application (@joehand)
    * Removed item, will handle in the future
* try using dat over tor (@pfrazee)
    * Removed item
    * pfrazee: "Metadata privacy is a priority but will need dedicated resources that I dont have atm"


### Maf's update on the protocol work

&lt;mafintosh&gt; We are integrating hypertrie into hyperdrive directly<br>
&lt;mafintosh&gt; This closes all existing bugs / scaling issues with the data structures<br>
&lt;mafintosh&gt; But keep the api the same which is great<br>
&lt;mafintosh&gt; Then we integrate the hypertrie into hyperdb with a union data structure<br>
&lt;mafintosh&gt; Gives us multiwriter<br>
&lt;mafintosh&gt; Then switch out hypertrie with hyperdb in hyperdrive<br>
&lt;pfrazee&gt; will the hypertrie update in hyperdrive be a major version bump on the hyperdrive structure?<br>
&lt;mafintosh&gt; yes, a needed one<br>
&lt;mafintosh&gt; but the subsequent multiwriter one will be a minor bump on the data structure


## ACTION ITEMS

* Gist/blogpost about scaling hypercore and what did/didnt help (@mafintosh)
* Review Sleep Headers DEP https://github.com/datprotocol/DEPs/pull/56 (all: @pfrazee, @bnewbold, @mafintosh, @jhand, etc)
* Finish wire protocol DEP (@bnewbold)
