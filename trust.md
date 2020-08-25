# Verifiable and trustworthy

One of our goals was to make a verifiable and trustworthy way to prove the origin of the electricity.

## Distributed ledgers

Distributed ledgers and blockchains are often mentioned as a way to create transparency and make data publicly verifiable, which in turn should make it trustworthy. Data written on the ledger cannot be changed without everyone knowing it, and depending on the setup, can be very transparent in what is happening and how to verify it.

But there still is a central issue between integration the digital and physical world. 

If a central authority like the danish DataHub is writing all the data, does a ledger make it more trustworthy? The data written to the ledger can still be incorrect.

If decentralized meters are writing the data directly to the ledger, how do we trust the meters? How can we trust that it is installed correctly? How can we trust it does not have a backdoor that enables data to be changed before writing it to the ledger?

## Trust

A central core issue is trust, i will not go deep into this topic, since it is huge, and is something we are also currently exploring together with IOTA Tangle EE project on digital identity.

Trust in the real world, is based on the institutions and patterns you know. You know how a passport or driving licence should look. You trust that the local government that has issued the driving license has checked that the person with it has passed the necessary tests and so on.

But what about in the digital world, how do we initiate trust?

How can you validate that some claim is indeed true?

### Trust Anchor

Trust anchors are identities that you already trust (local government). If your local government issues you and your fellow citizens claims of your age, driving licenses and so on, you should be able to accept that on a similar level as a physical proof.

This is similar to the issues above. 

Institutions you trust can issue claims to meter producers, that the meters they produce have been tested and validated to provided valid measurements.

Trust anchors can also issue claims to meter-installers that they are authorized, the installers can in turn sign a specific meter that it is installed correctly and tested.

But it is important to note that all systems will allow unlawful behavior if people who have been trusted position misuses their position.

# Important take away

Trust cannot be solely based on the ledger, the ledger is only as trustworthy as the actors writing the data to it.

There are still a lot of unanswered questions around how we can establish the trust in this sort of system.

To be able to continue the prototype, we chose to define the DataHub as a trusted source of data.
