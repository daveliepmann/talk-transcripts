# Deconstructing the Database (JaxConf)

* **Speaker: Rich Hickey**
* **Conference: [JaxConf 2012](https://jaxconf.com)**
* **Video: [https://www.youtube.com/watch?v=Cym4TZwTCNU](https://www.youtube.com/watch?v=Cym4TZwTCNU)**


[Time 0:00:11]

This talk is deconstructing the database and what it is is a look at a new way to look at database architectures. It happens to also be a look at the underpinnings of the architecture of Datomic, which is the database I've been working on for the last two years. But it's not a sales pitch for Datomic. It's really about the ideas underlying the design choices.

So why do we want to deconstruct the database? What are we trying to accomplish? What problem are we trying to solve? I think the fundamental problem we're trying to solve is the problem of complexity in programming. I mean, how many people think dealing with databases is easy and trouble-free?

[Time 0:01:00]

Nice. Most people don't. And there are a number of sources of complexity. There's this great paper Out of the Tar Pit, and in it the authors sort of identified a bunch of problems related to complexity in programming. They basically said that all complexity boils down to two flavors. One has to do with state and the other has to do with control. The authors didn't implement it, but they suggested that by adopting functional programming and declarative programming and a relational model for data inside our applications, we could get rid of this complexity. It's a great paper, I really recommend you read it.

But one of the problems with the paper is that while they had a good grip on the functional programming and declarative programming part, and also possibly on using a relational model for data inside your applications, they really sort of punted – everybody seen the cartoon where the mathematician has this chalkboard and it's full of stuff and then in the bottom corner it says "and then a miracle occurs"? Right. And there's the answer.

The big thing that was missing from their picture of the world was they imagined there would be this relational model of your data that you could access in your application and that somehow it got updated. Like, somehow something happened in the world and it was different. And that all the “ick” related to state had to do with however that got updated, but they didn't say how it would. And I would call that "updating," and the problem that they avoided talking about really is the problem of *process* in our programs. In other words, we know there's going to be novelty in the world that programs are going to encounter. Where does that go in a model that's otherwise functional? or as functional as we could make it?

[Time 0:02:53]

So some of the other problems we're trying to solve are things we want to obtain in looking at the database in a fresh way. I think [one] is to embrace declarative programming. I agree with the paper, we want declarative programming. What's the best example of declarative programming we encounter most often if we're not artificial intelligence researchers?

SQL, actually, is the most declarative thing most of us encounter on a day-to-day basis. And it ends up that declarative programming is much better at manipulating data than what we do in our languages, even functional languages. In our languages we sort of go through stuff, but we do not have this very nice higher-order set logic for dealing with data. And it is superior to dealing with data even in a functional language, and way superior to dealing with data in an object-oriented language. We use object-oriented languages because we have them, not because they're better at this. They really are much worse at this.

One of the problems we have is that with a client-server database, declarative programming is something that's alien to us. It's sort of "over there".

[Time 0:04:02]

The other problem related to the model these guys were espousing is that there's no basis, right? If I want to calculate something related to a database, what's the basis for that calculation? If the whole database is sort of changing all the time, we're back to the problems I talked about in the keynote. The database in its entirety is a _place_, and we have a problem of saying what's the basis for our decision-making. "Well, I don't know, it was whatever I saw last Tuesday when I ran this computation, but I can't tell you now what that was exactly."

There are problems with databases related simply to the client-server nature, about them being over there. The basis problem is one of them. The other is our fear of round trips. We're afraid of round trips, I think most often for performance reasons. But actually the biggest round trip problem is that same basis problem. What if I have a composite decision to make? Can I ask the database three independent questions over time and then get my answer? No. Why not? Because stuff has happened to that place in between those calls.

And it makes us do weird stuff. In particular, one of the things I think it makes us do is couple questions with reporting. Let's say you have your application and your application makes a decision about what entities we're going to put on sale or display on this web page. Do we ever send the query to find out what those entities are, and then later send a query to gather the data we need for display? No. A lot of times we piggyback those two things together because we're afraid of the result sets not matching up anymore. That's actually a fear that's born of the lack of basis again, and it's the biggest problem with round trips.

And of course from a design perspective we know those two pieces of logic should be separate. They should be independent decisions. One part of my app knows the logic for deciding what should be displayed, and the other one knows about what does the screen look like and what do we want to show on it.

[Time 0:06:00]

We have problems related to consistency and scale. I don't know if anyone saw the NoSQL talks this week, but a lot of times we have difficulty scaling servers that are monolithic by default. We've seen NoSQL, we've seen the Dynamo paper and some of these other technologies. And I think one of the questions we have in revisiting the architecture of a database is: what's possible? How much of the value propositions of databases can we retain while tapping into some of the new value propositions of distributed systems, in particular their arbitrary scalability and elasticity?

Also, people are sort of adopting distributed systems and getting a bunch of complexity as a result, because they're trading off distribution and scale for consistency. They're losing consistency in the tradeoff. But we have things like Dynamo and BigTable. How do we use them?

[Time 0:07:00]

Other problems we have in general when we talk about traditional databases are flexibility problems. Everybody knows the rigidity of relational databases and the big rectangles, and the artifice you have in having to form intersection record tables and things like that, things that you really shouldn't have to know about. Your application ends up becoming rigid because it does know about those. In addition, lots of things are difficult to represent in a traditional relational model, like sparse data, irregular data, hierarchical data, things like that.

So we want to be more flexible, we want to be more agile in our development, and we want to try to avoid this rigidity seeping in.

[Time 0:07:46]

And of course, related to the talk earlier, another thing we want to try to get right if we revisit database architecture is information and time. In particular, we want a database that we can use to represent information, that we can use to obtain real memory and real record keeping like we used to have before we had computers. There are lots of good reasons for this. It helps support decision-making, as I said in my talk before, and auditing. There are plenty of domains in which it's a requirement, and people are doing this manually on top of systems that don't really understand that that's what you're trying to do.

How many people have ever added a timestamp field themselves to tables and managed it all themselves? Right. How many people have written the query that gets you "now" out of that table? Yeah. How many people have tuned that? Yeah. That's a nightmare. Anybody like that query, tuning that query? It' brutal. The contention is terrible, especially if it's also an online system.

[Time 0:09:00]

And the last thing I think we'd like from databases that maybe we don't think about now, because we don't necessarily connect the two, is a strong model for perception and reaction. Perception is part of what I was talking about before, getting that stable basis for decision making. Reaction is more like eventing. Things are changing in the world. How do we see change in a traditional client-server database? What do we do?

It's the big four-letter word that begins with "p" and ends with "O-L-L." Poll. We poll. Very gross.

So we'd like to be able to make reactive systems that don't poll. And we'd like those systems to get consistent views of the world, which is another difficult thing. Even if you build manually a trigger-based eventing system — people have done that, right, with triggers, and they say "oh something changed". Okay great, but in between when they told you something changed and you're wanting to make a decision on the basis of it, maybe you're going to go back to the database. What's the basis now for that? Did it change again? What's in flight? You have no way to know this changed and that change was related to the database at this point in time, and you can go back and ask questions to figure out what was going on, and what either caused that change or what the effects of that change should be or how it relates to the rest of the world. You have no way to do that. You just were told something changed, and maybe a value about _it_, but not where that's situated relative to the rest of the world. We want to do that better.

[Time 0:10:16]

So if we're going to take a database apart, we have to look at how it's put together. This is not a particular database we're talking about here, but this should be familiar to anybody who's dealt with traditional databases. I'm sorry if this font is too small.

The guts, the meat of it, is at the bottom, so we'll start at the bottom. A database is certainly something we expect to be durable. So most traditional databases are built around: there is this disk, and we're going to put that disk in a box, and that box is going to be in charge of the disk, and everything comes from there. So there's some I/O subsystem that deals with the disk.

And then there are two fundamental sets of services the database will provide. (They both rely on this third.) The first is there's a transactional component that accepts novelty and integrates novelty into the view of the world. And then there's a query-support component that accepts questions and gives you back answers based upon the values here. And both of them, especially the query side, need to have storage. Like, if you just took everything that came into a database and you just appended it to a flat file, how good would the query engine be? Not very, right. So leverage comes from indexing. Leverage comes from organizing the way we store the data such that a query engine has sorted views of things that it can use to answer questions quickly. And that's the leverage of a database.

(I think we've gone to key-value stores that have almost no leverage and we're still calling them databases. But in my mind, this is what made a database a database. Otherwise we had file systems and all other kinds of things before we had databases, and we didn't call them databases. Why are we calling key-value stores databases now?)

[Time 0:12:20]

So in general, traditionally this was a big monolithic thing. There was a big, sophisticated or complicated process that knew about all this stuff and had an integrated view of how they would work. And because this was expensive, and the memory it needed was expensive, and the box on which it ran was expensive, this was a very special thing. You had one of them. And then you had clients, which were somewhat more lightweight, and they communicate using usually a foreign language. How do you communicate with a SQL database? Strings in a foreign language. You send it over and it does something. Same thing: how do you communicate queries? You send strings over, in a foreign language, and you get back — well, who knows. Maybe the API makes it look like a result set to you up at the Java level.

And then we know as we get a lot of apps going, this unique resource gets taxed. Everybody's putting all the data in there and everybody's asking questions there. We know the questions dominate -- in most applications they are read-oriented. So most applications eventually end up adding another tier. If it's very costly for me to ask questions, I'm going to store the answers to those questions in a cache. So maybe next time I want to ask that question I'll check the cache first. Otherwise I'll incur the cost of going all the way to the server.

And what goes in the cache? What form does it take? When does it get invalidated? Whose problem are all these questions?

[Time 0:14:00]

Yours. Your problem. Or maybe you buy into some fancy ORM that makes it your problem with another layer on top of your problem. Now you have two problems. There's no...who knows. It's definitely not the server's job. I would call this caching _over_ the database.

And there are some other things that a database comes with that we don't necessarily think about. Certainly most databases have a data model. It can be a really low-level thing that is about how things are stored, or an API kind of thing, or it can be a relatively high-level thing. It's certainly a great trait of SQL databases that they're based upon a mathematical foundation in relational algebra. That is a proper data model with a bunch of great characteristics that allow you to write those declarative programs.

But they also contain a state model. And in fact relational algebra is a lot like that old paper. Relational algebra is like "perfect", it says, "there is this state of the database and all this algebra applies. It's math, it's great." How do you get a new state of the database? Well, a miracle occurs, and then you have a new relational world, and you have that. But update is not mathematical. There's not the same model behind it. So there is a state model, and in general -- not all the time -- that's an update-in-place model that's subject to all the kinds of criticisms I gave in the keynote.

[Time 0:15:41]

What's usually missing is an information model. And here I mean something precise. By an information model I mean the ability to store facts, to not have things replace other things in place, to have some temporal notion to what's being stored. That's what I would consider a true information model. That's usually missing from databases.

So we want to solve all of this. We want more scalability, we want to try to leverage these new systems, we'd like to have more declarative programming in our applications, we'd like to have a proper information model. Maybe we don't want to program with strings anymore.

What are the challenges we're going to face if we tried to do that? The biggest one, by far, is definitely the state model, the fact that it's update-in-place. And again as I said in my talk, there's a great reason why traditional databases work the way they do. Because when they were invented 30 or more years ago, these resources were scarce. You couldn't make a database that said "I'll just keep everything," because you had this tiny little disk. So they invented all this update-in-place technology. Usually inside a database there are B-trees, they use blocks on the disk, they'll reuse the blocks, they'll fill the blocks, they're rewriting them, they're usually interacting at a pretty intimate level with the memory management on the computer. And because they're updating in place and they're trying to serve multiple clients, they have a huge amount of coordination overhead to do that, and that slows them down significantly.

[Time 0:17:25]

So the approach we're going to take in trying to break things apart is going to be based on these three principles. One is to move to an information model. Now I made claims during my talk that using values and having an information model has architectural implications. And if you take away nothing from this talk I would hope you would take away that this is a _real_ example of that in action -- that adopting a value-oriented model has architectural benefits, really substantial ones.

So we're going to move to an information model. We'll see how that plays out. We're going to split process and perception — I have a diagram later that will make that clearer. We're going to treat our use of storage immutably; in other words, we're going to store stuff but once we've stored it we're not going to change it. And the other half of doing that is that in order to deal with process, we're going to have to manage novelty in memory for a window of time. And I'm gonna break all these down.

[Time 0:18:36]

To move to an information model means to move to a data model that is fundamentally about facts. So we're going to say we're going to have a database of facts. That means sucking the structure out. Because when you look at a relational row or a document, there's nothing _fact_-like about that. It doesn't say 'when'. And the granularity of the fact is this composite thing. So if I had a whole row for you and you changed your email and email is one of the columns, the row is not a fact. It's not the granularity of a fact. It's bigger than a fact. Maybe it's a set of facts.

So we want to get down to single facts. That's going to be important for efficiency reasons, but it also dramatically simplifies stuff.

How many people know what RDF is? Not too many. OK. So RDF is an attempt to have a universal schema for information, and they use something called triples, which are `subject-predicate-object`. I argued at the speaker summit Sunday that that's not enough, because it doesn't let you represent facts because it doesn't have any temporal aspect. But if you take a step back from that and say it's generally a good idea — it seems atomic — we really do want atomic facts. We label them, we call them 'datoms'. We just spell it differently so we can say "datoms," because if we spelled it "D-A-T-U-M" the plural would be "data" and then we'd be into people don't know what that means (even though they say it all the time). So we have "datom" which is an atomic fact, and "datoms" are more than one fact. And it's just an entity, an attribute, a value, and then some temporal component.

It ends up that you could just put the timestamp there, but that doesn't give you a lot of power. If instead you say this thing came in as part of a transaction, I can store the transaction there. If your transactions are first class — and they are in this system — you can put the time on the transaction, but you could also put who said it on the transaction, or where it came from, or whether or not it's been audited, or any other kind of provenance or other characteristics. So that's what we do. But you can read that "T" part as time for the purposes of this talk, because I'm not going to talk a lot about that other stuff. It's a path to when. And that's the smallest fact.

[Time 0:21:00]

So now we have the problem of the database state. We say we want the database to fundamentally be a value, we want it to be immutable. It seems to be a contradiction in terms, because we know there's going to be novelty. Our business is going to run and we're going to sell more stuff or get new products or have new customers. That novelty, that newness, has to go somewhere. So how does that jive with the notion of a value?

The best analogy I could come up with was a tree ring. If you think of the database as an ever-expanding value: it never updates in place, it only expands, it only grows outward. It grows by accretion of facts. We're just going to _add_ more facts. We never go back inside, just like tree rings. You don't go back inside the tree rings, you just add more rings. We end up with something that really smells like a value and will function as a value. We're only accreting. The past doesn't change, so the core upon which we're building never changes. And that's really the key characteristic we expect of a value: that anything I've seen before will never change.

The implications are: as we have novelty, new stuff means new space. And this is sort of super key. We're going to get a lot of freedom by moving away from places right at this point.

[Time 0:22:28]

The other problem we have is how do we represent change? We said we're going to accrete facts. What is the granularity of change? We're used to saying "update this place," and here's the address of the place, or here's the primary key of the place, go do something there. If we don't want to say that anymore, if we just want to accrete facts, then what is the fundamental unit of novelty?

What we're going to say is that at the bottom we can represent this process (this is the problem we're trying to solve, the novelty problem): we can represent novelty just as assertions or retractions of facts. This new thing is true. This new thing is true. That thing that was true is not true anymore. Still a fact that it was true from then to here. This ends up being the minimal possible representation of process. With this you can express anything in these terms. And so we'll say that all the other transformations will expand into this, and I'll show you that a little bit later.

[Time 0:23:29]

The other key thing we want to do with process is we want to reify it. How many people have heard of event sourcing or anything like that?

One of the ideas behind it is that if you talk about a database that's been running for a while, and it's had a lot of activity, and you look at it and you want to know what happened, how do you figure that out? How did it become what it is? You have no resources for doing that -- unless you know how to read the logs; maybe there's a transaction log and maybe there's a way to read that, but a lot of times you're going to have to sort of replay that, because that's just a successive set of modifications to places. It's really hard to read that and understand what happened.

If instead we say we're going to reify process: when you add a new customer there's going to be something that says there is a new customer, that customer's name is Sally. That's what's going to be in a reified version of process that says process is just assertions and retractions of facts. So that's great. We want to make a thing out of that because that's something that we could store, we can look at, we can understand when we look at it. This change happened: we added this, we retracted that email, we added a new email, we sold this. We did this, we did this. Fact fact fact fact fact. These things happened. It's an information system. Fact fact fact fact fact. And that's going to let us do some other cool things later, like events.

[Time 0:24:59]

The accretion process: one of the things that is important to understand is that it really does add to what's already there. So that means that if you ever look at the view of the database at any point in time, you will be able to access the past. It's still inside, just like the inner tree rings are still there. It's not like there's a snapshot from last Tuesday and then one from Wednesday and one from Thursday and one from Friday, and each one has more stuff in it. It's as if anytime you look at the database it includes all of history inside of it. And this is important for an information model where we want to give people decision-making capabilities that say "how much have things changed in this window of time?" or "count how many of those happened over a window of time." We need all the time in one place. We don't want a bunch of independent records, here's Tuesday's facts and Wednesday's and Thursday's separately.

So we have this growing tree-ring thing.

[Time 0:25:57]

All right, so that's our plan. How do we do it? We're talking more now at the model level. We want to deconstruct this. Now we're looking just at the server component: indexing, transactions, query, IO, and disk. And I think you can divide this up into halves. This is what I said before: we want to separate process from perception.

There's a process part: novelty processing. I have a new thing, it has to go through a transaction processing thing, maybe indexing happens on it then or not — don't know, it's an open question at this point in the talk — and then output to the storage system. Completely independent of that is a perception characteristic of the use of databases. I have a question, I want to ask the question, maybe that leverages indexes — almost definitely it does — and there's going to be input (this is all relative, input _to me_) as I read back from storage.

We can separate these two things out, and we can only because we've adopted an information model and immutability.

[Time 0:26:57]

So the model we're trying to get to — and again this is not yet a physical model — is one where we can empower applications (and you can read that as application servers or analytics servers or anything you want, it's not like a desktop application). We want to empower independent applications with as much of these capabilities as we can. We want them to be able to perceive change, we want them to be able to react to it, we want them to be able to independently remember anything that's important to their decision-making process, and we want them to make decisions and then possibly affect the process that they're sharing.

There's going to be some shared resources. There's got to be some coordination around change, and there's going to have to be some shared resource around storage. We want to _minimize_ the coordination that's necessary to support this. But that's the model we want. Because now, if we can do this, if we want a more powerful system, what do we have to do? If we want more query capability, what do we need to do? Just add more of these guys. We don't really care about this growing because we're not asking it to do much, just some coordination.

[Time 0:28:12]

So if we revisit that whole tree ring thing, now we're into implementation details. How do we represent state? How do we represent this immutable expanding value? We know one thing: whatever representation we use, it has to be organized to support query. It has to be sorted in some way. That's really the fundamental leverage capability we have, is sorting things.

And it ends up there's a technique that can be used. It's used in functional programming quite often, called persistent data structures. And the word "persistent" there does not mean durable. It's a different notion of persistence. It has to do with the fact that you can represent a large immutable structure — it doesn't matter whether it's supposed to represent an array or a map or a sorted set — you can represent almost anything as a tree. And you can represent that immutably by using something called structural sharing.

So this tree could represent anything. It could represent, for instance, a sorted set. This is the view of it we have right now, and it contains all these nodes. If we want to add another child to this node — we want to make a new version of the set, we don't want to update it in place — we're going to need to allocate a new node for that leaf and then copy the path to the root. Then we have a new tree that has this new piece of information in it and substantially shares structure with the old tree. And we can do that because: why? It's all immutable.

This is the underpinnings of what are called persistent data structures. So we can do this in memory. It's done in memory by most functional programming languages. What we're going to start to do, though, is do this on disk, so we'll have a _really_ persistent, or persistent-persistent -- or, I'd rather say _durable_ persistent data structures that have this kind of shape.

[Time 0:30:12]

In the earlier diagram we had a server and it had IO and a disk. I don't want to know about IOs and disks anymore. There was a paper recently that came out that said "disk locality considered irrelevant." It's another one of these old notions that's now dying.

It used to be: boy, if you're not the machine that has the disks, you're at a tremendous disadvantage from a computational perspective, because that machine has a card that's attached to the disk, can get the data up into memory, it's lightning fast. That machine has privileged access to that disk. If you try to access that data on the disk from another machine you're going to be paying a huge overhead.

Well now, how much faster are networks? Way faster. And what's the differential between disk and memory? Huge. It ends up that anybody that needs to access the disk is losing, and the guy who has the disk in the same box is only losing very very slightly less than anybody else would lose. So it doesn't matter anymore. That kind of locality is not the way we should be architecting systems. We don't care about it. We care a lot more about putting data into memory and having good locality in the way we do _that_. But if we actually have to touch the disk we're losing anyway.

So we're just going to wrap up both the IO and the disk and say that's something we're going to call a black box called storage.

[Time 0:31:44]

What we're going to put in storage are two things. One is a log of all the novelty as it comes in , and that's really an append-y kind of job. Somebody says "I sold this," "Sally gave me an email." Fact fact fact. We're just going to shovel those facts as fast as we can into storage.

The other thing that will be in storage are much the same shaped things that we used to have locally. When we had a database that was monolithic, it had B-trees on disk. We're now going to have those persistent trees in storage, with nodes that we just don't change. But otherwise it's the same kind of idea. We've moved away from disk to storage, and we're storing index segments that we're not gonna mutate. 

But the key thing now is that we're treating storage with a simple interface. It's just a key-value interface. We say this block of the index tree maps to this segment, which is a blob of stuff, and it's going to be immutable. So all we need from storage is this sort of key-value thing.

It's a lot like the old databases used to say "this block on the disk contains these bytes." Now we're just lifting it up to a systems-level, cooperating-systems level.

So of the storage we need a key-value interface where we can store blocks of index under keys. We also do need a little bit of modifiable storage for the roots. What's the current root of the whole database tree is something we're going to need to point at new versions of the tree. And the other characteristic we need from storage is that we have to be able to obtain that using consistent read.

So we're now getting a set of definitions for a storage service. What are the requirements of a storage service? Must support key-value storage, and every now and then we're going to ask it for a consistent read. Those are the two things we need out of storage. Otherwise I don't care how it works or where it is. Don't want to know. I'm now getting architectural flexibility from doing that. And there are lots of things that can satisfy this. We can sit on top of DynamoDB, or a SQL database can satisfy those two things. You can treat a SQL database as a key-value store, stick blobs in it, and it offers consistent read.

[Time 0:34:01]

So an index is a very simple thing. It's a tree. There's some root, it's got pointers to inner nodes which have got pointers to leaf nodes. And all that's in these leaf segments are sorted datoms. So it's a big block of datoms sorted in a particular order.

You can imagine the orders. We said entity, attribute, value, transaction. So you're going to have a sort by entity. That's going to give you a great way to pull out what look like objects or documents, because it's entity-oriented. But you also have a sort that's oriented by attribute first. Same data, just sorted a different way. A second copy of it sorted a different way. One that's driven by attributes is going to feel like what kind of a database? Going to feel like a column store. A column store stores just email addresses all together and stores just phone numbers all together. And column stores are very powerful tools for analytics.

And you can store other flavors, but there are only, you know, six ways to sort them.

[Time 0:35:09]

So then there's the actual job of indexing. We get novelty in and we want to incorporate it. We're going to have these trees. Obviously, once we have a lot of data, those trees are going to be what? Huge! And we said they're immutable. So anytime you want to incorporate new information into that tree, we're going to have to do that path-copying job. Do we want to do that every time we get a new fact?

No. That's a disaster. Can't do that. I would call that "maintaining the sort live in storage," and it's not something you can do efficiently if you're going to treat storage immutably.

So BigTable — Google's big data system — is an example of a solution to this problem that other people have used as well. The idea is: everything that's new you're already logging to storage, so this is not a durability question. It just has to do with how often do you integrate novelty into that big, relatively expensive-to-create index.

The way BigTable works is it accumulates novelty in memory until it's got 64 megs of novelty, and then it blows that out onto disk. And then a separate process later takes that 64 megs and the big sorted flat file that's everything it knew before, and it does a merge sort and produces a new flat file. None of the files ever get changed. It's the same idea — they're all immutable.

The biggest difference between that and what I've been talking about is that we have trees of smaller segments and can share stuff. When they create a new flat file it shares nothing with the older flat file, and it's huge, so it's not particularly addressable and it's not easily cached in chunks. By using trees you get fine-grained addressability, and you get these nice small chunks which are good for caching. You also have the potential for structural sharing. I can integrate new stuff into the next version of the index, and it could share a whole bunch of nodes with the old index. And if they've been cached they're still good, because we know they're not going to change.

[Time 0:37:15]

So we accumulate in memory. Anytime we want a current view of the world we're going to need to merge dynamically what we have in memory with what's coming from storage. Just do a dynamic merge join between those two things. And every now and then something has to go and integrate what's in memory into storage. And as soon as that's been done, everybody can drop that from memory. We don't care about that. We're going to start it fresh, because we know now that's in the index that's in storage.

[Time 0:37:44]

So that just looks like this. Whatever is handling transactions is going to take novelty and immediately log it. That's where you get your durability. If the thing dies, it's somewhere. But that's not organized in a leverageable way. It's going to put it into an index in memory — we'll call that the live index. This is sorted! It's very inexpensive to create a sorted set in memory.

And then sometime later and occasionally some other process is going to take this and whatever is there and merge. There's going to be a lot more efficiency to that because now it's got a whole bunch of novelty and it's going to make a new tree that incorporates the new novelty. There's a lot of efficiency to sharing that job as opposed to doing it for every new transaction.

[Time 0:38:36]

So that's the process side. The perception side is really straightforward. If I want to see what's going on — it doesn't matter what this is, it could be a query, it could be an analytics thing, it could just be an ordinary "get me an entity" — if I want to see the current view of the world I have to somehow have access to the live index and access to storage.

Look at all the stuff I don't have to have access to. I don't need to be near the transaction processing system. I don't need to even have anything to do with it. Is there any coordination associated with doing this? No. 

Something has happened in a couple of slides here. What happened to read transactions? They're gone. Where did they go? They just disappeared. Because a correct implementation of perception does not require coordination. Perception in the real world does not require coordination.

So we are left with one lingering question, which is, how does this get updated? If this is actually local to me, where does that come from? I'll answer that in a second.

[Time 0:39:44]

OK, so we're going to have just a couple of names that may be new as we look at the real architecture.

One is the `transactor`. We've broken stuff down. We have storage separate. We have perceivers who might be doing queries — they're separate. And something that processes and coordinates transactions: we're going to call that the transactor.

We're going to call anyone who has all those capabilities — the perceive, remember, decide, react stuff — a `peer`. They're very powerful equals in this system. And they're your own application servers.

And then finally we have some sort of `storage service`. Ideally it would be a redundant store of one of these new-fangled storages that do distributed redundancy. They really have some great properties, as long as you don't try to treat them like a database! If you treat them like a key-value store, they are awesome. They're redundant, they're highly reliable, they're highly available, they're scalable, distributed. There's a lot of power there. We want to use that.

[Time 0:40:48]

So this is the same jobs all rearranged. For the purposes of talking about this, I'm going to say that the transactor will occasionally do the indexing job. And it does in the current implementation as well, just because it has the spare cycles and it's convenient to do so. But anytime you want to, you could move this to a separate box.

So we'll start with some novelty. Your app has some novelty; it needs to communicate with the transactor. The transactor is going to log that right away. It has its own live index. It's going to put the novelty in there. And the other thing it's going to do is transmit that novelty to any of the peers. And that's how the live indexes are kept together. It's just a rebroadcast of the novelty, and the novelty _only_. This is only the novelty that comes through.

Storage service, we don't care a whole lot about. It's very much a black box. It needs to be a key-value store that can support consistent read. This works on top of DynamoDB, it works on top of Postgres or any relational database, it works on top of Infinispan! Right, if you want a big memory grid behind that -- you don't actually ever want disks -- you put Infinispan behind it, now you have a big memory grid. Basically anything that can support that will eventually support it. Those are the ones we support right now.

[Time 0:42:23]

If we look up at a peer: they obviously have a communications component so they can talk to the transactor. They're going to have this live index in memory. And then they have to have some ability — if you just ignore the caching for the moment — some ability to talk directly to storage.

And this is the other critical thing. We said: who has locality to storage?

Nobody! Actually no one has locality to storage in this system, except whatever the storage infrastructure is. This guy is no closer to storage than these guys. Because there's no advantage to being close to storage. So this is just a service or a server somewhere else. But once it is that, it means that everybody can read directly.

[audience question, inaudible]

No, there's one transactor. I'll talk about that in a second.

[Time 0:43:21]

OK, so everybody has access to storage. It also means: where does query live? Query can live anywhere you want. What does a query engine actually need? It needs access to the organized data, access to the index. Since anybody can get access to the index, we can put query anywhere we want. There's no special machines for query. So in the case of Datomic you end up with this library you put in your Java app or JVM app that has a query engine built into it and it's got the live index built into it. I'll show you what that looks like in a second.

The other thing that's really critical about this is that we have this storage, it's remote. It's at least a network hop. We said the network hop isn't costing you nearly as much as the disk would if you actually had to touch the disk, but it is still a network hop. Can we alleviate it?

What's safe to cache from this storage?

Everything. Why? It's immutable. When does it expire? Never. These are the things you want to hear as an architect. This is sweet. We can cache this stuff relentlessly, anywhere we want. You can have a local cache. You can set up a memcached cluster and cache stuff there, so when guys don't find it in their local cache they can pull it from storage and put it in a cache that a whole bunch of peers could share. This is very powerful.

But what's different about this? Whose problem is this — looking it up in storage and then putting it in the cache if it's not there? Whose problem is it?

It's my problem. It's the system's problem. It's this library's problem. It's not your problem. You never see that. Effectively, you ask a query, the query tries to find the data, it's either got it in the cache or it goes and figures out how to get it. But it's _under_, this is caching under, or inside. It's not an application-level problem. This caching is mechanical. There's no special logic around it or anything else. The system can do this caching for you. All you have to do is start up memcached and tell it about it.

[Time 0:45:43]

[audience question, inaudible]

No no no, you don't bring anything in. So this is an app. What does it do? I don't know. Maybe it does analytics on pricing. So it's going to be interested in a certain portion of this data. That's all it's ever going to cache. This other app, this guy underneath him, he's the website. He's putting up product pages and he's reading all different kinds of stuff from here. This is not a replication of everything. It's not even proactive replication of anything in specific. Everybody is pulling in their working sets depending on the work they're doing. And they don't care, they never need to pull in anything else ever.

Now of course being trees, they're all going to have a really nice set of the top part of that tree that they'll all cache, and they'll get a lot of efficiency for that. And in fact, it ends up that that diagram where I showed you three tiers — that's it. It doesn't get any deeper than that. Which means that once you've cached the top part of that you can find any piece of information you've never seen before in one read. And potentially that's one read from memcached, although some of these storage services like DynamoDB are really fast.

So it's an on-demand-driven cache. It's filled on demand. It's filled with your working set. Different peers will have different working sets and different amounts of stuff cached.

[Time 0:47:16]

[Clarification of Q&A]

No, this part is writes. This is the write path. I have some novelty, I send it to a transaction, it writes it in here. That's the write part. This is all read.

[audience question, inaudible]

Let's just talk through this a little bit. Let's just say that everybody understands that indexing -- well, let's imagine it takes the log, it doesn't actually, but let's imagine -- it takes the log and turns it into the sorted tree. So log is just append-append-append-append, and the index here in storage is the sorted tree. Let's say it last did that an hour ago. Everything that's happened since it last did that is right here. It came in, it said boom, it got logged, but it's not in the index yet. But it got reflected back out, so it's here.

Now you want to answer a question. "I want to put up Sally's profile page, and in the last hour Sally told me her email address had changed." That fact is here. All the facts Sally's told us prior to an hour ago are in here. When I want to go ask a question saying "tell me everything you know about Sally because I need to show her profile page," the query engine is going to do a merge join. It's going to say: seek to Sally in here, seek to Sally in there, merge them together. If there's retractions in here, they'll make it seem like the older facts in here are invisible. But what you'll get is the sum of information from the two. And that looks like 'now'.

Eventually, let's say we allow this to build up for an hour and we say "now's a good time to index". We're going to make a new tree here, it's going to share a lot with the old tree, but it will now incorporate Sally's new email address. When that job is done, everyone will be informed, and this will turn empty, and we'll start accumulating the next window of change. That's how it works.

[audience question, inaudible]

The live index is actually local in memory to every peer... That's correct. Every change is broadcast to every peer.

[audience question, inaudible]

Well, the thing is, these peers are servers. You're not going to have 10,000 of them. This peer is really something like an app server. It in turn is probably serving other people the way your app servers do today. It's not necessarily the web layer. Although 10,000 peers in your web tier would be a lot. We don't currently have a multicast infrastructure for making that propagation if you wanted tens of thousands. But that would be a way to approach doing that.

Let me keep moving forward and then I'll try to answer questions later.

That stuff does come over here but it's not the same as what's there. It's not organized yet. The organization happens here.

OK, let me keep moving and then I'll take questions at the end.

[Time 0:50:47]

So process itself. It ends up that I said we can boil everything down to assertions and retractions. But you can certainly think of transformations that you couldn't express that way. Like, I want to add $10 to your bank account. That's the logical process I want to make. But it ends up that the fact that ends up from that process is dependent upon the existing value in your bank account. So how do you do that?

The way you do that is with transaction functions. A transaction function is a function of the database and some arguments, and what it yields is transaction data. We said transaction data is assertions and retractions of facts. Now we're going to say transaction data is assertions and retractions of facts, or transaction functions and arguments. Which means that we can have a transaction function that is "add" that takes the amount, and it will be passed the database. We say "add $10 to Sally's bank account." That function will be run inside the transaction. It will be given the current value of the database, because the database really is a value. So this is a real function of a real value. It can perform any queries at once, including looking up Sally's current balance. And then it will yield more transaction data.

So if it was that simple a thing, it would go: the function looks it up, finds out the balance is $100, says it's $110, and what it would expand into is the fact "Sally's new balance is $110." But it could do more involved things.

So you can imagine any transaction is assertions and retractions, and potentially calls to transaction functions. Those transaction functions can expand into calls to other transaction functions but eventually will expand into assertions and retractions. This expansion happens over and over again until it's all assertions and retractions. And this allows you to do any arbitrary transformation on the data in the database. You can ask questions, you can do anything you want.

But the cool thing is you have a decision — again, architectural independence — as to when you do this. If you know you're adding pure novelty, you don't need to do it inside the transaction. You just say "I have new facts, I know they're new." If you have something where you think you have very little contention, you can say "well, my local view of Sally's bank account is $100, I'm going to make it $110" and I'll just put that in with a condition that says "if it wasn't $100, fail." That's more optimistic. But if it was an inexpensive calculation and the chance of collision was really low, that's the more efficient way to do it. Or finally you can do it this sort of old-school way which is send the function all the way into the transaction, which is going to get run atomically inside a transaction with no interference.

[Time 0:53:36]

So we call that "expansion." This is what the transactor does. It accepts transactions. Transactions are just data: fact fact fact, you know, assert assert assert, retract retract. And then even if you want, you can have a function — it's still expressed as data, it says "call the update salary function with this argument and this target entity." It expands them. It will eventually apply them to the in-memory view of the database inside the transactor. It will log it. And finally acknowledge the transaction to you and then broadcast it to everybody else. Every now and then this indexing will occur. And as I said in my talk, the storage equivalent of garbage in garbage collection comes out of doing it this way. Because as you make a new index, now no one else is going to care about the old index. The root and a bunch of nodes of the old index are junk in storage. So we have to clean that up, but the analogy to memory management is really good.

[Time 0:54:35]

These peer servers have direct access to storage. They have their own query engine. They have their own live memory index. And they do that merge themselves.

There's also this two-tier cache going on, and this is inside, under the query path. When you ask a query, if it doesn't have the data it gets it. It will look for a local cache which is at the object level. If that is a miss, it will go and look for segments in memcached if you've configured it. Otherwise it'll go to storage. And it will cache it.

I'm not going to talk too much about this because it's sort of involved, but there's an in-memory persistent data structure that's the live index — it's really immutable in memory too, we're just moving from one to the next. There's an infrastructure for allowing you to find the data in storage. It goes through the cache and then finds the storage, and the trees, the roots of which will almost definitely be cached in memory.

[Time 0:55:32]

This value is just a pointer, a struct that points to these things, and it's inside a box. The only mutable thing in the system is the fact that the contents of this box will move from one of these immutable structures to another, whenever we've updated the memory index or whenever an indexing job is completed. There's nothing else mutable in the system. This one identity, which is "the database as of right now" — which means if you've obtained this and you started running a long-running query, everything that you've got won't change underneath you, even amongst threads in your same process. You're really working with the value.

[Time 0:56:19]

OK, so what are some of the characteristics?

Well the first thing is, somebody asked about it before: does it use Lamport clocks or vector clocks? One of the cool things about separating perception and process is you can now make independent decisions about availability and scalability for those two things, because they're completely separate. So the decision that's made by Datomic — because I think it's a market need and has a lot of value to companies — is to make a traditional decision about the process side. It's transactional. It's a consistent view of the world. It's a single-writer system.

The cool thing though is that that transactor? It's not doing anything else. It's not serving queries or anything. All it has to do is handle writes. If you want arbitrary write scalability, you're going to have to give up transactions and queries, and everybody who's adopting NoSQL databases that make that choice for you is getting that tradeoff. And I think it's not a great tradeoff for a lot of companies, and that's why I'm in this space trying to give them this hybrid solution that's the combination of two.

On the read side though, we get all the benefits of distribution. If you put DynamoDB in that storage slot you have arbitrary scaling — all of Amazon's goodness about storing stuff in different places. You have knobs on throughput for reads and writes. Tremendous service approach to storage. 

Or [if] you're already running a SQL database you can leave it there and just start adding data in this format on top of it. But it's an independent decision. If you do choose a redundant scalable storage subsystem, though, you will get the benefits of having done that. So you get scalable reads. 

And obviously queries scale, because the queries are not in one box. The queries are in every peer box. So you not only have scalability of that, but you have very elastic scalability. It's not like adding a new box to a cluster configuration, that's kind of a big job. This can scale up and down with Amazon's auto-scaling. More peers come up and down due to load, you have more query capability. So it's elastic.

[Time 0:58:38]

I'm going to hurry through the last couple here. You definitely have more flexibility. The schema model is extremely small. All you define are the attribute definitions: the name and type of an attribute, what its cardinality is, and things like that. There's no higher-level thing. The fundamental unit is a fact. The only configurable aspect of the fact is the attribute. So that's the only schema that exists. And there is schema.

And the net result is that it feels like you're programming with multimaps. If you take an entity approach to this, it feels like every entity is a key-value set where each key — each value could be multivalued if you wanted. So it's very convenient, it's very flexible, and a easy-to-use programming model.

[Time 0:59:25]

The other huge benefit you get from this is you get time. The database is really a value. It does include all the past. You can take any database that you've got and say "pretend it's as of last month." You can just say the database: pretend it's last month. That doesn't do anything, it doesn't change stuff, it doesn't throw stuff away. It just says pretend it's last month. Now you can ask queries of that database. When you ask the queries, if it finds anything newer than last month it just doesn't use it. And it can answer those questions. Which means you can do as-of questions against the current database for a past point in time. They don't cost anything to do that.

The other critical thing: how many people have ever built systems with timestamps and had to do as-of queries? It's brutal. Because what do you have to do? You have to flow around that time. If I want to say "as of last week," last week has to be part of every query, has to be part of every join. And it's nasty: you have to do max and all that. Very very tough stuff.

What if instead I walk up to a database and I say "as of last week"? So I can have a query which is "get me the sales subtotals." That query works against 'now'. I can tell the database "give me yourself as of last week." I can take that same query, run it against that database value. It's not parameterized by time anymore. The database knows "I'm supposed to be pretending it's last week". Now the same query you can ask — doesn't have any time as part of the join — of course you can also ask queries where you want to compare times and things like that. So you can do as-of a point in time, you can do windows since a point in time.

[Time 1:01:02]

The other thing you can do, because the database is actually a local value — you have that memory component but it is really local in your process — you can do as-if. What if? What if I added this stuff, what if I committed this transaction, what would the database look like? What would the answer to this query look like? Do I need to go to the server to do that?

No! I can just take the in-memory data structure, which is persistent. I can make a new version of it that includes the transaction data I'm thinking about committing. That's now another database value. It's all in memory, but it can include stuff from storage. The novelty is still in memory. Now I can take the same query and ask it. I don't need anybody's help. I'm not bothering anybody. I can do "what if." And then I can say "well that query still works, so good, now I'll try to commit it." Or maybe I'm just doing what-if analysis and I never commit it. We're just trying to make decisions. "What would happen if we made this decision? What would the world look like?" I don't have to put that into the database to answer the question. I still have query capability.

[Time 1:02:01]

Perception and reaction: obviously perception is straightforward. We have this immutable thing. All the queries have been flavors of perception. But reaction is now easy, because we have this live feed. The transactor is sending all novelty to all the peers. Which means it's easy to make an event on the peer that says "some novelty came in." But the other thing that's beautiful about it is you can say "here's some novelty, Sally changed her email address." And if it was important to you to say "was it the same as what it was" — can you do that? Sure. Because the database as a value means that you can capture the database value when that change was made. And in fact it gets sent to you with the event as well. Here's the database before the change, here's the data of the change, here's the database after the change. Ask any queries you want. And if you want to have sophisticated eventing, you just have to query that data. You just say "I got this event feed and I'm only interested in these certain kinds of changes." You just query the feed and now you get that.

Because the query engine is in memory, and it operates not only against the database but on in-memory data structures too — combinations of your own memory and what's coming from the database. So you can filter that way.

[Time 1:03:16]

So I think you get a lot out of this. In particular what you get is simplicity in the database. The state model is what I would call epochal. It only moves from valid consistent point to valid consistent point. There's never any in-between. There's no read coordination or anything else. There's only coordination for the process, and that's done in the transactor. Every time you ask the same query you get the same results. You have a stable basis for decision making.

If you think there was a problem with the system in the middle of the day and you're not going to get to look at it till next week — how many people have ever had that happen? The queries are returning some weird results at like 5 o'clock, everybody wants to go home, we'll look into this tomorrow. And then you come back tomorrow and the query is fine because just more data was added into the system. How are you ever going to find out what was wrong? You're toast.

With this it's straightforward. We can say "ask that query as if it was 5 o'clock," because I know at 5 o'clock that query was screwy and giving me weird results, and you could figure out the answer to your problem. Because you can get a basis anytime you want. And the transaction is really well-defined: what a transaction is is a function of the database value.

[Time 1:04:24]

The other things that are important: you can communicate a basis. If I make a decision or I want to give you work to do, I have a way to communicate it. I'm not going to say "go look in the database." I'm going to say "there's some work you have to do at database time T," and you can go and look at exactly what I was seeing. So you know what to do.

We've seen already that architecturally, because we've broken stuff apart, we have freedom to relocate things. We don't care where the queries are run. We don't care where the storage is. We don't care where the transactor is running. You have a whole bunch of flexibility about where you put things. You can put data up on DynamoDB and run the whole system in your LAN and put memcached in your LAN and isolate yourself from the fact that the internet is actually potentially in between.

We saw the time-travel stuff, and we saw the event processing.

[Time 1:05:12]

So the net result of deconstructing the database is that you're able to treat the database as a value, in precisely the way I was talking about in my talk. You have a real information model. You have a system that's substantially less complex. It's easier to understand, it's easier to change. You want to change your storage, you can do that. It's easy to replace components, it's easy to relocate things. It's more powerful. It's more scalable. You want more brains? Start more peers. There's less coordination, which also leads to the scalability aspects. And the information model not only lets you remember everything, which is important for your businesses because they want to make decisions, but also gives you more flexibility, because that's a model that's easy to turn into any shape that you want.

And with that I'll wrap and answer any questions. Thanks.

[Applause]
