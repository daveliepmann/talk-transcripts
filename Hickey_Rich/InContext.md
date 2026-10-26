# In Context

* **Speaker: Rich Hickey**
* **Conference: [Clojure/Conj 2026](https://2026.clojure-conj.org/)**
* **Video: [https://www.youtube.com/watch?v=P5eO7BNyJao](https://www.youtube.com/watch?v=P5eO7BNyJao)**

Hello, hello. Welcome everybody. Alex did a good job of saying thank you all for coming, but I'm going to say thank you all for coming and I hope you really enjoy this couple of days of being in the presence of other human beings, which is increasingly rare.

So, today we're going to be talking about context. And that means, unfortunately, I will be talking a little bit about static typings and static things and static thinking. And I want to just make sure that it's clear that there's no fight here, although people get very defensive. 

I think we all should recognize as people in the technology world that all technologies have trade-offs. There's no trade-off free technology or approach to doing what we do. But these are two interesting perspectives, you know, if you use types, you can't go wrong for some definition of wrong. And Greenspun, whose 10th law said, yeah, okay, but you're going to end up building Lisp if you don't have Lisp.

So, what does it mean to be right or wrong? I mean, these are things that come up all the time. What does it mean to be correct? And what's interesting about these definitions is that they bifurcate, right? "Correct" means being free from error, which means being consistent with your own propositions. And then, furthermore, "consistent" has a bifurcated definition, and in this case, it means not containing contradictions. This is the world static typing is trying to enforce.

The other meaning of "correct" means being in accordance with fact or truth. I think people who are forced to use certain kinds of approaches to development these days are increasingly valuing consistency with the world and finding it wanting in many areas. So, we want to be consistent with the world, and here "consistent" means something else. It means standing-with or being-compatible or in-agreement-with something else, something outside of what we're doing. And I will contend that this latter definition of correct is really what you get paid to do. Okay?

So, what does it mean to be static? It means to not be moving. It's a standstill, and in particular, I think the thing that matters in software development is to be static means to be standing still with other things or other parts of your program or other people at the same time. Being static means having synchrony. And synchrony means operating on the same time scale. Means we all have a shared notion of now, and right now we all think these are the definitions we're going to use.

This is not something that occurs in the natural world very often, if at all, because there's no such thing as "distributed now". Right? Anybody who does distributed programming theory knows there's no such thing as distributed now. And I would also contend that in a development effort, as soon as your team gets large, as soon as your program gets broken up into more parts, independent libraries, independent programs, independent teams, independent delivery schedules, you also lose the sense of now. And if you're using tools that require you to have shared now, you're going to struggle to move in a quick and timely manner and deliver for your org and meet your market requirements.

So, the other thing I want to talk about and talking about context is to think about where you are. There's nothing inherently wrong with static tools and static checking and the things that they can accomplish. You have to think about where that's appropriate and what kind of programming that's appropriate for. And I think that Clojure is interesting in being a language that is primarily pointed at people that are doing what I would call information programming, which is not a commonly used term. 

But it should be more commonly used because I think the overwhelming percentage of developers do information programming. They are not writing device drivers, they're not writing compilers. I mean, these are important things. But most people are not doing that. Most people are writing information programs where information means facts about the world, things that are actually occurring, people buying stuff, people moving stuff. You know, products and packages moving around, logistics, people getting sick, people taking, you know, x-rays, events that systems generate, things that are _actually_ happening. And information programs need to track those things and manage the flow through systems that make decisions and support decision-making. 

I think that's what most people do. So, how do you know if you're an information programmer?

Well, most information programs are very clearly situated in some way. They involve a database. That already means, right, it's sort of implicitly, the stuff that's not in your program that matters to you, the stuff that's not in your program that you care about from instantiation to instantiation of your program, the stuff that's not in your program that other programs may care about.

Right? Now you're situated. Now there's this outside world, external systems if you talk to external systems. Handling events. In other words, you write a compiler, a compiler opens a file, it compiles a file. It's driving the entire time, right? Nobody's saying, "Oh, wait, what about this?" You know, there's no like mouse clicks in the middle of compilation that you're like, "Whoa, they clicked on this. Now I need to, you know, do something else."

And another aspect of information programs is that usually they're broken up into more programs than one.

So, I want to just ask the audience, how many people use a database? Use external systems? Handle events? Have their program broken up into multiple programs. All right, how many people have write programs that do none of those things?

One person. What do you do? Two people. What do you do?

>I teach.

You teach. Okay. That's always the out, isn't it?

So, we have a room full of people who are doing what I would call information programming. And I think the appeal of Clojure is the people who do information programming. And there's nothing wrong with appealing to people who do one kind of thing and not appealing to people who do another kind of thing.

So, what does it mean in information programming to get it correct in the real world sense? Well, we have to get the types right...and we have to get the right information from the right database with the right query on the right tables combined with the right information from other databases with the right basis there, too. Process it with the right algorithms on the right thread pool, coordinating with the right external systems over the right channels using the right protocol using the right credentials, dealing with exceptional situations, rendering in the right format, communicating with the right parties over the right channels at the right time, recording the right activity in the right places, and a lot more other stuff, and returning the right answer.

Okay. So, there is a _lot_ of things that we need to make work that are not going to be in static scope.

So, if we look at static types, I think that — you know, I was talking about the right tool for the job. When are static types right? I think static types are particularly good and useful for handling the part of your programs that are _mechanisms_.

And what do I mean by a mechanism? Mechanism means, you know, mechanical, machine. And a machine is a device or a contrivance. Essentially, it's something that you're making up to serve your purposes. But, like, you're inventing it. And an example of this in programs would be a hash table. A hash table is a contrivance, right? There's no hash tables in the world. There's only hash tables in programs. We make them up. And what's interesting about your contrivances and the mechanisms, which are usually sort of at the bottom of your programs, is that they're inherently internal. They're parochial. You made them up, you define what they mean. If you're writing a programming language, you just, you write all the rules, you make up everything. So, you have no challenges in conforming with anything outside.

And, you know, so an analogy in the physical world would be — I would have these connectors, right? I have a DB25 connector, right? It's got 25 pins. This pin means that, that pin means that, and like, what's great about that? Well, you can't accidentally plug a DB9 into a DB25. Woohoo, you know, static typing, no errors, right?

And there's a lot of utility to that. You know, it makes for compatibility, and you are more confident when you plug in two things with the matching connector that they're going to do something useful together. But what about doing that using that same approach for information? Do we do that? Can you imagine taking your DB25, and every time you have a new piece of information you want to have go over that wire, you put another pin on this connector and you make a different shape connector, and you break everybody who is using the other connector and say, "Oh, just start using this connector." Right?

It doesn't matter if your language or your tools — you know, if you call that refactoring and it makes it easy — this is still what you're doing.

And this is not good, right? And think about it. Think about your older computers. How many different kinds of connectors did you have on your older computers? You had one for printers, one for the modem, one for the mouse, one for this, one for that. How many connectors do you have on your computers now? USB-C, Thunderbolt — like everything happens over that same thing. And what's interesting about that is that you want to have fewer of these things. The fewer you have, the better off you [are]. And what travels over that universal thing? (That's the "U", right?) What travels over it? A heterogeneous set of stuff. It's a heterogeneous conveyance. This is essential to making flexible systems.

So, should we be concerned with getting it wrong? No, we should _know_ that we're going to get it wrong. You're absolutely going to get it wrong. You need to write programs when you're dealing with the outside world that you _expect_ to get wrong.

Did anybody watch Artemis? The Artemis flight or whatever. It's a phenomenal lesson in dynamic programming. Right? You put the spacecraft into space. Okay, well, there's no recompiling or do-over or anything after that. What happens? You know, you made your best effort. You're trying to do things. Some things are not working. Some things happen that you didn't anticipate. All of a sudden you need a whole bunch of visibility, right? What happened? The toilet didn't work on Artemis. They had people looking out the window while they're trying to evacuate the toilet, to explain what was going on. Like, this is the real world of trying to make a complex system work in the real world or in space.

And I think that making a bank, or making something that works on a credit card, or writing a big e-commerce system or a medical information system or whatever — these are things that you're not going to completely know how to do in advance, and you're going to need to learn as you go.

So, successful large systems have some qualities I think they always share. They are _always_ dynamic. Big systems are dynamic. The internet is dynamic. Right? Large systems are not dynamic — nature and natural systems are all dynamic.

And it's interesting, the root of "dynamic" means _power_. And I think this is absolutely true. You have more power when you're being dynamic than otherwise.

Large systems are also flexible, and that means that you can bend them. You can make them do something slightly unanticipated — not necessarily completely unanticipated — without them breaking.

And the third quality I think all large systems share is that they are observable.

So, one of the questions you have if you're going to be programming is: how many languages do you need to do your job? Do you need a language for your mechanisms and a different one for your information? Right? Because you're going to have some mechanisms and some information. I mean, how many people routinely write something like their own new kind of hash table? It's very uncommon. One person and two people in the room raised their hands, right? It's very uncommon. Mostly, you're not doing that. You're reusing those things.

But, if you're working in a language that's primarily oriented at making mechanisms, what is the language giving you for what you have to do every day for your primary domain problem? For the information problem? And usually, that's nothing. You're going to have different libraries, a hodgepodge of stuff, all kinds of impedance mismatches, conversions, dealing with JSON, and it's just — this is your problem. But, this is your problem! 

I think you want a language that helps you with your problem, that works in the space that you're thinking about. So, Clojure's approach is to use an information orientation to all programming — between parts of the program, how the program will talk to itself, and how it will talk to the outside world. Right? That's why we do, you know, everything is a map. Dev time and run time are not different. There's many things that are involved in making something flexible, and I think there's a separate talk, actually, about Clojure and every way in which it tries to embrace that, but specifically today, first-class names are very important, late binding is very important. You want to be able to adapt your program and manipulate more information without more mechanisms, without more stuff. Right? Equating information with the mechanism, I think, is an absolute anti-pattern. And you want to develop in your running program.

So, what benefits do we get from this? I know everybody here is, I think, aware of most of these things, but I'm just going to say them for the people at home. You spend far less time inventing new constructs, right? People here are rarely doing that. Your program is much smaller, right? And it's a fact. It doesn't matter what language you're using. More code is more maintenance cost. It's more bugs. More code is more bugs in every language.

You spend more of your brain space on your domain and less on making up, you know, fake abstractions. Your program is more easily broken apart. You get observability, a wire format, and durability support for free. It's not necessarily that you're going to use [EDN](https://github.com/edn-format/edn), you know, everywhere forever or have that be your database or anything like that, but it is a high-value proposition that when you want to see something, you just look at it. And when you want to manufacture something to pass to something, you can just manufacture it. Right? These are universal tools that are available. You want to have a properties file, you know, you [don't] need to think about it. You're just going to use EDN.

So, this is an approach that says information is the dominant way we're thinking about the world, and we're going to do that all the way down. And I think that this is interesting because what happens is if you make a large system not using an information-oriented technology, what you find is as your system gets larger, it becomes an information system about itself. Right? Who has programs that produce logs? Yeah. Everybody's logging. Who has dashboards to see how the program's working? Right? And etc. etc. etc. like telemetry and whatnot is super important. And what are you going to do if your language has thrown away everything about how you thought about things during compilation? You have to reinvent all of this stuff to get this information at runtime as opposed to using a language that says, "You called this in your program, and I'm showing you this in your log."

And yeah, and we talked about this REPL capability. So, there are absolutely trade-offs to doing a dynamic style of programming, and you shouldn't be afraid to acknowledge what they are. One is that static programs can be more efficient, right? Static programs can compile into, you know, go to offset 42, and computers love to do that. They're very fast at doing that. 

The other big trade-off, and this is what these constructs are pointed at, is that if you don't have really good documentation, it can be unclear as to what the requirements are for a particular function or what it returns. This need not be this way. People could write better documentation, and they should. And if you look at, you know, good Common Lisp programs from back in the day, I mean, those people were documenting like crazy, crazy good documentation.

But when you have mandatory constructs, you sort of get a form of documentation for free, and you're forced into doing it. So, no one's forcing you, and you're not doing it. And that's true. People walk up to Clojure code bases and are like, I wish I was back in static typing land. And they're not thinking about the trade-offs, they're thinking about the benefit. And the benefit was I could see what the expectations were.

And the other thing is that if you have an inconsistency in your program, right? If one part of your program was supposed to pass X, Y, and Z, and only passed X and Y, and this guy's expecting X, Y, and Z, and doesn't see it, that turns into a breakage that will turn potentially into a null pointer exception far away from where the mistake was made or even detected. And so, that's what these features coming in Clojure 1.13 are about: checked keys!

So, a longer time ago than I wish I had to admit, I gave a talk called [Maybe Not](https://www.youtube.com/watch?v=YR5WdGrpoug), which was engaged with these kinds of issues and how difficult it is to deal with context-specific requirements, because that's what we're talking about when we're talking about documentation. We're talking about "I'm looking at a function, I have expectations of the function about what it's going to be passed, and maybe giving me expectations about what it's going to return. How do we do that?" And the most common thing done everywhere, including in Clojure by most of the schema libraries and by Spec 1, was to have context-independent aggregate types. That there's this aggregate type called, you know, whatever, and it has these six things in it.

In Spec at least, it didn't say those six things are these types. Those things were independent. I'll talk about that in a second. But that talk was about the fact that that doesn't work. There's no possible way you can have a context-independent, you know, single definition that will work in every context. You don't know what's going to happen in the future. You don't know what things are going to do. And sometimes the contexts disagree. So, something's optional here and mandatory there, or mandatory here and optional there. There's no way to say that.

In addition, they do not nest well at all. They do not compose, that kind of approach. Even if you thought you had all the maybes right over here and over there, when you nest them, now well that's not — that's maybe there, but not maybe inside this.

And I speculated about things we might do in Spec. So, the fundamental idea here though was to split out the two different things, right? I still contend RDF had it right. The right way to think about properties is independent of any aggregation. You have a property with a global name, has a global semantic, and a global, probably, scalar type. And that's a thing. And you can combine those things and talk about an aggregate, or you can talk about those things as being required in a particular context.

So, that's important. And then we have the separate idea now where we're not doing these two things together. We're not saying struct `foo` and then has X, Y, and Z, and X, Y, and Z are these types. We're not doing that anymore. We're saying X, Y, and Z mean this. And here, I'd like to see X and Y. So, we have the context-specific part, which is about aggregation, right? This is a collection of things I expect to flow through here, and requirements. So, that's what Checked Keys is pointed at.

So, this is a form of context, right? So, what does context mean? It means the circumstances that form the setting for an event, statement, or idea, and in terms of which it can be fully understood and assessed. And I'm going to add "and used." In other words, context is also important for use. I'm going to take advantage of this thing. So, not only do I understand it, I'm going to try to take advantage of it. And it means "weave together," which seems particularly dangerous to our thinking world here, where you know, I showed you that picture where weaving was bad. But the fact is that weaving happens, right?

And there's sort of two forms of weaving. There's "I know about this weaving and I'm going to say, 'Ooh, this thread and that thread I'm going to do this with those two things.'" And then there's weaving which is about sort of flow, which is inherently going to happen. But you can do flow in a highly ignorant manner. I'm going to advocate for ignorance here quite often. The less you know, the less you need to know in a particular context, the more flexibility you're going to get.

So, you don't want to know about everything that's woven together. You want to be able to tap into a woven thing, access what's important to your context, and leave the rest of it alone, and let the rest of it flow.

So, there are different contexts. We talked already about your program in the world, your program being situated, other programs, databases, things like that. And now here we're talking about more in the small. The context is in this part of your program, what are the expectations? What are the needs?

So, check-keys is a different approach. It came out of thinking about "maybe, maybe not," and trying to continue working on it for however many years it's been. And I had a few things I was trying to accomplish. One is I want to have something in core Clojure. I mean, I do think this is a legitimate criticism of Clojure, that it does not have tools for you to do this. And without those tools, people are not documenting, and therefore it is a challenge for people coming to new codebases, moving between codebases, adopting Clojure, and things like that.

So, we'd like to have something in core Clojure, but not dependent on spec. I didn't want to privilege spec here. And spec does very powerful things with powerful tools, but it should be more lightweight to be a proper language construct. So, something that could go in `defn` — and it ended up not being a part of `defn`, but a part of destructuring. But could go in place in the syntax.

So, it ends up that destructuring already was involved in the kinds of things that matter for doing this context. It's already a place where we talk about context-specific stuff, right? We have this map, and we're trying to pull stuff out of it. So, implicitly we get some expressions of what our expectations are. And it's context-specific, right? Here, I destructure this thing here this way. I might destructure that same thing over here differently, because I care about different parts of it.

So, these are not global things. There are no global destructuring things in Clojure. And the other thing that destructuring had that was useful is map destructuring already had syntax and a method for talking about nesting, which is the other really challenging thing about doing context-specific requirements. It's _never_ about just a root level. You're going to have some nested thing, right? I have the person records coming through, but the person record has an address record. (I'm saying record, but you know, like, maps.) It has an address in it, and the map — the address is its own map, and it has these things in it.

So, you can't just define, you know, address over here and say address is like the third thing in person, because in a context, I may need the zip code only. In another context, when I'm taking a person, I may need the whole address, because I'm going to try to send them a package.

So, you need something that talks about nesting, and destructuring already did. So, the idea behind what I'm going to show you is that we're going to move from destructuring, which isn't an expectation system, right? It implicitly reveals some expectations, but it's incomplete, right? It's just what you're choosing to bind. So, it's not doing the whole job.

We're going to try to move from that implicit expression of intent to an explicit superset of your bindings. You may have some things you want to bind. You may have nothing that you want to bind. But we're going to enhance destructuring to allow you to explicitly talk about things you might _expect_, either optionally expect, or you require.

So, in addition to the optional stuff — and what we're going to say is of existing destructuring, that everything is optional, right? If you make a destructuring thing, you get a default, right? You get a default of `nil` for the bindings for which there are missing keys. So, that means it's optional, right? It will work without the stuff being present. So, what you have today is completely optional. And there are two forms. There's a baked-in default of `nil`, or you can use `:or` to say if they failed to provide this, use this value.

To add syntax to enforce required keys — to express and enforce, and those are two different ideas. I hope I don't need to say this: of course, we did this by only adding stuff and breaking nothing, right? Which is probably rule number one of flexibility, which is caring about other people, right? And thinking that breakage, even if you could automate it, is bad. **Breakage is bad.** And you really can avoid it. When you look at how rich a thing we just glommed onto this, you see you can actually accomplish a lot without breaking things.

So, again, I know probably the people that show up here are among the most experienced Clojurists, but not everybody is. Just give you a little refresher on destructuring, because a lot of people don't use all the parts of destructuring, and when they see them they're like, "What? You could do that? What?"

So, right now today, before the new feature, destructuring is strictly about bindings. You use it when you're trying to create bindings, right? You're trying to create a local variable whose value is part of — comes from part of this map that's been provided. And really I'm only talking about map destructuring today, not otherwise. But this is true, obviously, of sequential destructuring. You provide this form where the map is expected, right? So, I'm taking this argument and instead of calling the argument a map, I call it my use a destructuring form. It will create locals bound to the values from the map or these implicit values.

I already talked about that being incomplete and everything being optional. So, that's the purpose of destructuring today.

The shape of destructuring, the syntax of destructuring looks like this. It's a map. So, a destructuring form for maps is itself a map. It has keys.

The map destructuring map has keys and they can be one of three things. 

They can be binding names. Now, there are people who only use keys and they never use the binding names part. I mean, it was the primitive destructuring used binding names, you know, before there were keys. And now nobody uses them unless they need to rename a key or something like that. 

But they can take a simple symbol, right? As the key in this destructuring form. 

They can take another form, right? A map or a vector, which means further destructure the value at this key. This is how you handle nesting.

So, it can be a symbol, it can be a form, or it can be a directive, which is a keyword. Now, this is a new word, the new _tagging_ of what the keywords are, but I we need it because I need to talk about these directives more because we're actually going to expand the space of directives. 

So, what are the directives we have now? What are the keywords we can see in destructuring? We can have `:as`, which just names the entirety of the map because you use this destructuring form, you don't have the ability to name it except for this way.

We have `:or`, which allows you to define the default values for bindings for keys that are missing. 

And we have `:keys` and `:syms` and `:strs`, which are ways to easily create bindings that have the same name as the key.

Okay? So, these are our current directives.

And this is what it looks like in practice. And again if people don't do a lot of nested destructuring, they can get confused by that. But we say `let` [and] `name` is a symbol, right? So, we're going to have the local variable name come from the `:first-name` key of the map that's provided.

We're going to take whatever's at the `:address` key, and we're going to break it into `:city` and `:zip` and bind local variables to those parts.

We're going to create a local variable `id` from the keyword `:id` and a local variable `nickname` from the keyword nickname.

And if they don't have a nickname, we're going to call them `newb` -- or noob, depending on how old you are.

And then we pass a map and it takes this map apart and sticks it in local variables. So, then we'll have code that uses these locals. This is destructuring today.

So, where do we go? So, we're going to say, "Now of destructuring, destructuring is about binding and requirements." All right? So, we're going to talk about expectations, things that are required. We're going to have new syntax to talk about required keys.

We're going to have new — and by this I mean additional, not changes to things — additional syntax to talk about additional keys not used in bindings. This is kind of a mouthful. Hopefully, it becomes evident why you would want to do that later. But, you know, I don't want to make a thing that forces you to create local variables in order to document stuff or to have requirements, right? Sometimes you consume keys of the map by using, you know, get the keyword or the keyword as a function or you map something or you use `select-keys` or I mean you don't have to use bindings. So, this shouldn't be dependent on making more bindings and it isn't. So, you have ways to talk about keys without creating more bindings.

And now there's going to be some new runtime enforcement. Right? So, you say something's required, that's useful as documentation. What's going to happen if somebody doesn't give you what you want? Well, that's about policy. So, there's going to be enforcement.

So, we're going to have some new directives and I'm also going to invent another term for `:keys` and `:strs` and `:syms` so I don't have to keep saying `:keys` and `:strs` and `:syms`. Which is these are _'binding list' directives_, right? These are directives that take a vector of bindings today. And that list of bindings all become independent bindings with the same names. So, now we're going to have variants of those binding list directives with bang (`!`) at the end: `:keys!`, `:strs!`, and `:syms!`.

And they have exactly the same syntax as the originals. And as I enhance the syntax for the originals, it's same thing here. So, the same syntax. Inside the square brackets, same syntax. So these are just like that. They're shaped the same way. They do binding the same way. The difference is that these directives say these keys are required. I expect you to provide them and if not, something's going to happen. I'll tell you what that is in a second.

You cannot have defaults for required keys. You won't get any from the construct. There's no supplying `nil`. And you cannot put them in `:or`. You can't talk about them in `:or`. You said these were required. Providing a default means they're not! So, you can't provide a default.

Okay, so it looks like this. It doesn't look a lot different than before. Right? We added the bang. But we said now that I want to see `a` and `b` and so we pass through a map with `a` and not `b`. And I said, "You're missing the required key `b`." So the default behavior, the default enforcement of required keys is an exception will be thrown.

Okay, so now I said you're going to have the ability to talk about additional keys. So this is in all the binding list directives, the original set of three and the new required variants. Which is that you're allowed to follow the binding name. So I can that square brackets where I said `a` and `b`, I can now say ampersand (`&`) and follow that with explicit names of keys, like keywords if it's `:keys` or symbols if it's `:syms` or quoted symbols if it's `:syms` or strings if it's `:strs`.

And these are the explicit names of keys. These are keys for which I'm not creating bindings. I don't want to talk about them in terms of binding names. So I want it to be clear when I'm glancing at the destructuring form what locals I'm getting. So I'm not going to try to be clever, although I did try to be clever. I've discovered that was not a good idea.

So you have to say that explicitly. No bindings will be created for the things you put after ampersand. And it documents that they're allowed if you're just using an irregular, you know, non-banged variants. And if you're using the required variants, it says, "These are now required." So it's a way to require things that you know you're not creating bindings for. You're going to use them in some other way.

So that looks like this. We moved `b` to — you know, where I don't actually need a binding for `b` in my code — so I'm going to move `b` to the right side of the ampersand and talk about the keyword explicitly. I'm making the same expectations. I just don't get a binding for that. And I get the same error.

And so now we can talk about keys after ampersand. This allows us to have a complete specification without, you know, forcing you to just _for effect_ create bindings.

Okay, so that's great, Rich. You gave us this thing. You can say the thing. You can slap my wrist, whatever. But, you know, a big part of talking about the static part of this talk is — I think what people expect when they get tools that let them do schema or whatever is like what they have in Java or whatnot, you know, which is if you didn't give me what I said, it's wrong. And that is brittle. That's not the approach you want to take, right?

This system so far says you have to give me what I need. And that's different from "did you give me what I said," right? It's not wrong so far, and it will never be wrong in anything _I_ write, to pass more stuff. I'm never going to give you a closed thing. I'm not going to apologize at this point. It's not going to happen. So it's not a prohibition system.

And Chouser isn't here this year, but I had a conversation with him a year ago where he said, you know, but there's just sometimes where, you know, I need to know — like I need to know I have only what I care about, right? And there's two ways to get only what you care about. You can deny anyone from giving you anything other than only what you care about. That's prohibition. Or you can do what we're going to talk about now, which is selection. I'm going to give you tools that let you _get_ only what you care about without disallowing things you don't care about.

And this is sort of one of the recipes that's throughout a lot of Clojure. There's all kinds of these recipes for flexible systems, and this is part of it. So we're now going to reify this concept of the _selection_. And what I'm going to say is that a "selection" is the structure that's implied by the entirety of your destructuring form. So all the keys you talked about, all the keywords, all the binding names, all the nested structure, whatever — that implies a structure. It says "I like these keys here, and under here I want to see these keys. I like this." It's sort of orthogonal to them being required or not. This is a structure. This isn't about how it's used. So it's orthogonal to that. It's orthogonal to whether or not you are binding anything. But if you look at any particular destructuring form in this new world, you can say there's a selection implied by that. And then I'm going to give you some directives that take advantage of this selection.

So let's first just try to see a selection. This is a slight modification to what I showed you before that just uses a little bit more of the new stuff. `name` is bound to whatever is at the key of `:first-name`. We still have the `:address`, but we are only going to bind `city` from that. We still want to see both `city` and `:zip`. `nickname` is optional and defaulted in `:or` as it was before, and `id` is required and not bound.

Okay, so I hope that's clear from what I've been saying so far. So what are the bindings? And I hope this is good — like, when I glance at this now, the only unqualified keys are — the unqualified symbols are the bindings. So you could still at a glance see that this binds only `name`, `city`, and `nickname`. Right? Everything else is explicit.

So those are the bindings. And then what is the selection? What is the structure implied by this form? It says at the root I want to see `:first-name`, `:address`, `:nickname`, and `:id`. And inside `:address` I want to see `:city` and `:zip`. Now I'm not saying — I could use, it's optional, it's required — it doesn't really matter. This is the structure implied by this form.

Okay. So now, what can we do with that? If we understand what a selection is, what can we do with it? Well, we're going to have these new directives. The first one is `:all`. It says, I want to see the original map, but I want all the missing keys to be filled in with their `:or` defaults. So that's what all means. Everything you gave me, but the defaults are in place. And these directives all work deeply through the map, all the way down, all the nested maps.

Okay, so we have the `:or` defaults. We have `:select`. This is the one that you would normally get through a prohibition system. Only the stuff in my selection. So this is only the things that are in the selection, but also with the `:or` defaults in place. So that's what `:select` means.

And then there's `:excess`, right? Which is predicative, which means if there is no excess, this will be `nil`. So all these directives — I should have said somewhere, I'll show it to you in a second — all these directives are all in a binding name: `:select` in a binding name, `:excess` in a binding name. So this will be bound to `nil` if there's no excess stuff. Otherwise, the excess is whatever was coming in, whatever was in the input, that's _not_ in the selection. And this is sort of a deep set minus, right?

It also means that this other set algebra works, which is: the selection — the `:select` plus the `:excess` equals the `:all`. These are important properties. They'll really let you write interesting programs. And in particular, the point of `:excess` is not to throw an exception if it's not `nil`! It's not to turn this into a prohibition system. Please do not do that, right? This would have done that if that was a good idea. That is not a good idea. This is not why that's here. But it does leave people wondering what — why — what can you do with this?

And what you can do with this is you can write what I would call a well-behaved conveyance, a conveyance that's ignorant of some of what's coming through it. That's super important. Anybody who uses cascading processing things or transforms or intermediaries or anything like this is how the world works. You need to be able to have trucks that can handle whatever you put in the truck. They don't need to know what's in the truck. And they go someplace and they take out the package for you. That's what you need to build a big system.

So, in the case here, you have some logic you want to do. You are interested in this part. You need this information to do it. You're going to do something and end up with, for instance, a transformed map. And you're going to need to reconnect the stuff you didn't understand or don't want to know about to what you made and flow that down. Or you may also want to just pass the excess to somebody else who deals with the excess. That's fine. You can flow these things, but it gives you power to manipulate information you do not need to understand.

That is flexibility.

So, this is `:all` in practice and this is the same binding form. We're going to use the same input map. So, that's not going to change. The only thing that's going to change is they're going to put in one of these directives and show you what it yields.

So, let's look at the input so we know what's going on. It has provided `:id`. It has provided a `:first-name` but also a `:last-name`. We never talked about `:last-name`.

And it's provided a `:city` and a `:zip` but also a `:state` which we never talked about. So, this is an input that has some extra stuff. It's not missing anything because otherwise the requires would all just fail.

So, this is all code that is correct. So, what do you get if you say `:all`? Well, you get everything that was talked about, but you see we've subbed in the `nickname` because the nickname was not supplied.

Okay? So, it has `:first-name` but it also has `:last-name`. It also has `:state`.

So, this is everything that came in with the default supplied. That's `:all`.

Okay, `:select`. Right? We're going to have just what we selected. We say select a cell. What do you get? Nickname, ID, first name, city, ZIP. That was everything we said -- and address, right? That's all the structure we talked about in the destructuring form and _nothing_ else.

So this is the way you get what you want so you can work in a context. Maybe you need to call somebody who's brittle. Right? You didn't make it a brittle thing. But you know there are other brittle people that you need to talk to, right? And they will, like, fall over if you give them a new thing. 

I mean, why is this important? Get in the real world. How many people have ever tried to update a client-server system where the clients and servers are owned and run by different people at different time frames? Right? Like is that something you want to do with like D-sub DB-25 connectors? "Oh, I have a new connector" and, like, you're instantly broken.

How many people have ever tried to solve that problem in a world where you have a cluster and you're rolling updates across a farm of machines? So not even all your own machines on one end are doing the same thing.

You need stuff like this to build systems that can do that without driving you crazy.

Okay, and finally we have `:excess`. So excess is just the extra stuff. So `:last-name` we never talked about and we never talked about having a `:state` and `:address`. And so this is the extra stuff.

Okay, so now we get to something I think is super interesting and I'm going to have to go even faster, which is enforcement. So far we've used exceptions. And that is an okay way to have one part of the program say to another part of the program that part of the program, you're broken.

But how many people have to deal with input that comes from databases, input that comes from external people who are calling your API? How many people do either of those two things?

Right? So like going and looking in the database and finding that something is not there, well, that could be because the org doesn't know that piece of information. That's something you have to be able to deal with. You're not going to fall over. You shouldn't be using exceptions to do normal flow of control.

So, there's two different policies you might want. You want one policy for "my program is broken," which may be a direct call, but it may also be the database. You could be — your program is broken if you look in the database and the ID is there, but the record was never initialized because your code expects this other part of your code to initialize the database. That's information that comes from outside that reflects a program error not being there. Or you have these cases where external things are supplying stuff.

So, we want some other way to do this and this is again — you have one language and two contexts. How do we talk from the language to itself? And how do we talk about the outside world where we need to be graceful?

It's the same expression of what you care about, "I need this thing to do my job." But if an external entity didn't provide it or it's not in the database, some flow of control needs to go and manage that. Talk back to them, talk to the user, put up a new screen, go to a second source of information to see if we can find it there...not throw an exception and fall over.

So, now we have this additional directive called `:missing`. This is going to say, instead of throwing on required keys, we're going to collect them into a map. We're going to hand you that map and say, "This is what's missing."

Now, of course, if you're doing this, and you're using bindings, you're going to have to get values for those bindings and those bindings will be `nil`. So, if you have missing stuff, you're going to have to look at what it is and realize that those things have been bound to `nil` because I have nothing else I could do for you there.

And you couldn't have defaulted them, right? Because they were required. We're talking about missing required keys.

But now you can do programmatic handling. And this also is predicative, right? If nothing is missing, `:missing` will be `nil`. So, you have an easy `nil`-punning test. If nothing's missing, we're fine. If something's missing, go through this path where we report, branch, log, whatever. Real world stuff.

So, this is what `:missing` looks like, right? Now we have a different input because I'm not going to have everything. And we don't have the `:id`, which is super critical, nothing can happen, no other calls can be made without it, and I can't do something important without your ZIP code, and you get this kind of unusual looking return where the structure has only keys. And it's like this because we want to support nesting, and it has the same form as everything else. So, this is not an information conveying structure, it is a key-only conveying structure, but it is a map of maps to deal with the nesting. So, that's the kind of return you get, and you'll use `keys` to look at like what exactly is missing here.

All right, so we need to broaden `:or` a little bit. It was just for bindings, now we can talk about keywords, the keys specific explicitly in or for optional things.

And you need to think about `:or` a little bit differently. It used to mean defaults for bindings. Now, that's a subset, that job is a subset of a bigger job, which is defaults whenever this system needs a value at a key that's not there. So, that could be when I need to get bind, you know, supply a value for a binding, but it could also be when I need supply a value for one of these selection directives. Right? So, whenever I the constructs need to produce a value for a key, and the key is not there, or lets this be the value for that key. So, this will be what appears in all in those other things.

And remember, there's no defaults for required things. So, the last feature we have, which is very recent in the latest thing we shipped today, which is to say this is cool stuff, this is really powerful, it seems a shame for it to be locked into destructuring only. Right? Which is kind of a fixed position, and it's not very programmable.

So, we have this notion of a `selector`, which is making a programmable or program-useful version of the same logic.

So, `selector` is a macro. It takes exactly the same destructuring form I've been talking about with exactly the same semantics I've been talking about and returns a function which does that same job. It doesn't do any of the binding because obviously we're not in a binding context, but it will support one or more of these directives. `:select`, `:all`, `:excess`, and `:missing`.

So, we get a function back from `selector`, which is a function that takes a map and gives you the selection results.

So, using it looks like this. I have a `selector`. It looks exactly like that binding form. I'm going to map it across these data structures and get normalized data. In this case, that's what I'm using it for, but you could use it for anything. You could use it for validation. Map that there's nothing missing these things. Map all the excess of these things. Map `:select` in this case, so I get my normalized data with all my defaults in place.

So, this is all that power now in a programmable way. And this is one of the things you can do in a language like Clojure, right? There's not this bifurcation between the language does this and throws everything else away and everything about runtime is your problem and you don't have like the same language for the two things. You have one language that does both things.

So, finally, I implore you to use this for good and not evil. Right? These are tools to support _flexible_ information programming for the dynamic context you're in.

Select things out, don't reject things. Think about flow. Think about conveyance. You have some power maybe you never use because you were afraid, right? Because I would say doing the things that this does manually is really not trivial. It's like it's not like rocket science, but it's code that's easy to get wrong. Walking through nesting, using `walk`, trying to build stuff back up. We will also, I should have said before, there's also merge deep coming so that you can do that algebra coming back so you can actually merge deep my transformation of `:select` with `:excess` and get something to pass along.

I think one of the most interesting insights I had in doing this is that I think that dynamic systems support learning as you go. Right? As opposed to static systems which require you to be a know-it-all. You have to know it all before it compiles.

And that's not like "big system"...you never know everything that's going to happen in big system. So you can learn! You're interactively developing. You can see all of your values. You can develop inside the middle of your running program with your database loaded. All those things.

But you do have the ingredient — I don't actually know what's in concrete, but — the what is in concrete is here, right? You _could_ make concrete. So please just don't do that. But otherwise have fun. And thanks for coming.
