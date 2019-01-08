# Lighthouse Call 0

#### Meeting Date/Time: Tue, Jan 08, 2019, 22:00 UTC
#### Meeting Duration: ~30 Minutes
#### [Github Agenda Page](https://github.com/sigp/lighthouse-pm/issues/1)

## Agenda

1. Introductions
2. General update (PH)
3. Spec progress update (PH)
4. libp2p update (AM)
5. Future meeting frequency
6. Refining direction of `beacon_state` dir.

---

## General Updates

* **@Paul**:
    * **Primary goal is testnet in March.**
    * Q1 2019 was the date that was set.
    * Focus is on beacon chain - 4 core tasks.
        * Implement spec.
        * Networking - Libp2p/Gossipsub.
        * Syncing.
        * RPC.
    * After the syncing and libp2p are up we can have blocks over the network.
    * Validator client is separate client/binary - starting late Jan/early Feb.
    * Past few weeks - working to isolate the 4 main areas to work in parallel.
        * PR [#129](https://github.com/sigp/lighthouse/pull/129) Brings us closer.
    * **@Paul** working on chain structure (interface between spec and syncing).
    * Resourcing:
        * Project has been running lean over the past couple of months, this is due to security assignments at SigP that needed attention and also a desire to reduce duplicate work as the spec changes.
        * No burning runway, good for project in long run but need to start scaling up and ramping up some work for this year.
        * We have the equivalent of 2x full time devs this year, looking to apply more.
        * Crunch time for the project - not here yet.
        * Working but not working hard - Crunch time = **February/March**.
        * Have all ready for crunch time.
        * Who is doing what:
            * @Paul: Project Management, RPC, whatever.
            * @Age: libp2p; syncing;
            * @Luke: infrastructure (CI);
            * @Mehdi (Not present): spec; more engaged later.
            * @Grant, @Michael, @Stan: helping externally doing great work.
            * If you want to help, please check out the "Good first issue" issues or jump on the Gitter.
    * Spec Changes:
        * Spec has been changing over the past few months, but EF is showing signs that changes will be less frequent now.
        * Consulting with [Danny Ryan](http://github.com/djrtwo) - Lighthouse going to resume spec implementation.
        * Danny has advised to work on state transitions - @Alex is working on that.
        * Paying attention to the validator client in the beacon chain.
        * Expected Changes to spec:
            * New Shuffling.
            * Bug fixes.
            * attack mitigation coming through as people start getting more sophisticated.
    * **@James**
        * Looking for good first issues, should we go through the eth2.0 spec first?
        * Any preliminary reading that's recommended?
        * **@Paul**:
            * Look at the docs ([README](https://github.com/sigp/lighthouse/blob/master/README.md)/[onboarding docs](https://github.com/sigp/lighthouse/blob/master/docs/onboarding.md))
            * Recommend going through the spec because it's a large task.
            * Good thing to read through and get an understanding.

## Libp2p/Gossipsub

* **@Age**
    * Absent from the channels and the main repo in the last few weeks - dealing with company stuff, hoping to be around more.
    * Any questions hit up in the gitter or repo stuff.
    * Playing around in an entirely different repository - LibP2p repo.
    * Not sure about familiarity with people - current trend is libp2p (networking stack) pubsub protocol called gossipsub.
        * Allows nodes to communicate without too much duplication on the network.
    * **@Stan**: How does it differ from floodsub?
        * Floodsub is a superset on how it handles peers; It sends message to all nodes that it has.
        * Gossipsub minimizes the amount of duplication and having a smarter way of sending messages without spamming the network.
        * More efficient in the transferring of messages.
    * Current state of things:
        * A lot of the other clients (JS, Nim, ..) there's no native Libp2p implementation for the networking backend.
        * Libp2p created a daemon to run independently and interact with it - it has the functionality that is needed.
        * There is an effort to implement all in native Rust. Being worked on by the Parity team.
        * Idea is to figure out what the Parity people were doing and implement the GossipSub in Rust Libp2p.
        * There are a couple of PRs to try and get Gossipsub in the libp2p.
        * Working towards making it so that it can be a crate to import into Lighthouse.
        * If there is anyone interested in this stuff - happy to work together.
    * Update:
        * Getting head around the implementation.
        * Hopefully in the next few weeks be actually building some of the stuff.
    * **@Stan**: Anything blocked by the implementation of Libp2p/Gossipsub?
        * We can (a) stub it for now or (b) use the daemon and interact with that if it ever occurs.
        * Currently there are no actual issue that are blocking because of this.
        * We always have the Go daemon that we can always use.
        * If anyone is keen on working on the networking and syncing, can use the "Floodsub" but we can upgrade to "GossipSub" when ready.
        * Libp2p is very modular - Transport trait that gets upgraded to the protocol.
        * Easy to add and remove things - makes for some interesting Rust.

## Future meetings

* **@Paul**:
    * Host this call every 2 weeks to give an update on everyone.
    * Each week the team members can drop a few lines on the Gitter to help people to understand where everything is.
* **@Alex**:
    * Gitter can be used for the syncing.
    * Fortnightly calls? Start and see where it goes.
* **@Paul**:
    * Go ahead with the **2 week** call schedule.

## Beacon State directory changes
* **@Paul**:
    * PR [#129](https://github.com/sigp/lighthouse/pull/129) - moved the chain structure from `beacon_chain/` into `lighthouse/`.
        * It's a component that works with the database and helps to manage the spec.
    * Turn the `beacon_chain` directory into a workspace (maybe `eth2`) that contains a bunch of crates that do a bunch of Serenity stuff.
        * Validator assignment, bits and pieces that will be useful for other people.
        * Divisible enough such that the validator client can implement parts of it without having to import the entire thing - don't want to have the 4 lines of code in a crate.
    * **@Alex** raised a point for the crate to be "stateless" - maybe if dealing with state it can read closures and be passed the state.
    * @Alex supports.
    * @Stan - cool with him too (maybe biased because spec is still in making)
        * Previous remarks could have been harsh because of how hard it is to keep up.
* **@Paul** looking to merge PR [#129](https://github.com/sigp/lighthouse/pull/129) and make an issue to rename the crate from `beacon_chain` since Lighthouse will need a `beacon chain`.

## Misc

* **@Stan**: Looking for an issue that can be picked up and done.
* **@Age**: in general do you think we should be making more "Good first issues" - lots of people chasing to start and looking for things to work on.
    * **@Paul**: Thinks it's quite a good point.
    * **@Luke**: Maybe we should aim for 5 or so "Good first issues" at a time for people to pick up.
* Figuring out next meeting time and will publish.
* Call recording is going to be published.

## Participants

* [Paul Hauner](https://github.com/paulhauner)
* [Age Manning](https://github.com/agemanning)
* [Alex Stokes](https://github.com/ralexstokes)
* [Luke Anderson](https://github.com/spble)
* [Kirk Baird](https://github.com/kirk-baird)
* [Grant Wuerker](https://github.com/g-r-a-n-t)
* [Johns Beharry](https://github.com/johnsbeharry)
* [Stan Drozd](https://github.com/drozdziak1)
* [Michael Keating](https://github.com/mjkeating)
* [James Zaki](https://github.com/jzaki)
* [Chris Natoli](https://github.com/natolichris)
