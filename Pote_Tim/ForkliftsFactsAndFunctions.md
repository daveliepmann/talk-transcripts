# Forklifts, Facts, and Functions: Building a Warehouse Management System with Clojure+Datomic

* **Speaker: Tim Pote**
* **Conference: [Clojure/Conj 2025](https://www.2025.clojure-conj.org/)**
* **Date: 13 November 2025**
* **Video: [https://www.youtube.com/watch?v=NV02r1Y1B-8](https://www.youtube.com/watch?v=NV02r1Y1B-8)**

![00.00.15 title page](ForkliftsFactsAndFunctions/slide-00.jpg)

All right, so my name is Tim Pote, and in this story, I was the director of technology for a company called Simple Modern. Simple Modern is a consumer product company. They make primarily vacuum-insulated steel tumblers and water bottles. In the summer of 2023, I was asked by the executives of Simple Modern to get them onto a WMS, a warehouse management system. They had this warehouse in Oklahoma City where they're based out of and they were running it up to this point on spreadsheets and sweat and tears.

It's not easy to run a sizable warehouse on spreadsheets. So they had decided it was the right move to get started, but they had decided they were ready to get onto something a little more systematic. So we sat down, we talked, and we all decided that this is probably going to be a case where you buy something off the shelf. A WMS is like a commodity item. You can find it at every price point. There's probably dozens or hundreds of these available. So that was the rough agreement initially. So, we had the conversation. I went back to my desk and I wrote down all of the WMSs I could think of off the top of my head and went to each one of their websites and looked at their feature grid and found the one that had the lowest price for the most feature because that -- as we all know -- is how we solve problems. Yes.

I wasn't expecting a yes. Yes. No! That is not how we solve problems. I went back to my desk and what's the first thing that you do? What is the first thing you do when you need to solve a problem?

Write it down! You write down the problem. 

![00.02.05 How do we track inventory?](ForkliftsFactsAndFunctions/slide-01.jpg)

So I sat down and I said, "How is it that we track inventory in our warehouse?" Notice this is different from "how do we get on a WMS?" WMS is a solution. This is a question. How do we do this? And there's a few criteria that we need to know about in order to decide. So we have criteria for the solutions that we might want to apply.

![00.02.30 track movement](ForkliftsFactsAndFunctions/slide-02.jpg)

The first criteria is whatever it is has to track movement in the warehouse. This is an incredibly important function of a WMS. Who here knows offhand [which] movie this shot came from?

![00.02.40 Indiana Jones warehouse](ForkliftsFactsAndFunctions/slide-03.jpg)

Indiana Jones. Yes. Indiana Jones and the Raiders of the Lost Ark, right? And in that crate, it's the lost ark, the ark of the covenant. And they put it into an unmarked crate and roll it into an unmarked location. And the camera pans back. And what's the implication here? This ark is once again lost to the sands of time. It's never coming back. And that is exactly what happens when you don't write down where stuff is in a warehouse. You can't find it, you can't ship it. You can't find it, it might as well be lost, stolen. It doesn't matter, right? It's gone. So knowing where stuff is is an incredibly important aspect of a WMS.

The other thing that, okay, when people think of a warehouse, usually you're thinking of what? A place where you put stuff, right? There's a bunch of stuff in a warehouse. That's true, but it's also very much not true. A warehouse is part of a logistics chain. It's a flow-based thing. You're pushing stuff through a bunch of steps. A warehouse is about flow. At the end of the day, you have to have some way to bring inventory into the warehouse. You need a process to track inbounds. You also need a process to track your outbounds, shipments, things that are going to retailers or to customers. So with these three criteria you can actually build a functioning WMS but in practice you need a fourth. The fourth one is that it must integrate with whatever accounting system you have. 

![00.04.20 Track track track integrate](ForkliftsFactsAndFunctions/slide-04.jpg)

So if you don't know what an ERP is-—enterprise resource planning-—this is the central hub of any company that runs physical goods. This is the thing that knows where in the logistics chain all of their inventory is, what sales orders need to get fulfilled, what is going to arrive at the warehouse and when. It knows everything about the entire company. And so the WMS has to get its orders from the central source of truth, from the accounting system, right? So the inbound orders come from the accounting system and the outbound orders come from the accounting system. And when the WMS is done with those things, it goes back to the accounting system and says, "Hey, I received this" or "I shipped this." So the WMS is a small part of the total knowledge graph of a company.

With these four things, you actually can build a fully functioning WMS. And what's more is these are not complicated requirements, right? We could probably close our eyes, throw a dart, pick any WMS on the market, and they would at least do this much. So this ought to be an easy job, except there are a couple of...small details. They're not going to be important. Don't think about them too much, but I'll mention them here for completeness. So, first of all, when you're thinking of a warehouse, you're probably thinking of something along the lines of this, right? 

![00.05.30 warehouse](ForkliftsFactsAndFunctions/slide-05.jpg)

You have rows and rows of bins or pallets and some walking space in between. Imagine I had time to put doors onto the warehouse, and that is pretty much what our warehouse looks like except with a small change. 

![00.05.30 yellow line](ForkliftsFactsAndFunctions/slide-06.jpg)

So this yellow-orange line in the middle of the building is the dividing line between two completely separate corporate entities. They're on their own books. They have their own accounting systems. On the right is a manufacturing facility. On the left is the warehousing facility. And they have the same parent company, but they are different corporate entities. So we have to keep them separate. And so this is actually not a big deal as long as nothing ever crosses that yellow line, right? But we would have trucks come that had both inventory for the warehouse and raw materials for the manufacturing facility. And so things would go across that yellow line. So this is something we're going to have to handle somehow, right? All right. 

![00.06.55 footnote](ForkliftsFactsAndFunctions/slide-07.jpg)

That's not going to be a big deal. It's just one caveat, except—-

![00.07.00 yellow line 2](ForkliftsFactsAndFunctions/slide-08.jpg)

Okay. But when I told you that this is how the warehouse looked, I might have oversimplified.

![00.07.20 another addendum](ForkliftsFactsAndFunctions/slide-09.jpg)

It actually looks a little more like this, where even on the Simple Modern side, the warehouse side, there are not one, not two, but three separate virtual locations. Locations that NetSuite, the accounting system, thinks are separate, but they're actually in the same physical space. And you might think that this is a complication, but this is actually incredibly important. It's important that you be able to sell groups of inventory separately. So, not only do we need to track every time something crosses the yellow line and do a cross-company transfer, but we need to track anytime something goes across a white line and do a cross-location transfer.

![00.07.50 another caveat](ForkliftsFactsAndFunctions/slide-09b.png)

All right, with those two caveats-—oh, well, there's one more caveat. This is a warehouse that's been running for two years and they have a bunch of processes that they've put in place and you cannot reasonably expect them to change all those processes overnight. You can't wake up day zero and we're going to do something completely different, right? So, you have to allow for some amount of on-ramp, some smoothing time for them to get used to these new processes.

![00.08.00 third caveat](ForkliftsFactsAndFunctions/slide-10.jpg)

All right. With all the caveats, this actually got a little trickier, right? Well, here I'll go ahead and give you a decision matrix. 

![00.08.30 WMS decision matrix](ForkliftsFactsAndFunctions/slide-11.jpg)

I didn't actually use this in real time. This [decision matrix] is for this talk, but this is what I was thinking more or less, right? The top four things are commodity. You can just get them. It doesn't really matter. There's no differentiation there. The next three things, the footnotes, you start to see other WMSs may or may not be able to handle this and then once you start to talk about time—what amount of time it's going to take to get onto a WMS that we built versus one that we bought—that's when you start to see some strong differentiation. So I'm estimating, given what I'm seeing, like six weeks; the top three things are literally adding and subtracting integers, you know, like how hard could it be—-famous last words. And then the next few things are interacting with NetSuite, which I've been doing for two years solid. I know how to interact with NetSuite. So I feel like this is doable. I feel like six to eight weeks might be optimistic, but I think it's doable. Conversely, if we were to buy something, four to six weeks is a normal sales cycle for any enterprise sale. It's not a short thing. And then whatever happens, they are going to want to do some customization because those next three line items—-the footnotes—-are not going to be accommodated out of the box. They're going to want to put in more stuff and that's going to be two or three months of them figuring that out. And then we finally get to the install time. So we're talking literally cutting the time to install in half if we were to build it ourselves. And it runs for cheaper at the end of the day.

So, this is what I did. I went to the execs and I said, "All right, give me six weeks. In six weeks, I will either have you, not a prototype, I will have you a working V1 or we'll stop. We'll re-evaluate. We'll figure out, hey, do we want to keep going with this or we want to buy WMS at this time?" Right? So they said yes, let's do it. And I was off to the races. 

So it's at this point that I want to talk a little bit about the technology half of the project. I actually wanted to spend a lot of time on why I chose Datomic. 

![00.10.50 datomic decision matrix](ForkliftsFactsAndFunctions/datomic-decision-matrix.png)

I had the whole decision matrix ready to walk through and I absolutely do not have time for this. So hopefully this will be on YouTube. If you're actually curious about the decision matrix, you can see some of the trade-offs here. And the coloring by the way is all for _my_ problem. So if you use this in any way, form or fashion, the first thing you should do is recolor it to match your problem. But I will call out three things in particular that are salient for this discussion. First of all we'll get the bad out of the way.

What we used in the end was Datomic Cloud. Datomic Cloud has the significant downside that it doesn't have any kind of snapshotting mechanism for the DB. Your data is absolutely safe. You don't need to be worried about it. It's in DynamoDB. It's in S3. You're never going to lose stuff. But if you ever have an oopsie, if you ever accidentally delete a database or something goes really awry and you need to roll back, that's not a thing that's built into Datomic Cloud. I have talked a number of times on Slack about this, and the Datomic team assures me that they have big plans for this, much bigger than just a snapshot. I'm just waiting on it to land. And then that orange cell goes away permanently.

![00.12.30 datomic decision matrix pointing to 'flexible' and 'history'](ForkliftsFactsAndFunctions/slide-12.jpg)

Okay, the bad stuff is out of the way. I want to subtly draw your attention to a couple of lines on the decision matrix. First of all the flexibility of Datomic is unmatched and we'll see that in just a minute and the history aspect of Datomic—you'll see in a minute how this plays out—are big factors in choosing Datomic Cloud for me for this particular project. The reason snapshot storage is not a massive deal for us is because the absolute worst case is that we do have to do a full floor count which is a thing that you do regularly anyways. So it's bad but not catastrophic in our case. So we can tolerate that amount of risk.

Okay. I also wanted to talk a little bit about how I structured the solution for this. So Datomic does a thing: they have transactor functions and they push you to structure certain solutions like a state machine, right? It's a function that takes the current state of something, some arguments, and it returns the new state. 

![00.13.20 code 1: transfer](ForkliftsFactsAndFunctions/slide-13.jpg)

So in our example here the `transfer` function--this is how we're going to track where stuff is. We have a database. Its current state is this. 

![00.13.30 warehouse DB](ForkliftsFactsAndFunctions/slide-14.jpg)

There's only one SKU in the whole warehouse and it's evenly distributed in all bins. It's a massively simplified warehouse. And we run the transfer function. So we say we're going to move 50 units from the red bin to the green bin. 

![00.13.50 warehouse DB](ForkliftsFactsAndFunctions/slide-15.jpg)

When we make the call to the transaction function, it returns a new state that looks like this... 

![00.14.00 warehouse DB](ForkliftsFactsAndFunctions/slide-16.jpg)

...with the subtraction applied to the red and the addition of 50 units added to the green. It's very simple, right? Not hard. Also in addition to this state, it also appends to a transaction log that this happened.

A couple of pro tips. If you're going to use Datomic, number one, _do not_ leak the internal entity IDs that Datomic provides to the user. You will have a bad time if you do this. If you ever need to decant, if you ever need to shard that Datomic, you will be upset at yourself. Trust me. I didn't have to do that for this project, but there are projects that I've had to do that for, right? So use the UUID v7. They're readily available. Expose that to your users instead.

Number two, if you're going to have a user-facing transaction log, which in this case we needed, they needed to see everything that happened serially in the warehouse. Do not piggyback the Datomic transaction log. That's for the system. You can put stuff in that transaction log. That's good. But for the user-facing one, make your own. So this function would append to that transaction log and it would return the state that looks like this. It turns out that if you want to write a fully fledged WMS, it doesn't actually take a whole lot of these. It is a state machine. It's not a finite state machine, but it's a state machine with a finite number of transitions. A very small number of transitions, actually.

![00.15.30 code 2: count](ForkliftsFactsAndFunctions/slide-18.jpg)

The next one is `count`. Count is how we get stuff in the door. So a truck comes, we don't just believe whatever is on the manifest, we count it, right? So the way it looks is this truck arrives, we do a count operation. This is the count operation. 

![00.15.45 warehouse DB](ForkliftsFactsAndFunctions/slide-20.jpg)

We've added 20 units to the inbound lane processing area which returns a state that looks like this. 

![00.15.50 warehouse DB](ForkliftsFactsAndFunctions/slide-21.jpg)

Now there are 20 units sitting down there. The last step of this inbound motion is to move the inventory to its final resting spot. 

![00.16.00 warehouse DB](ForkliftsFactsAndFunctions/slide-22.jpg)

And it's not until we've reached this state that we go back to NetSuite and say we have received this. This is good, right? We can start shipping. It's not until we reach this state.

![00.16.05 warehouse DB](ForkliftsFactsAndFunctions/slide-23.jpg)

Okay, so that's the count operation. That's how we get inventory in the door. And the last step is of course getting stuff out the door. So the `ship` operation works like this. 

![00.16.30 code 3: ship](ForkliftsFactsAndFunctions/slide-25.jpg)

We start with our normal database state. We transfer stuff into a staging area for shipping, which leaves us in a state like this. 

![00.16.40 warehouse DB](ForkliftsFactsAndFunctions/slide-27.jpg)

![00.16.45 warehouse DB](ForkliftsFactsAndFunctions/slide-28.jpg)

And then we run the ship transaction which deducts that inventory from the warehouse and 
leaves us in this state where that inventory is just gone now. 

![00.16.45 warehouse DB](ForkliftsFactsAndFunctions/slide-29.jpg)

All right. It turns out with these three transactor functions you can run an entire warehouse. We added a fourth to destroy inventory for convenience, more or less. (You can actually do that with count if you wanted to.) I know of a couple others that you might want, but at the very core, this is enough. And it's incredible that we have a database and a data model that makes it so simple.

So, back to the story: I was able to deliver all of these features in six weeks. And even that is a little bit of a fib. I had a vacation already planned in the middle of all this and I wasn't going to miss it, right? So I wasn't going to work on it either. So I took a week off in the middle of it. This was all done in five weeks. In five weeks, we had a working V1 that we started to deploy to the floor. So I think it's an incredible testament to the power of Clojure and Datomic.

With the remainder of my time I want to run through a couple of scenarios that really highlight the power of Datomic. 

First of all it of course has to do with the yellow line, right? It's the footnote features that end up kind of dominating your design process and dominating the debugging process. So this is going to be a debugging story. 

![00.18.20 warehouse DB](ForkliftsFactsAndFunctions/slide-35.jpg)

What happened was we received a shipment, they counted it up, and the first thing the people on the floor did was transfer it across that yellow line, which left us in this state...

![00.18.45 warehouse DB](ForkliftsFactsAndFunctions/slide-36.jpg)

...which is fine in principle, except the first thing that the WMS does when it sees something go across that yellow line is go back to NetSuite and say something moved across the line—-deduct it from one side, add it to the other. And when we tell NetSuite that we've received stuff, there's still 20 units sitting down in the inbound lane. NetSuite doesn't know we have stuff and we just told it we removed it. We went negative on inventory. So, we have an ordering problem where the people on the floor want to just do their job. They want to move their stuff around and we need to order it in a particular way for the accounting system. We need to receive it and then transfer it, right?

![00.19.20 warehouse DB](ForkliftsFactsAndFunctions/slide-37.jpg)

You might think, well, just look for anything that looks like this. If it looks like that, don't process that transaction yet. And that would work as long as nobody does something like this where I got halfway there and took a lunch break and then came back, right? 

![00.19.30 warehouse DB](ForkliftsFactsAndFunctions/slide-38.jpg)

That's a thing that happens. So, we needed something more robust. 

What we ended up with was a system of breadcrumbing. 

![00.19.45 warehouse DB](ForkliftsFactsAndFunctions/slide-39.jpg)

We start with the breadcrumb on the inbound lane. And the rule is if there's a breadcrumb on the 'from' location, you copy it forward to the 'to' location and the breadcrumb is specific to the order that was inbound. So when you move something, you copy it forward. 

![00.20.00 warehouse DB](ForkliftsFactsAndFunctions/slide-40.jpg)

Move something, you copy it forward. 

![00.20.02 warehouse DB](ForkliftsFactsAndFunctions/slide-41.jpg)

Move it again, copy it forward. 

![00.20.03 warehouse DB](ForkliftsFactsAndFunctions/slide-42.jpg)

This also applies to sales orders. So if you pick something, you copy that forward. 

![00.20.07 warehouse DB](ForkliftsFactsAndFunctions/slide-44.jpg)

You don't ship the sales order if it has a breadcrumb on it. You hold.

![00.20.15 warehouse DB](ForkliftsFactsAndFunctions/slide-45.jpg)

Finally, we'll move those last 20 units out, leaving it in a state that looks like this. 

![00.20.20 warehouse DB](ForkliftsFactsAndFunctions/slide-46.jpg)

And it's at this point that we go to NetSuite. We say we fully received it. We clear all the breadcrumbs and all the other transactions come through after that.

![00.20.28 warehouse DB](ForkliftsFactsAndFunctions/slide-47.jpg)

It's important to note two things. Number one: we did not know this was a feature that we needed. I rolled out V1 that didn't include this. Had no idea this was going to be a requirement. And number two: the entirety of the commit—-the relevant parts of the commit-—is right here. 

![00.21.00 commit](ForkliftsFactsAndFunctions/slide-48.jpg)

There's like 400 lines of tests that I didn't include. A lot of this is actually whitespace changes. There are PR changes. This is a sum total of a 20-line commit. Twenty logical lines to get everything that I just said and it wasn't planned whatsoever. This is because of the flexibility of Datomic: the fact that you can tack an attribute onto anything you want and copy it forward very easily. It's very straightforward, very simple to implement.

Okay, the second story requires a little explanation. This one has to do with the white lines, right? It's always the little footnote things people forget about. I'm going to color all the bins in this one location orange because, well, you'll see why in a second. 

![00.21.50 another footnote](ForkliftsFactsAndFunctions/slide-50.jpg)

So what would happen is the people on the floor would get a phone call from the logistics team to say, "Hey, I want you to move these 100 units over into the orange area," only because we want to sell it out of NetSuite in a particular way--not for any reason other than the accounting system. 

![00.22.00 another footnote](ForkliftsFactsAndFunctions/slide-51.jpg)

We have another impedance mismatch between what the people on the floor want to do and what the accounting system wants to do. This is a waste of their time if you're on the floor, right? I have lots of things to ship. I have inbounds that are late that I need to move to the right spot and you're asking me just to shuffle stuff around the floor? This is stupid. So I implemented a feature that allowed them to just change the color, so to speak, the location of the bin in-place, right? 

![00.22.30 another footnote](ForkliftsFactsAndFunctions/slide-52.jpg)

So, they immediately went around and just changed a bunch of locations of bins, which is great.

![00.22.40 another footnote](ForkliftsFactsAndFunctions/slide-53.jpg)

But then, two weeks after I implemented this, it's a Friday afternoon, 3:00. I get a call from the logistics team. They're saying something is wrong in NetSuite. This feature is broken. So I go take a look at the system and it looks like this. 

![00.23.08 another footnote](ForkliftsFactsAndFunctions/slide-54.jpg)

And I think that's strange. I don't think they went back and undid all the work they were planning on doing. So I queried the entire database for every time any bin ever changed location— all of history. This is the thing SQL cannot do...oh, unless you plan ahead. You'd have to really plan ahead and know that you're going to want to run this query one day or you can't do it. So I ran a query to say every bin that had ever changed location and, oh, I could see where they changed them and then I see where a whole bunch of bins changed in the same transaction all at once. And I was like, well, that's weird. The next thing I did was grab that transaction, pull it out of the transaction log, and look at it. This is another thing you _cannot_ do in a SQL system: see what was between the begin and the commit. I want to see everything you did and what you saw in between. You can't do that in SQL. It's impossible. So I grabbed that commit and I saw what happened. What happened was something changed all the bin locations and that was it. It didn't do any of the other work you have to do in order to change a bin location. It's like someone went in and just reset everything. Who could do that? It was definitely me. I was definitely at fault because there's no user path to do this. It's impossible. 

Then I remembered I have a bunch of seed data for the warehouse in files checked into git and it included all the bins and their locations and I would just run that all the time to reset seed information from the original database state. When I did that it clobbered the current state of the database.

All right. Three o'clock Friday afternoon. What I was able to do then was, for each of the affected bins—-there's about 20 or so-—for each affected bin I was able to see what transfers happened in and out of those bins for the entire two weeks we'd been running in this state. I can see all the transactions and I can see them with precision. It's not like an updated-at, "I hope I'm about right." It is definitely everything that happened between here and there. I'm able to fix all of the transactions in my transaction log to be correct. I'm able to set the bins back to their proper state and then I'm able to delete all of those transactions from NetSuite and let them just replay back into NetSuite. 

![00.26.00 another footnote](ForkliftsFactsAndFunctions/slide-55.jpg)

It is a very straightforward operation once I realized what the problem was. Again, I started at three on Friday, went until five, stopped, I stopped, had supper with my family, put the kids to bed, picked it back up at eight, and by ten o'clock I was completely done. Four hours for something that would have been...I would have been so hosed if it had been SQL. If it had been a SQL store, I would have been walking around, hat in hand, "Do you remember what you did? What was the state back then?" Right? Nobody knows. But I had it 100% fixed. There were no holes in this. It just worked once I had done it.

![00.26.50 use datomic](ForkliftsFactsAndFunctions/slide-56.jpg)

So my hottest take of this entire talk is: you should use Datomic. It's a fantastic system. Especially for the people in this room, you're using Clojure already. Datomic should be the first stop. It should be like, "I'm using Datomic _until_ I realize why I'm not using Datomic." Maybe I have throughput needs or other needs that push me beyond what Datomic can do. But you start here. Because the leverage you get, especially around flexibility and history, is just--it's incredible. I can't speak more highly of it.

![00.27.30 thank you](ForkliftsFactsAndFunctions/slide-57.jpg)

I also, you know, the fact that I was able to do this at all--the initial thing, five weeks, one dude, to get all that done, and then fix these things too, right? I owe a huge debt of gratitude to Rich, to the core team, to the Datomic team, and to the Clojure community at large. It's because we have a community that values simplicity and stability that I was able to do this. And it's still running! No one's taking care of it, more or less. It just continues to run to this day. 

We used a bunch of tools like tools.logging, Integrant, Hiccup, Ring, data.xml, data.json, cgrand/xforms. I don't know if C. Grand is in here but I use that stuff all the time.

Huge, huge thank you to the Clojure community and to Rich and the Clojure team.
