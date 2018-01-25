# Peers

**The two of the most important concepts in a blockchain network are ledgers and smart contracts**. Recall that a ledger is like a file or database store, and a smart contract is like a program to access that store. Smart contracts and ledgers are used to encapsulate the shared **processes** and shared **information** in the business network, respectively. When a developer writes a blockchain application, the ledgers and smart contracts are used by peers to allow a network participant to provide or consume services in the network.

What makes peers important is that **they are the place where ledgers and smart contracts physically reside**. Because of this, ** peers are what actually form the physical structure of blockchain network**. Moreover, peers have connections to other peers (and other components such as orderers), and these connections are likewise a vital part of the network.  But at the core of the network is the concept of a peer, and its peers that we discuss at length in ths topic.

In passing, it's difficult to discuss peers in isolation, so some sections of this topic refer to peer related concepts -- such as orderers, channels or identity for example. However, this topic doesn't cover these related concepts in detail -- it gives you just enough detail to help you understand the concept of peers. You can find out more about these other related concepts in their own dedicated topic.  

## Peers form the blockchain network

A blockchain network is primarily formed by a set of peer nodes. Peers are the most important elements of the network because they host ledgers and smart contract chaincodes. Without peers, there cannot be a blockchain network, and they are the first physical concept to understand. Other elements are important, such as orderers, channels and policies -- you'll learn about these elsewhere, but you'll find it easiest to start with peers.

![Peer1](./Peers.diagram.1.png)

*A blockchain network is formed from peer nodes, each of which can hold copies of a ledger and copies of a smart contract. In this example, the network N is formed by peers P1, P2 and P3. P1, P2 and P3 each maintain their own instance of the ledger L1. P1, P2 and P3 use smart contract S1 to access their copy of the ledger L1.*

It's helpful to remember that the blockchain network only comes into existence when peers and certain other objects -- such as ledgers and smart contracts, but also policies, orderers, and MSPs -- are defined. In other words, **blockchain networks are not actually objects in themselves**. The administration of a blockchain network, then, really amounts to the management of these constituent objects, rather than any separate object called a "network".

**Peers are the primary objects that administrators manage on a day-to-day basis using the `peer` command**. They can be created, started, stopped, reconfigured, and even deleted. You'll see later how peers are represented as operating system processes to provide the functionality to application programs who wish to consume key services provided by a peer.

## Peers host smart contracts and ledgers

Let's look at a peer in a little more detail. We can see that it's the peer that hosts both the ledger and smart contracts. More accurately, the peer actually hosts *instances* of the ledger, and *instances* of smart contract chaincode. Note that there is a deliberate redundancy in a Hyperledger Fabric network to avoid single points of failure.  We'll learn more about the distributed and decentralized nature of a blockchain network later in this topic.   

![Peer2](./Peers.diagram.2.png)

*A peer hosts instances of ledgers and instances of smart contracts. In this example, P1 hosts an instance of ledger L1 and an instance of smart contract S1. There can be many ledgers and smart contracts hosted on an individual peer.*

When a peer is first created, it has an empty instance of a ledger, and no instances of smart contracts. We'll see later how it's necessary for a peer to install and instantiate a smart contract before it can generate ledger transactions to distribute throughout the blockchain network.

Because a peer is a *host* for smart contracts and ledgers, if a network participant (e.g. an application outside the network) wants to provide or consume smart contracts and ledgers, they must interact with a peer. A network participant might be using an application, or might be an administrator -- but it's always the peer that provides the key services that allow interactions with ledgers and smart contracts. That's why peers are considered the most fundamental building blocks of a Hyperledger Fabric blockchain network.

## Peers can host multiple ledgers

A peer is able to host more than one ledger, which is helpful because it allows for a very flexible system design. The simplest peer configuration is to have a single ledger, but it's absolutely appropriate for a peer to host two or more ledgers when required. We'll see later how peers interact with the ledger, but for now, it's easiest to think of the ledger has being hosted on the peer.

![Peer3](./Peers.diagram.3.png)

*A peer hosting multiple ledgers. Peers host one or more ledgers, and each ledger has zero or more smart contracts that apply to them. In this example, we can see that the peer P1 hosts ledgers L1 and L2. Ledger L1 is accessed using smart contract S1. Ledger L2 on the other hand can be accessed using smart contracts S1 and S2.*

Although its perfectly possible for a peer to host a ledger instance without hosting any smart contracts which access it, it's very rare that peers are configured this way. The vast majority of peers will have at least one smart contract installed on it which can query or update the peer's ledger instances.

## Peers can host multiple smart contracts

Typically, a peer will host many more smart contracts than ledgers.  Whereas there is a single ledger for each channel to which a peer is joined, there might be multiple smart contracts for every ledger hosted on a peer.  

![Peer4](./Peers.diagram.4.png)

*A peer hosting multiple smart contracts. Each ledger can have many smart contracts which access it. In this example, we can see that peer P1 hosts ledgers L1 and L2. L1 is accessed by smart contracts S1 and S2, whereas L2 is accessed by S3 and S1. We can see that S1 can access both L1 and L2.*

It's also usually the case that different smart contracts access different ledgers. That's because different ledgers usually have different data structures, although as you can see, if it's appropriate, one smart contract can access different ledgers. This is particularly useful if an organization in the network -- an insurance company, for example -- wants to be able to offer the same services to organizations on different channels.  

## Applications use peers to consume smart contract and ledger services

Applications connect to network peers when they need to access ledgers and smart contracts. A Software Development Kit (SDK) makes this process easy for programs -- its APIs (application program interfaces) make it possible to connect with ledgers, invoke smart contracts, and receive ledger notifications.

Conceptually **you can think of a peer as being similar to an operating system on the device you're using right now.** Blockchain applications use tools to interact with this operating system and can, using established rules, procedures, and connections, cause it to interact with other operating systems (i.e., other peers), similar to how the browser application on your device has interacted with the internet to bring up this Web page. We'll expand on this analogy to show how peers are also like routers in a moment.

![Peer6](./Peers.diagram.6.png)

*The peer is the access point to the network for an application. Through a peer connection, applications can execute smart contracts to query or update the ledger. Ledger queries are returned immediately, whereas ledger updates are asynchronous -- they require the consensus process to complete before the application can be informed.*

A query transaction can return its results immediately to the application because all the information required to satisfy the query is in the local copy of the ledger. Indeed, an application can connect to one or more peers in the network which hosts a copy of the ledger to issue a query, as each peer's copy of the ledger is kept up-to-date; though typically applications will connect to a single peer. It's interesting to note that for query transactions the peer does not need to consult with other peers in order to return the results to the application.

An update transaction is quite different, because a single peer cannot, on its own, update the ledger -- it requires the consent of other peers in the network. A peer requires other peers in the network to approve a ledger update before it can be applied to a peer's local ledger. This process is called *consensus* -- it is asynchronous in nature and therefore takes longer to complete than a query. But when all the peers required to approve the transaction do so, and the transaction is committed to the ledger, peers will notify their connected applications that the ledger has been updated.

Again, the SDK makes these two process easy for applications. For query transactions, applications connect to peers, invoke a smart contract regulating query transactions, and receive an immediate response. For update transactions, it's only a little more complex; applications connect to peers, invoke a smart contract regulating update transactions and receive an asynchronous notification when the ledger has been updated -- they do not need to worry about the mechanics of the consensus process.

## Peers connect to each other via channels

Although this topic is about peers rather than channels, it's worth spending a little time understanding how peers interact with each other, and applications, via channels. By analogy, think of a group of people holding hands together to form a circle. The circle is formed by the people -- it's not separate to them -- and it's like this for peers and channels.

A channel is a mechanism by which a set of components within a blockchain network can communicate and transact privately. These components are typically peer nodes, orderer nodes, and applications, and by joining a channel they agree to come together to collectively share and manage identical copies of the ledger for that channel.

![Peer5](./Peers.diagram.5.png)

*Channels allow a specific set of peers and applications to communicate with each other within a blockchain network. In this example, P1 and P2 have a communication connection with channel C. Client application A also has a communication connection with channel C. Therefore, channel C provides two-way communications between client application A and peers P1 and P2. (For simplicity, orderers are not shown in this diagram, but must be present in a functioning network.)*

We see that channels don't exist in the same way that peers do -- it's more appropriate to think of a channel as a logical structure that is formed by a collection of physical peers. Because of this channels are actually accessed and managed via peers. **It is vital to understand this point -- peers provide the control point for access to, and management of, channels**. Note that in the diagram, it's tempting to think that channels are the things that applications and peers connect to, but that's not really the case - they are a way of dividing up the network.   

A blockchain network is typically built from multiple channels, and these channels are formed by the peers that join them. Within a blockchain network, it's helpful to think of different channels as being completely separated from each other. Although this isn't precisely true, it makes it easier to understand -- and we'll cover the exceptions later.

## Peers from multiple organizations form a blockchain network

Now that you understand peers and their relationship to ledgers, smart contracts and channels, you'll be able to see how multiple organizations come together to form a blockchain network.

As you've probably heard, blockchain networks are decentralized -- they are in most cases not owned by one organization but by a collection of organizations, called a Consortium. Peers are central to how a this kind of distributed network is built because they are the connection points to the network for these organizations.  You can learn a lot more about the [consortium concept](../Consortia/Consortia.md) elsewhere in this guide.   

![Peer8](./Peers.diagram.8.png)

*Peers in a blockchain network with multiple organizations. The blockchain network is built up from the peers owned and contributed by the different organizations. In this example, we see four organizations contributing eight peers to form a network. The channel C connects five of these peers in the network N -- P1, P3, P5, P7 and P8. The other peers owned by these organizations have not been joined to this channel, but are typically joined to at least one other channel. Applications that have been developed by a particular organization usually connect to the peers owned by that organization. Again, for simplicity, an orderer node is not shown in this diagram.*

It's really important that you can see what's happening in the formation of a blockchain network. **The network is both formed and managed by the multiple organizations who contribute resources to it.** Peers are the resources that we're discussing in this topic, but the resources provides are more than just peers.  There's a principle at work here -- the network literally does not exist without organizations contributing their individual resources to the collective network. Moreover, the network grows and shrinks with the resources that are provided by these collaborating organizations.   

You can see that there are no centralized resources -- in the [example above](#Peer8), the network, **N**, would not exist if the organizations did not contribute their peers. This reflects the fact that the network does not exist in any meaningful sense unless and until organizations contribute the resources that form it. **Moreover, the network does not depend on any individual organization -- it will continue to exist as long as one organization remains**, no matter which other organizations may come and go. This is at the heart of what it means for a network to be decentralized.

Applications in different organizations, as in the [example above](#Peer8), may or may not be the same. That's because it's entirely up to an organization how its applications progress their peers' copies of the ledger. This means that both application and presentation logic may vary from organization to organization even though their respective peers host exactly the same ledger data.

Moreover, we can see that applications in a given organization access the blockchain network via the peers that the organization provides to the blockchain network.  While it's possible for one organization's application to connect to a peer in a different organization, it's not typical, and is somewhat against the spirit of a de-centralized network.

### An internet analogy

If you know a little (or a lot!) about how the internet is structured from routers, it's useful to compare this framework to how a Hyperledger Fabric blockchain is formed from peer nodes. The size of the internet (or an intranet) is determined by the number of routers. These routers can be added by anyone, with each router making the network bigger. An organization who contributes a router to the network grows the network for all despite retaining management over the piece they're contributing to it.

The same thing is happening in a blockchain network -- organizations contribute peers to the network while at the same time retaining ownership of them.  

Moreover, to extend the analogy, existing internet routers need to start routing IP packets to a newly added router. What's happening here is that the administrators of existing routers are acknowledging that the newly added router can handle internet traffic and that they -- the existing administrators -- will likewise accept packets from it.

In the same way, a Hyperledger Fabric network has a network channel policy which defines which organizations can add peers to send and receive ledger updates (the "traffic" of a blockchain network). It's a little nicer in Hyperledger Fabric though, as there is a policy which defines which organizations are allowed to contribute peers (and orderers) to a network. The process of peer addition to a network does not require administrator intervention on a peer by peer basis.

Analogies are never perfect of course -- the key point here is one of decentralization -- multiple organizations come together to contribute resources, and agree between themselves how the network in managed. **There is no one organization that owns or controls the network**, and even though this makes it a little more difficult to understand and manage decentralized networks, there are significant benefits in terms of resilience from a technical and business perspective.

## A peer's identity determines its organizational membership

Now that you've seen how peers from different organizations come together to form a blockchain network, it's worth spending a few moments understanding how peers get assigned to organizations by their administrators.

Peers have an identity assigned to them via a digital certificate from a particular certificate authority. You can read lots more about how [X.509 digital certificates](../KeyConcepts/Identity.md) work elsewhere in this guide, but for now think of a digital certificate as being like an ID card that provides lots of verifiable information about a peer. **Each and every peer in the network is assigned a digital certificate by an administrator from its owning organization**.

![Peer9](./Peers.diagram.9.png)

*When a peer joins a channel, its digital certificate identifies its owning organization via a channel MSP. In this example, P1 and P2 have identities issued by CA1. Channel C determines from its policy that identities from CA1 should be associated with Org1 using ORG1.MSP. Similarly, P3 and P4 are identified by ORG2.MSP as being part of Org2.*

Whenever a peer connects to a channel in a blockchain network, **its identity is checked against the channel policy to determine its rights as dictated by the organization it belongs to**. The mapping of identity to organization is provided by a component called a [Membership Service Provider](../KeyConcepts/Membership.md) (MSP) -- it determines how a peer gets assigned to a specific role in a particular organization and accordingly gains appropriate access to blockchain resources. Moreover, a peer can only be owned by a single organization, and is therefore associated with a single MSP. We'll learn more about peer access control later in this topic, and there's a entire topic on MSPs and access control policies elsewhere in this guide. But for now, think of an MSP as providing linkage between an individual identity and a particular organizational role in a blockchain network.

And to digress for a moment, peers as well as **everything that interacts with a blockchain network acquire their organizational identity from their digital certificate and an MSP**. It's more than just peers though -- applications, end users, administrators, orderers must have a identity and an associated MSP if they want to interact with a blockchain network. **We give a name to every entity that interacts with a blockchian network using an identity -- a principal.**  You can learn lots more about [principals](../KeyConcepts/Principals.md) and [organizations](../KeyConcepts/Organizations.md) elsewhere in this guide, but for now you know more than enough to continue your understanding of peers!

Finally, note that it's not really important where the peer is physically located -- it could be resident in the cloud, or in a data centre owned by one of the organizations, or on a local machine -- it's the identity associated with it that identifies it as owned by a particular organization. In our example above, P3 could be hosted in Org1's data centre, but as long as the digital certificate associated with it is issued by CA2, then it's owned by Org2.

## Peers use orderers to collect, package and distribute ledger updates

We've seen that peers form a blockchain network, hosting ledgers and smart contracts which can be queried and updated by peer-connected applications.

Moreover, applications which generate ledger updates have to wait for these changes to be validated by the other peers in a channel before the application can be notified that the ledger update is complete.  Orderers are central to this process -- they receive the proposed ledger updates from an application, package them into ordered blocks, and redistribute these blocks to all peers in a channel for validation and inclusion on the ledger.

![Peer10](./Peers.diagram.10.png)

*The role of orderer nodes in a blockchain network is to distribute ledger updates. In this example, application A1 sends a proposed ledger update U1 to the order O1. O1 packages and redistributes U1 to peers P1 and P2 on channel C. P1 and P2 each validate U1 to ensure that it has been properly endorsed, and applies it to the ledger. For simplicity, we do not show blocks in the ordering process.*

The relationship between peers and orderers is important to understand.  The process of updating the ledger is actually split into two phases. In the first phase, a peer is used by an application to generate a proposed ledger update which is signed by the peer with its private key -- in a process called *endorsement*. The second phase is called *validation*, and involves every peer in a channel (including the originating peer) checking these proposed updates to make they conform to the *endorsement policy*. Specifically each ledger update generated by a smart contract must be validated according to its endorsement policy before it is applied to each peers' local ledger.

It's the orderer than connects these two phases together. An orderer continuously receives multiple proposed ledger updates originating from many different points in the network. It's the orderer's job to arrange these proposed updates into a well-defined sequence, and package them into *blocks* for distribution.  Once a block is full of updates, due to size or elapsed time, it is sent to all peers in the channel -- where it is then unpacked, validated according to the endorsement policy, and finally applied individually to each peer's local copy of the ledger. This entire process is called *consensus* because every peer is checking that it is happy that ledger updates are well-formed before they are applied to their local copy of the ledger. It's also a multi-step process, which is why applications are notified asynchronously when ledger updates are complete, rather than when proposals are generated from smart contracts.

We can see that whereas peers host the ledger and optionally smart contracts, orderers most definitely do not -- they only hold a set of the most recently collected blocks, to enable temporarily off-line peers to receive updates they might have missed after a short communications outage. Orderers are responsible for the simple but vital processes of collecting proposed updates from applications, ordering them, packaging them into blocks and distributing blocks of proposed ledger updates to peers, which update their local ledgers. It is these peers, rather than the orders which inform applications when these ledger updates are complete.

We discuss [orderers](../KeyConcepts/Orderers.md) in a lot more detail elsewhere in the guide, but for now, think of orderers as nodes which collect and distribute proposed ledger updates from applications for peers to validate and include on the ledger.

### Review to this point

### Following material to be incorporated



## Peers and channel policy

In fact there are 2 MSPs at play when a peer joins a channel.  The first MSP is local to the peer, and it's where the peer's digital certificate is stored. This MSP actually has very little information held within it, just the individual peer's digital identity including, most importantly, it's digital certificate which can be shared, and it's private key, which absolutely must not be shared.

The second MSP is global to the channel, and it has lots of information in it. Each organization in a network has an MSP definition in the channel, which is used to associate peers to organizations.  Moreover, whenever a peer

You might have noticed that the example above does not show an orderer, but as you've seen, orderers -- like peers -- are not necessary to form a blockchain network on a logical level (just as the rules of the road can be written before there are highway interchanges).

are connected to all channels, and

Orderers can distribute updates as well as queries - any result set can be logged; but this detail for later.


The physical structure of the peers and orderers separates them from the logical framework of the policies that govern how peers and the orderer nodes interact. This is similar to how a *set of laws* -- the rules of driving, for example -- are different from the people, cars, and physical structures that actually make up the *highway system*.  



## Peer Container  

When a peer does host smart contracts.

That's not the whole story, however, because these different *physical* copies of the ledger need to be kept synchronized with each other, so that they can be considered to be the same *logical* ledger. This is achieved by a process called consensus -- which keeps the individual physical ledgers in step with each other. We'll discuss the logical/physical nature of the ledger and smart contracts later, as well as how peers take part in the consensus mechanism. First let's understand a little more about individual peers.

## Applications and peers

The peers in a Fabric network function both as **providers** of services to the network and as **consumers** of those services. A rough analogy might be to a co-op -- a grocery store where the people who shop there are also the stockers and cashiers.

This combination of tasks and responsibilities makes the exact "role" of a peer somewhat difficult to pin down -- it has, as a practical matter, **many** roles. Consider that in the course of a transaction flow, a peer might be involved in all of the following:

  1. Proposing the transaction (either directly through its own application or as a medium for an outside application using the peer as an access point to the network).
  2. Checking the endorsement policy for the transaction to make sure the correct number -- or a particular set of peers specified in the endorsement policy -- have signed the transaction.
  3. Executing a version check of the ledger (held by the peers on a channel -- more on this later) to make sure that there have been no other transactions written to the ledger since the proposal was made.
  4. Receiving the transaction response from the ordering service.
  5. Writing the transaction to its ledger (assuming all goes well).  

Peers are owned and controlled by consortia members. Each member organization can have one peer or several, depending on its needs. Within an organization, one peer must be specified as the **Leading Peer**. It is this peer that interacts with the network at large and disseminates ordered transactions to the other peers in its organization. If this peer goes down, another peer within that organization will take over its role until the leading peer is back functioning. For the purpose of communication between organizations using gossip, an **Anchor Peer** is also specified. This peer will often but not necessarily be the same peer as the leading peer. We'll talk more about gossip later.

## Peers and the Ledger

Ledgers are stored on a peer's file system (either locally or externally depending on the type of database the peer is using). Peers will have a separate ledger for every channel they're a member of.

Link here to TheLedger.md.


## Peers and Identity

Local MSP (at the peer level). Who is allowed to do what on that peer? The peer will have the same MSP as its organization (since peers belong to organizations). Will go back to the RCA of the ORG. This org msp structure is what allows intraorganizational gossip and the function of the leading/anchor peers.

Peers will also maintain Global MSP for every channel they're a part of. Regulates permissions on those channels. Peers may be a part of many channels and will maintain a Global MSP for each channel.

Sign certs.

Also, at the channel level -- what are admins for that peer allowed to do on the channel (instantiate chaincode, for example -- implicit metapolicies)?



## Starting and Stopping Peers

The state database will automatically get recovered (or generated if needed) upon peer startup, before transactions are accepted.



## Joining a Channel

Get the ledger from the Leading Peer. Or, if there's no Leading Peer, from the orderer. It could also be gotten from another Anchor Peer in a channel, but this is not a best practice.

Difference between being added to a channel that's already running and being part of the founding of a channel. If peer is part of the creation of a channel, it's ORG MSP will be part of the initial config block of the channel. If peer is being added to a channel that doesn't recognize its ORG MSP, the MSP of that ORG must be added by the admins of the channel.

Specify that even if ORG1 and ORG2 use the same RCA -- Verisign, for example -- the specific ORG MSP must be added to the channel first (since peers must be tied to an organization). *I think that's right, anyway*.


## Changing a Peer

All of the components in a Fabric network have a version.

What are the configurations of a peer that are optional (ie **can** be updated)? Admin policy.



## Gossip

Peers leverage gossip to broadcast ledger and channel data in a scalable fashion. Gossip messaging is continuous, and each peer on a channel is constantly receiving current and consistent ledger data from multiple peers. Each gossiped message is signed, thereby allowing Byzantine participants sending faked messages to be easily identified and the distribution of the message(s) to unwanted targets to be prevented. Peers affected by delays, network partitions or other causations resulting in missed blocks, will eventually be synced up to the current ledger state by contacting peers in possession of these missing blocks.

The gossip-based data dissemination protocol performs three primary functions on a Hyperledger Fabric network:

    1. Manages peer discovery and channel membership, by continually identifying available member peers, and eventually detecting peers that have gone offline.
    2. Disseminates ledger data across all peers on a channel. Any peer with data that is out of sync with the rest of the channel identifies the missing blocks and syncs itself by copying the correct data.
    3. Bring newly connected peers up to speed by allowing peer-to-peer state transfer update of ledger data.

Gossip-based broadcasting operates by peers receiving messages from other peers on the channel, and then forwarding these messages to a number of randomly-selected peers on the channel, where this number is a configurable constant. Peers can also exercise a pull mechanism, rather than waiting for delivery of a message. This cycle repeats, with the result of channel membership, ledger and state information continually being kept current and in sync. For dissemination of new blocks, the leader peer on the channel pulls the data from the ordering service and initiates gossip dissemination to peers.

Online peers indicate their availability by continually broadcasting “alive” messages, with each containing the public key infrastructure (PKI) ID and the signature of the sender over the message. Peers maintain channel membership by collecting these alive messages; if no peer receives an alive message from a specific peer, this “dead” peer is eventually purged from channel membership. Because “alive” messages are cryptographically signed, malicious peers can never impersonate other peers, as they lack a signing key authorized by a root certificate authority (CA).

In addition to the automatic forwarding of received messages, a state reconciliation process synchronizes world state across peers on each channel. Each peer continually pulls blocks from other peers on the channel, in order to repair its own state if discrepancies are identified. Because fixed connectivity is not required to maintain gossip-based data dissemination, the process reliably provides data consistency and integrity to the shared ledger, including tolerance for node crashes.





## Peers in DRIVENET

Regal might choose to have two different peers to put on the two channels they're on (but they don't need to).




          + What is a peer?
          + Peers and the ledger
          + Peers and Identity
          + Starting and Stopping Peers
          + Joining a channel
          + Changing a Peer
          + Peers in DRIVENET

[Next: Chaincode](./Chaincode.md)
