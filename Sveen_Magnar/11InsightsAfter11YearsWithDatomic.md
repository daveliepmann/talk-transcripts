# 11 Insights After 11 Years with the Functional Database Datomic

* **Speaker: Magnar Sveen**
* **Conference: [NDC Oslo 2024](https://ndcoslo.com)**
* **Video: [https://www.youtube.com/watch?v=YSgTQzHYeLU](https://www.youtube.com/watch?v=YSgTQzHYeLU)**

Hello everyone, thanks for coming to this talk. My name is Magnar. After being a consultant for nearly 20 years, I'm now employed at the Norwegian Food Safety Authority. I have made some video series throughout the years — one about how this weird old editor is sort of cool, and others where I do some pair programming, test-driven development, stuff like that. And I've also spent 25 years over my years writing a Norwegian text adventure game, which is a weird thing to do, but for the last eight years that has been running on Datomic in production.

So — Datomic. Who has heard about Datomic Prime prior to this conference? Some of you. Okay, so who is hearing about Datomic for the first time now? Wow, that's cool — you're in for a treat. Datomic is the thing that you would draw like this in an architecture diagram. So, it's a replacement for the trusty old SQL databases that we all grew up with.

And often times when a new database enters the scene, they will place themselves in a sort of supplementary position — like a time series database, or a key-value store for your session data, and so on. But Datomic places itself squarely as _the_ database, as the place where you keep your crucial business information.

So Datomic was written in Clojure, which is a data-centric functional language on the JVM, by this guy, Rich Hickey. He wrote the Datomic database, and he wrote Clojure the language. So while all languages can use Datomic, there is little doubt that Datomic and Clojure are best buds. They go together like — well, like they had sprung from the same mind or something.

So, a new database — what kind of databases are there? A lot, but most prominent is the classic square-table relational database. Then there is the NoSQL document database, maybe the simple key-value store. And when I started using Datomic 11 years ago, I had no idea what kind of database it was. I was only interested in it because Rich Hickey, the creator of Clojure, had created it. I was a fan of Clojure and Rich Hickey and all these cool talks. So let's check this out — and to my surprise, Datomic was neither of these.

In fact, Datomic has quite a different take on the shape of the data. It's most similar to semantic triples from RDF, or the Resource Description Framework, which sounds very fancy but it really is not. It's almost like grammar: subject, predicate, object — "Sarah reads a book." In Datomic we call these entity, attribute, and value, and Datomic stores its data in this format.

So here's the first lesson I learned: it is that Datomic is fundamentally different from the databases we grew up with.

So let's take a look at entity, attribute, value. Sarah reads the book 1984 — this is a piece of information. Here are some more: John reads The Beach, 1984 was written by Orwell, and The Beach was written by Garland. And Datomic is a database of facts much like these.

Let's break down the first one. There's some entity number 17 who has the attribute of person with a given name and the value is Sarah. Then there's another entity 23 which has a book title of 1984. And then we have entity 17 has the attribute `:person/reads` 23, the book. [This] sort of paints a picture about Sarah reading the book 1984.

Let's add a few more. There's a new entity 40 which is George Orwell, and then the book 23 has a book author that is 40 — George. So these are the entity IDs. They are not 17 and 23 and 40 in the database — they're quite a bit longer than that — but these slides would look ridiculous if I had that, so this is easier. And then these are the attributes and finally the values: entity, attribute, value.

And when you look at this you might notice a few things. First of all you might notice that these are references to other entities, and the significance of that we will talk about soon. But first the attributes — this part here of the attribute looks quite a bit like a table, like a person table and a book table, right, and in that case this would be the columns. So let's take a look at this data sort of in those square tables.

Here's the person table. It has the ID column, given name, family name, and reads, and Sarah is entity 17 and she reads 23 which is a foreign key to the book table. Right, and it has a title — 1984 — and the author is 40, which is a foreign key back to the person table, which is George of course.

As you may notice there are some blank squares here. In Datomic we can model sparse data, but in tables we sort of have to use space on the lack of information, so there's some nulls we have to put in there.

So that was tables. But instead of looking at this data in tables, what if you look at them like nodes in a graph? So we have the 17 node, 23 and 40, and then these attributes that are pointing to other entities — they sort of become the edges or the vertices in the graph.

And to illustrate just how natural it feels to use Datomic as a graph database, I'm going to show you a little piece of Clojure code. And if you haven't written any Clojure code before, it's fine, I'm going to guide you through it. So we define person to be the entity in the database with the ID of 17, and when we look up `:person/given-name` we will get back "Sarah" — that's probably not a big surprise.

But what happens if we look up `:person/reads`? Do we now get the number 23? No, we in fact get the entity. We get the book. It has the database ID of 23, but it also for instance has the book title. So now we can look up the book title that the person reads. We're following the arrow to the other entity, and we can keep going — we can look at the book author, follow the arrow there, and look up the person's family name, and it's George Orwell — at least it's Orwell. So in code we're now following the vertices in the graph.

Now let's say that we instead have defined the author to be the entity in the database with an ID of 40, and you might notice that dear old George does not have any arrows pointing out — it doesn't have any attributes that point to other entities.

But it would be useful to know which books he wrote, right? And you can actually do that in Datomic — you see we used a book author there, but you see that sneaky underscore — there's a sneaky underscore in front of the author there? That tells Datomic that we want to follow this reference the other way. We're following it _back_, so we're not following back to a single book — we're following it back to all books who has George Orwell as its author. So we get a list of entities.

And we can do the title of the first book and we're back to "1984". So this is pretty nice — sort of navigating in the graph feature of Datomic.

But maybe maybe the coolest thing about these entities and sort of the whole RDF thing is that the entities and their attributes are not constrained to living just inside the square box. Because Sarah, in addition to being a person, is a user, and as a user she has an email address. And I'm pretty sure George Orwell does not have an email address. On the other hand, George is an author and he has a biography and he was born in India, and so on. And maybe the book title is part of some product with the SKU. So we are able to model the world as we see it.

If you have ever encountered the object/relational impedance mismatch, you will know that it can be quite painful, because you've modeled your domain so perfectly and it won't fit into these square boxes in the relational database. So we don't have that problem here. So Datomic can model square data, but also sparse data and graphs and mixed data — all the complexities of the world — and that is very freeing.

I said that Datomic is a database of facts, which begs the question: what are facts? Think about it, I'll drink a little bit. We will start out very very simple. Is "Sarah" a fact? No. "Sarah" is not a fact. What about "Sarah reads"? Well, it is a complete sentence, but I will argue that it is not a fact yet. How about now — "Sarah reads 1984"? Is this a fact? I am going to say no, this is not a fact, because facts never change. Now that's a strange thing to say. I think we're going to have to dig a little bit deeper into that one.

What happens if Sarah starts reading "The Beach"? Does the fact that she did read 1984 change? It does not. What's missing here is time — _when_ did Sarah read 1984? And now Sarah is free to start reading The Beach, and maybe 10 years ago Sarah read The Hobbit. So adding time to the equation removes the need to update our data, because the past never changes.

Now this is an important point — you might wish you could change the past in some way, but well, so in Datomic we only ever add new facts. So Datomic is a database of facts that never change. And at this point I can imagine that you might have a few questions regarding that statement and I will try to answer them now.

This is our entity/attribute/value and to add this into the database we do this. `:db/add` is a transaction function that adds a fact. We will call the `transact` function and we will pass it the connection to the database and this will transact it and we will end up with a tuple in the database that looks like this. There's a 17 entity with the `:person/family-name` of Connor and the time — you can see that the transactor added the time at the end there.

And now for your question: yes we can remove a piece of information as well. We can _retract_ it. We can say this piece of information is no longer valid, this is no longer the case. But look at what happens to the database: we have added a retraction. The past still did not change.

Now you have another question. Oh yeah, that's the first one: even retractions are additions. I think I think this is very very very cool. But your other question is this: what if we are legally obligated to delete information? And yeah, we can do that in Datomic. We can do so with _excision_. This one says excise for entity 17 these attributes and that will forcibly remove the information even back in time. So it is possible, but anyway

It's time for a little story.

18 years ago David Heinemeier Hansson released this amazing video where he for 15 minutes programmed a blog using TextMate and he said "oops look at all the things I'm not doing" and I loved it. I was writing Java at the time and he was showing off Ruby on Rails and Ruby and the language and it was so amazing. I learned so many things: database migrations, the model view controller pattern, higher order functions like map and filter. I think maybe that was where my journey in the functional programming world started. I learned that code could be beautiful.

And I also learned that I should always have the `created-at` and `updated-at` columns in all my tables. And I did, and I felt like I was now a professional IT developer because I could look at the created at and updated at. I even learned this MySQL incantation that allowed MySQL to just keep these fields updated for me. It was pretty cool.

So let's take a look at our Person table and we will add some `created-at` and `updated-at` fields to it. Let's examine this for a little bit because there's an ID and a given name and I noticed that `created-at` and `updated-at` are not the same, right. This row has been updated and probably ID has stayed the same and probably Sarah the given name has stayed the same. So my theory is that book 23 is the one that was started reading yesterday. Must be right. But look at this: today Sarah starts reading book 77, The Beach, and the same sort of logic goes here. But what if tomorrow Sarah enters into our user preferences page and she finally enters her family name? Now it's becoming hard to sort of figure out what exactly was updated and where did book 23 go?

So this should give you some clue as to why Datomic does not use tables for business critical data. Let's look at the same little sequence of events using facts. So she has a name that was at this time and she has reading 23 which was at that time and when she starts reading The Beach today we just add it onto the list and when she changes her family name tomorrow we also just add it to the list. So not only is there no information lost but now we're keeping track of time for all attributes and their values.

Let's write a little query. This is sort of the three facts that together reads Sarah is reading The Beach. We will replace the concrete name of Sarah with a variable, `?name`. The question mark in front indicates to Datomic that this is a placeholder, and 17 is the concrete ID of one specific entity, the person. We will replace it with `?p` and notice that `?p` is one placeholder and it's used in two places. It's the same placeholder, and in other words it's the same entity, so the person with the given name of name is the same person that reads the book `?b`. And finally we will replace The Beach with just title and now we have something that instead of being about concrete data instead expresses the relationship between these attributes.

So to find something here we will find for instance the `?title` where this holds and we have to pass in some parameters, so we're passing in the database and the name. And the thing that you are looking at now is actually Datalog, which is a Prolog-inspired query language from the 70s, and this is what Datomic uses. Side note: you can use SQL as well, but hey look at this cool stuff.

So to query we pass this Datalog query to the `q` function, passing the database and the name Sarah, and we get back The Beach — which is pretty good. Sarah is reading The Beach today. But why only The Beach? If you remember, Sarah was reading The Hobbit, she was reading 1984, she was reading The Beach. There is something going on here, and the thing that's going on is that the database — the `db` we're passing in there — that is actually a materialized view into the current state of all the database's entities. It is a snapshot of the data as it is right now, and in our model you only read one book at a time — deal with it — so it's just giving us the last one.

But I told you that Datomic doesn't throw away this information, right? So instead of passing in the snapshot of _now_, instead of passing it the database as-is, we can pass it in "as-of" some other point in time, like yesterday, and when we run this query we get 1984. And yeah, she was reading 1984 yesterday — that's great. And we can pass in the day before that, and the most recent information we had about Sarah's reading habits at that point was that she was reading The Hobbit. Now it shouldn't take 10 years to read The Hobbit — maybe she watched the movies. In either case, if we go back even more years, well at this point we didn't know anything about Sarah's reading habits and we get an empty result set back.

And if that's not enough, we can just say to Datomic: please just give me _everything_ — everything, the entire history of the database — and it will oblige. Now we get all of them: The Hobbit, 1984, and The Beach. So Datomic keeps historic data for all your attributes and entities.

Now looking at Datomic's historic features sort of zoomed into just one attribute like that might not give the full image of what's going on. So let me tell you a story. At a former job a few years ago we were asked to create a new report for the higher ups in the business. It was quite a bit of work and it queried a whole lot of the database, but when we were done we could generate a report like this — sort of like this — generate a report passing the database, and it would spit out this cool little with graphs and everything.

And the business people they were happy, they got their data, and they were looking forward to seeing how this data would evolve as time went on. What they did not know was that we were using Datomic and had been for a few years. So instead of passing in the database, we passed in the database as of the beginning of last month, and we got another report just like the database was at _that_ point in time. And we did it again for the month before that, and again, and in the end they got a whole year's worth of reports with the entire database. And mind you, the code didn't have to change! When generating a report for now and generating the report at some other time, the code is the same! Because you're always looking at a snapshot. So you can query the database as it was at any given point in time.

Now like I said the reports were quite involved and took some time to create; there were lots of heavy queries. What do you think we used? A data warehouse? A backup of the production database? Or did we just run it straight in production?

This is a trick question, right. Of course we ran straight in production. Yeah, that might sound crazy. What about those heavy queries? Didn't it grind production to a halt? Well, in order to answer that question we're going to have to learn just a little bit about Datomic's operational model — not much mind you, but a little. And we will start this off with a quote: "Design is to take things apart in such a way that they can be put back together." And look it's this guy again, Rich Hickey, and he created this database, so yeah he loves to take things apart.

Normally a database server is responsible for at least these three things: transactions, queries, and storage. Not in Datomic. Of course the server is responsible for the transactions. Datomic's transactions are ACID in that they are Atomic, Consistent, Isolated, and Durable, so it needs to be sort of centralized. But the fact that the server is doing mostly only transactions means that in Datomic nomenclature we actually call the server not "server" but the "transactor".

But then who are doing the queries? Well, the clients are. The clients are doing the queries themselves, because they have direct access to storage. And that's why in Datomic we call the clients peers, because they're not clients of some server, they are full peers of the database. And the storage is pluggable. Datomic is not in the business of writing files or bytes to disk. You can plug in DynamoDB, you can use Cassandra, you can use any SQL database. And I have personally used DynamoDB, Postgres, SQLite, and at some point for many years actually I just wrote directly to file system, which is not something that the Datomic team encourages but it worked for many years.

Of course when the clients want to write something they send it to the transactor, which in turn writes it to the storage. But what you get here is linearly scalable reads. In other words you can spin up as many clients of as many applications as you want and they never interfere with the work of the server. And how is this even possible? It sounds crazy.

And it is possible because facts never change, because of immutability, because it's eminently cachable. Because you can cache the database data in Memcached. You could theoretically put the Datomic database behind the content delivery network. And the other trick here is that you're always looking at snapshots. So when I start performing my query at some point it sort of freezes the database, and the server can continue transacting stuff, but I'm now looking at the database as it was at that point. And even if I spend 2 hours performing my report generation I will get a consistent view, because I'm not working towards the data set that is constantly evolving. So the clients, they are true peers of the database, and queries never disrupt other services.

Remember this? 

```edn
(q '[:find ?title
     :in $ ?name 
     :where 
     [?p :person/given-name ?name]
     [?p :person/reads ?b]
     [?b :book/title ?title]]
  db
  "Sarah")

;; = [["The Beach"]]
```

It's where we find titles of the book Sarah is reading. I just wanted to show you this cool little thing. Compared to the other stuff this is sort of just a little thing, but I think it's pretty cool. If you look at the `:find` there — `:find ?title` — what if we instead find the name and pass in the title "The Beach"? Well now we will get back Sarah and John, because Sarah and John are reading The Beach. And if we go back, look — title and name, name and title — we don't have to change very much with this query, right? 

```edn
(q '[:find ?name
     :in $ ?title 
     :where
     [?p :person/given-name ?name]
     [?p :person/reads ?b]
     [?b :book/title ?title]]
   db
   "The Beach")
   
;; => [["Sarah"] ["John"]]
```

This part here never changes. It's almost timeless in that it expresses the relationship between these attributes in a way that is not directly dependent on the question we're asking. I think that was pretty cool. So Datalog is expressive in surprising ways.

So by now you are pretty familiar with these: entity, attribute, value, and time — well, I have been lying to you. The `t` isn't time, it's transaction. Here are three facts that were asserted at the same time. They belong to the same transaction. That tells us something important: these three facts are related. Our facts [are] no longer just a mishmash of facts strewn around with no nothing keeping them together. Now they are connected by their transaction. And it's on the transaction that we have the time. But since the transaction is now a fully fledged member of the system, an actual entity in the database, we can add more information to the transaction, like _who did it_ and _why_.

Imagine living in a world where all your data you can tell when it was done, by whom, and for what reason. I'm pretty sure most people in this audience — it's certainly true for me — have at some point looked at a piece of code and thought "what moron wrote this, this makes no sense," and then you do the git blame and of course it's you six months ago. Well with this now you can do that along multiple axes.

So now that we know that transactions are fully fledged entities we can go back to this history query — you remember it gave us all of these books — and add a crucial piece of information: you want to also find the _instant_ at which they were asserted into the database. And if you now look at the person reads row, we have the E, A, and V, and then the transaction, and this refers to the transaction entity. And we can add that to our query, look up its attribute of database transaction instant, and bind the value to the instant variable there, and run the query again. And look, now we have instants for all of these reads. So transactions are first class entities in Datomic.

So we have now reached the final point, the final lesson, and it's going to be a little philosophical. I'm sorry. Because I'm going to ask you a question now that you know the answer to, and that is: what storage do we as developers give ourselves to store our code in? Git, right? You remember the tool that you're using that gives you the full history of everything, that you can look at the code as it was last week, where you can see who did what at what point in time. Even just imagine going back to this — have any of you ever done sort of the FTP into the server and you're copying your code onto the server and just for sake of security copy the old one and add sort of "new old" in front of it? I've done that. It was horrible. So the Git part, that's the good part, right.

And yet we are basically giving our users and our customers the folder. And the number one argument I hear against this idea is that, "doesn't it take up a very lot of space though?" "Doesn't it fill out your hard drives really quickly?" Well, first of all, my text game, it's been running quite hard for the past eight years and it's still less than 4 gigabytes. But the most important thing here is: available storage space since those classic SQL databases were conceived of has increased a _millionfold_. That's six orders of magnitude! Now imagine if your sock drawer had increased size a millionfold — at least possible space to put in there. You probably would not be concerned about can it fit all my socks. You would probably be renting it out, right, putting apartments in there, and so on.

So this is no longer a real limitation. So we can and we should have better tools for ourselves and for our users, and let's treat our users' data with the same care as we do our code. 

And those were 11 insights after 11 years with the functional database Datomic. And here they all are. And that's all I had.

[Applause]

I will be here and I will be walking the floor, you can stop me at any time. I love talking about Datomic. Thank you.
