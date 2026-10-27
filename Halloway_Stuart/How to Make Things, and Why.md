# How to Make Things, and Why

* **Speaker: Stuart Halloway**
* **Conference: [Clojure/Conj 2026](https://2026.clojure-conj.org/)**
* **Video: [https://www.youtube.com/watch?v=7yeH1DqK984](https://www.youtube.com/watch?v=7yeH1DqK984)**

So, I'm going to talk about making a living. And being a maker and working with people who create things. And of course, because this is a Clojure conference, I'm going to start with a definition slide.

—Actually, we interrupt this presentation to tell you that definition slides for words that are in the top 1,000 most frequently used English words are rude and no longer allowed. So, I will not be defining "make" today.

Instead, I want to talk about some of the things that making means to me personally. I think a lot of people have read Mihaly's book about flow. And this is a big part of the fun and the joy of making things. It's not necessarily in the finished product. Right? It's the friends you made along the way. And so, being totally absorbed in something and losing track of time, feeling like you're in the zone. And then you have a fancy word, of course — this is a Clojure conference — _autotelicity_, right? The job feels like the end in itself. And so, this to me is one important part of why I like to have a career making things.

Another reason is community. The internet has allowed us to see what 'making' is like throughout the planet. And maker spaces and the maker community are one good example of this. The Clojure community is a good example of this. But an important part about making things is making things with and for other people.

Another part of making things is the skills that you gain and the satisfaction of developing skills. And a lot of times we talk about `techne`, right, the root that drives "technical" and "technology." And I'm working on a "technical" task. But today I'm going to be talking a lot about metis. And I'm going to use James C. Scott's definition, which is not a standard definition, which is why the definition slide is allowed — so for those who are keeping score on that. It's a wide array of practical skills and acquired intelligence responding to an environment, and it comes from repetitive action _with friction_ over time. Like you're doing something, you're doing this task and it has slight variation over time, and that's where skill development happens.

And I've come to think of the classic "how to draw an owl" as a joke about techne versus metis. Right? Techne is "draw the circles." And then there could be a lot more techne, right? There could be a ton of instruction. There could be whole books of instruction, but eventually you still have to draw the effing owl. And there feels like there's something tacit or ineffable about the process that does not get captured in words.

Now another important thing about making things is we get to make a living making things. And at the first Conj, I don't remember how many people were able to raise their hand and say, "I do or have programmed Clojure professionally." But just a quick show of hands, people who have made a living using Clojure, right? And that's just spectacular. Right? That's what an amazing thing that we have to share together. And Clojure has been particularly good among programming languages on various metrics over time. I'm not going to belabor that today because I need to get on to some more important things, right? We all know this already. We're making a living by making software.

And everybody knows how making software works. It works like everything else. You start with an idea, and then something happens, and then profit. And in our case, that something that happens is programming. This is the picture of a programmer at work from Wikipedia. So, if you want to know what programming is, it does not get more official than that. And I don't think you can tell how many characters per second or minute that she's typing there, but I'm sure there's a lot of typing going on. It's got all the canonical elements, right? The human, a keyboard, banging away, and a big monitor.

But as you program for a while and as you experience that friction of doing repeated tasks, and you start to develop metis, and you start to develop a set of sensibilities, you realize you need some more advanced techniques than programming, and you move — when the going gets tough, the tough get meta — and they start metaprogramming. And Paul Graham has a quote, which actually Chris already used the exact quote that I was going to say here, so it's already been said, you have to go back and watch Chris's talk. Right, this is the quote about, you know, macros are powerful and challenging, and it's in poor taste to use them unless you need them.

In order to flesh that out, I did a broad survey of all the people who have ever talked about metaprogramming, and found that it's just these two blowhards, Paul Graham and this one other person who dumbed it down to a Fight Club joke, but it's basically the same idea.

Now, metaprogramming is a thing that, however you talk about it, it's not the first thing you learn when you're programming, or it hasn't been until now. And in the last few years, a new way of programming has emerged that some people say is set to completely transform the way we write software. And it's called stochastic language model metaprogramming.

And so the idea is we're going to take a hard problem and decompose it into two harder problems. Right? Writing with language and metaprogramming. And so then you can go back to the making-software slide and say, "Okay, now we know how we're going to do things in the future." Right? We're going to have an idea, then we're going to use stochastic language model metaprogramming. So this is where you interact with a statistical model of language and try to trick it into doing metaprogramming, writing the code for you to produce a working system.

Now, one of the problems with this is that it's really big on the slide. So in fact, it's so big it pushed the human right off of the slide. So, obviously, there's an acronym. And so, the problem with this acronym is that critics immediately started saying, "Oh, slumming." Right? Which was not going to gain a lot of traction. But advocates for this style of programming said, "No, no, you're missing it. This is slimming." This is getting rid of the fat in the development process and leaving the pure essence.

Well, what is the pure essence? Well, we actually have a quote. I'm not going to say who this quote is by. This is an actual quote about a product recently. And it says, "Imagine you had a second mind that did everything for you, to leave just the human part to you. And the human part is just wanting and dreaming." This is a real quote by a real person about a real product. I appreciate this summarization of the human experience.

And so this line of thinking leads to a better acronym, which is Vauntingly Ignoring Basic Engineering. You don't have to know how anything works. You just have to want stuff. And in fact, I don't really think that's strong enough, because if it's really just wanting and dreaming, you can really Vauntingly Ignore Basically Everything.

And of course, this has an easy acronym, which is VIBE-ing. And so this acronym gained popularity among the slumming or slimming metaprogramming or however you want to call them for a while. But it has not been the word that really dominates. And this is kind of maybe a little bit too pointing at programmers. And so we needed an easier thing to say that we could explain to people who were non-technical about what was going on. And so we came up with this two-word marketing term.

"Gâshûrz thrakatulûk". This is in the dark speech of Mordor. But rendered in the common tongue, it's "artificial intelligence".

Now, we're technologists. And so the first thing we do when we're faced with a new opportunity is we ask no questions about why or what problem are we trying to solve. We jump in and try to make it work. So that's what I did. Starting in January of 2026, having been on the sidelines of this movement prior to that, I jumped in to ask myself and answer for myself the question: does it work? Now, stipulate this is a bad question. I'll come back to that in a minute. But it is the way we think about technology sometimes, right? I want to go play with it. I want to see what I can get it to do.

And there's a lot of problems with making this work. Natural language programming — we have known since Dijkstra, if not before — is not a good way to express your intent. It's not precise. And nothing about the last five years has changed that. Right? Natural language continues to be imprecise.

Meanwhile, again quoting things that are like 50 years old, coding is not the hard part. Right? Fred Brooks said, "Look, if you could make coding costs zero, you still haven't really moved the needle very much on being effective."

So there's a lot of reasons to worry about this from first principles. But we don't have to go from first principles, we can look at productivity studies. And I have. And they're garbage. And some of them, of course, are vendor-motivated marketing, but even where people have tried to do a good job — and this shouldn't surprise you very much, because most people in this room probably have not made technology decisions based on peer-reviewed studies about productivity — I have, over the last 20 years, regularly read those studies, and they were garbage before this moment. And I didn't use them to make decisions.

And all of a sudden I'm in this moment, and now management's taking an interest and saying, "Oh, not only are you going to do this thing, we're going to prove it's a good idea using these productivity studies." Now, we can actually observe what happens in the real world. And so we have this 2025 report that says with a 30 to 40 billion-dollar investment in GenAI, we have a surprising — to some — result that 95% of organizations are getting zero return.

Now, if you read these studies, I have yet to find one that looks at that and concludes maybe this is a bad idea. Right? They all look at this and say, "Well, the 95% of companies are doing it wrong. Read on to the end of our report, which — we are selling something which will help you do it right, and join the 5%."

We also have evidence from communities that are not being coerced by management: in open source. And so there are multiple sites that are now maintaining catalogs of slop-free software, which are open source projects that reject the use of AI either partly or entirely in the development process. It might not surprise you that Clojure is on that list.

Now, there's a lot of reasons there to be worried, but there's always more rabbits to pull out of a hat. And the current rabbit is: we're going to solve these problems. Your non-technical people cannot effectively use this new technology, and your technical people also cannot use this new technology. So vendors are going to send you forward-deployed engineers, and they are going to either A, show you how to use the technology, or B, make sure that you're locked into a particular project forever.

So there are processes in the industry where some people try to take a stepwise approach to assessing technology. And so we could kind of ThoughtWorks technology-radar this. We could say, where should this be? And I'm going to say, by the way, that this is completely unfair of me, right? Because I'm talking, I have 40 minutes, and I am oversimplifying. We should have a different word for that because of the collision with our use of "simplicity," but I'm massively oversimplifying, and obviously there are a wide variety of possible uses.

And so it may be that certain uses of LLMs in the development process should be under "caution," and other uses should be under "assess." Some should be under "trial," and maybe some should be under "adopt." We could have a conversation about that, and we could put different very specific tasks — right, I am building a throwaway prototype where everything that I'm building is visual and can be immediately validated — is a different kind of task than most of whatever I do. So I don't really have a strong opinion about that.

But we could disagree about these things. But what has actually happened, as far as I can tell, is that we have a new setting in the radar, which is "bet the global economy." And, by the way, that's for everything. Right? Like, use this everywhere. And that's the way things are going to be.

Now, we could keep talking about how much or how well this is going to work. But I think you can get to the underlying problem by reading this book. So this is your first homework assignment. Brian Cantwell Smith, unfortunately, passed away a few years ago, wrote this book, The Promise of Artificial Intelligence, Reckoning and Judgment. And in this book, he characterizes first wave AI and the current wave of AI as _reckoning systems_. Right? They don't have agency, they don't have self, they're not grounded in the world, and they're not situated. And they make fancy as hell demos. And there's a quote in the forward of his book where he says, "I am terrified that we will rely on reckoning where judgment is needed."

And blind, uncritical adoption of these tools is absolutely already doing this. Right? We are relying on reckoning machines where judgment is needed.

Now, happily, there are some areas where the effects of AI are more clear than the crap studies we have about productivity. People are definitely getting dumber. Right? You can find study after study that shows that when people cede authority to a chatbot, their ability starts to degrade. This was a study that was done in China where they were following students who started to use AI to help out with their homework.

I don't think it takes a crystal ball to figure this out, but when students started using AI to help with their homework, their homework became perfect. Right? There's a different graph that shows homework just shooting up to basically perfect. And their ability to do any other thing — and admittedly none of these measures are great — but their ability to do any other thing, in particular they got worse at being tested on the material that they were doing the homework for, and they got worse on more like IQ, critical thinking type tests.

So, this is pretty dismal. But remember that we are down the rabbit hole of making the classic developer mistake of just immediately throwing ourselves in and trying to make it work. So I'm going to pull back from that a little bit and try to ask some better questions. When we say does it work, what is "it"? Why is "it"? And who's benefiting from it?

And the first thing to understand is that the "it" here is automation, not artificial intelligence. And once you understand that, there is a long history of industries being disrupted by automation, and what people who are imposing automation without the input from the people who it affects are doing, and how people resist that and fight back against it. And that's the literature that you should be looking to to understand this moment.

"Why" is because they think they can. 

Amazon drivers pee in bottles. They are so closely surveilled while they work, they don't have time to go to the bathroom, they pee in bottles. Vice ran a story about this, and after a politician said something about it, Amazon escalated by saying it wasn't true. And then Vice ran a story where they went back and got actual photos, which is the photo on the left. But then, not being satisfied with that, Vice decided to test Amazon's product quality control by releasing human urine as a product. So the picture on the right is Vice's caffeine-free release, which is made of water and urea. And they actually got that up the recommendation chart on Amazon briefly.

What is the difference between you, in this room, a programmer, and an Amazon driver? Nothing. The reason that your employers, in general, as an industry — I'm not calling out individual people, right? — the reason that the industry is nice to you is only because it thinks it has to be. And we know exactly how they will treat people that they don't have to be nice to.

And, lucky for us, they overstepped a little bit. They hit the starting gun on laying people off before this AI shit was really working. And so we're in a moment now where we have a chance to claw some things back.

And of course, I mean, it's probably obvious by now, but who benefits is the ultra-rich. The time frame on this is about the time frame of my life, from the 1960s to today. When I was born, CEOs made 30 times what employees made. Now they make 300 times what employees make. You could ask yourself, did the C-suite get 10 times better while nothing else changed, or did employees get 10 times worse, or maybe there's some other explanation for this.

Meanwhile, labor's share of income is at an all-time low. Now, let me apologize, I'm grabbing all of these graphs off the internet. This one is not zero-based, which was what I would always do. So this only shows the share of income from, what, '52 to '68. But the trend is down. And so again, you have to ask yourself, like, is the vast majority of society shirking, or is there something else going on here?

And Cory Doctorow does a pretty good job of explaining this by saying that Big Tech has been rewriting laws to protect monopolies — to protect monopsonies, if you want to use fancy words — and to write IP law in such a way that it's weaponized for incumbents and against anybody else getting in.

And we had a moment with the inauguration of an American president last year that I'm going to optimistically call peak regulatory capture. So we have this moment where government and the most powerful tech bros are clearly buddies, and we know from history how they will behave. We can watch, you know, power corrupts, you can come up with — there's a million reasons to think about this. And I think you can actually make this critique from the left or the right. And I'm actually trying to stay neutral on that in this talk anyway.

But the question I would ask is, they've done all this IP shenanigans to transfer wealth from everybody to the super rich. What is it they want from AI? And it's quite clear. Right? We had the G20 meeting in Chapel Hill, North Carolina just a few weeks ago, where David Sacks — who apparently thinks this is the picture of himself he would like to have on Wikipedia — said that AI is too dynamic and fast-moving to be regulated.

All right. So we have some problems.

When I have problems I like to take a deep breath and take a walk. This is my version of a hammock, and look at a tree. I can't go for a walk and look at the tree right now cuz it's in Chapel Hill, but I brought the tree here so that we could all look at it together and just take a breath.

There is a lot that you can do about these problems. And I'm going to say "these problems" and cast kind of a bubbly umbrella there, because there are a lot of different ways to see this as a problem or as a set of problems, and we won't all agree precisely on what that is. But there are a lot of things that you can walk out of here today and do differently that will help to some degree.

So, one thing you can do when you're communicating at work is insist on AI disclosure.

I think this should be uncontroversial. It's already happening in science. It's already happening in medicine. I grabbed this one off the web. It's a CDC one. And whenever they're communicating, you start with: What content am I making? What action did I take? What AI tool did I use? What was my purpose in using it? And what was the human oversight of the result?

This is going to be an evolving space, and depending on what you're doing, it'll be different things. But it starts with it needs to be something. And so, I didn't actually do that at the beginning. So, here's my AI disclosure. I, Stuart Holloway, created this talk. I avoided all intentional use of AI. I created and revised the content myself. I disabled AI summaries in my search engine, and I read the reference materials directly, not AI summaries.

I am not, by the way, saying that everything has to be done that way. The entire point of these disclosures is there will be variation. There will be moments where you have specific purposes. So, that's fine. But it's important. And this is a stepping stone to an even better idea, which is: establish that AI slop is rude.

[applause]

Text used to be proof of thought. That's probably a little too optimistic. Text used to be proof of effort. Right? If I sent you 5,000 words, I made 5,000 words.

And the LLMs have completely upended this. You can send me an arbitrary amount of words, and then you can ask me to engage with that. And I have seen this happen in workplaces, right? Where people— I mean, obviously you see it in pull requests, right? You get a pull request that's— you used to get these little pull requests, and now you get this thing that's like, "I changed every file in the project," and, you know, no explanation why you did it.

The nice thing about establishing rudeness is it's a different level of argument. Like, if you make it something unfashionable, if you make it gross, right? I considered calling this "make AI slop gross always," but that acronym was already taken by another group.

[laughter]

But this is super important, and this is something— and I will say, it's difficult to go in a different direction when you don't see people around you at your company doing it.

I did this when I was working for somebody else. I did the things that I'm talking about here. And, you know, I was in a fairly privileged position. So, you know, it's different if you're on the bottom rung of the ladder at your company. But I assure you that there are other people thinking the same things you're thinking, and they're not saying it.

And so, you know, find the safe way to get into this conversation.

Also, it's important to continue to point out that productivity studies are still crap. Right? When you are working on projects, anchor success in the customer, if you're building for customers. Do not anchor success with tokens burned or number of days a week that an AI tool was used or whatever. All the normal Socratic technique applies here. Hey, what problem are you trying to solve? How are we measuring that we succeeded in this problem? You can do this all very politely. You don't have to be, you know, a rage monster about it. You can just have the conversation with people and say, "Okay, is this actually solving our problem?"

It's important to deplatform, deplatformize the big LLM vendors, right? And this is already happening because people don't want to pay the cost. But this should be an incredibly easy argument to have with your boss, right? "Oh, so you're telling me we should send all of our IP to a giant rapacious company that has a history of stealing IP and repurposing it for its own ends? Maybe we could run a local LLM and get approximately the same results."

It's important to keep the friction in how we develop skills. And this is really a piece of good news fairly recently. New York City announced a one-year ban on forklifts and jetpacks in elementary school physical education classes. And you can imagine the response, right? The forklift industry came back with a press release where they said, "You realize that these kids are going to get out of elementary school lacking necessary forklift skills to enter the modern economy."

Okay, so that's not actually what happened. Actually what happened is New York City and other areas as well announced a one-year ban on generative AI in elementary school. It's a good start. And this is something that you can advocate for. Obviously, most of you probably don't control school districts, and I promised things that you could do individually, but this is also something you can advocate for in your own self, and you can advocate for in your children.

I despair at the thought of my kids using LLMs as part of their learning process. But then these are areas where we can make concrete steps. And again, socializing this stuff makes a difference to people around you who may be thinking the same thing and are not speaking up.

If you're going to automate, automate for quality! Back to Cory Doctorow again, he bifurcates automation and says, when capital drives automation, it's usually about more stuff. And when labor drives automation, it's usually about making higher quality. Well, we don't have to agree with that, but we can say, "Look, he's playing Rich's simplification game." Right? He's taken automation and split it into two. If you look at actual workflows in your business, you can split it into a thousand.

And you can look at it and say, "You know what? These jobs really require care and judgment, and we shouldn't automate _here_. And there's this thing over here that, yeah, this is crap, you know, this is low value and easily validated." Right? There are judgments that you can make and decide what to automate. But when you're automating, automate for quality. We have a quality problem in the software industry. We had that before this moment.

If nothing else, LLMs are a tech debt force multiplier like we have never seen. Right? So, we're going to see an absolute explosion of tech debt that we have to deal with.

Now, collective action. Moving on from individual things you can do. These are bigger asks. And so, you know, you don't have to run out and try to do all of these this afternoon. You have until next week. I'll be checking in.

Group peer pressure at all scales. And I have seen this in companies. I've seen, you know, people ask questions, and then their peers jump in and ask questions. And in companies that have, you know, open forums where they feel safe talking, people jump in and apply pressure and say, "Okay, maybe this is not such a good idea."

I think we're in a starker moment than that, and I think that the right time for us to have unionized as an industry was when we didn't think we need to. But the other right time is yesterday.

And obviously, be safe about it, study up and learn. Big tech spends over a billion dollars a year union busting. And that money goes after you if you decide that you want to go down that road. So, be careful.

Advocate for politics and policy, and advocate for enforcing existing rules. Right? I mean, one of the things to realize here is as soon as you're not saying artificial intelligence equals magic, then you're saying automation equals things that we have traditional understanding of, and traditional rules might apply.

And so, then you can start to say we need to hold companies accountable. This is the risk triangle from the European Union regulation. And this is about promulgating new AI-specific accountability. And this is one I didn't actually read. I'm not saying that that's out there. But the thing that I would say is we don't need special legislation to hold companies accountable. If your company hacks another company and steals and damages stuff, you should be civilly and criminally liable, period.

And the people that should be civilly and criminally liable are in the C-suite, not developers that they hold out as puppets to be sacrificed when these things happen. And these are things that we have to argue for.

But all of this is kind of specific detail. The important thing is to stay on topic and keep in mind at all times: LLMs are automation. They're directed by people for political and economic goals. They are not intelligent. They have no agency, and they take no responsibility.

Which puts us in position, if you hold this slide in your mind, to start engaging with another question that the rage-driven media is trying to get us to think about right now, which is: will AI kill us all?

Well, based on this talk, you know, the first fix we'll make to this question is it's not AI, it's automation. Will automation kill us all? But that's really only a step, because AI doesn't have agency, but automation also does not have agency, right? Guns don't kill people, people kill people. So a decent question here is: will oligarchs and automation schemes kill us all?

I think this is a decent question. It's worth worrying about, especially because, you know, I don't know if you believe that power corrupts and absolute power corrupts absolutely, but I feel like the billionaire class in the United States has gotten more and more bizarre in their public statements as they've gotten wealthier and wealthier. We have a smaller and smaller group of people seemingly less and less connected to reality.

And now, with this question in mind, I'm going to ask what I think is actually a really interesting question, and this is my only slide that's actually about artificial intelligence.

Are we responsible enough to handle artificial intelligence? We are nowhere near it right now, but we are flunking the hell out of the pretest.

Right? If this is the way that we're reacting to language models, I dread the moment that something powerful actually comes along. We've clearly demonstrated — and this is a big problem, right, we need to look at the tree again in a minute — because, I mean, this is — we're nowhere near having the kind of political and social will to do the right thing, and we're proving it right now. We're nowhere close. Okay.

Now, everybody wants to go out and have fun tonight and go to the afterparty. So, I will end with a bunch of good news.

We are at a Clojure conference, and you are probably going to use LLMs at work. And there are broadly speaking at least two circles on the Venn diagram there. I have been commanded by my boss to use AI for everything. Right? Some people are in that boat right now. Or: I'm a practitioner who's figuring out for myself some places to use it without being completely reckless about it.

I have advice for you, regardless of which of those camps you're in, about how to responsibly use these tools. And it's all good news from here. So, the first thing is that everything you care about in the Clojure community still matters.

Right? Programming languages still matter. The stability of Clojure over time hugely matters. Immutability still matters. Quality libraries still matter. Community still matters. Right? The fact that we're coming together still matters.

Hard problems are still hard, and are interesting to work on, and LLMs are not doing a lot for that at the moment, at least. And friction still builds expertise.

Knowing these things, you have a competitive advantage. Because a lot of people don't know these things, but secondly, they're about to be poisoned by interacting with chatbots.

Programming remains a human activity. So, Tony Hoare said that the quality of software is measured by a number of totally incompatible criteria. How do you balance those criteria? That is a metis job, not a techne job. Right? You cannot write a complete instruction manual for what a good programmer does when they're balancing criteria to achieve a quality program.

And it's unfortunate that we don't actually have the English word "metical" based on "metis" to go along with "technical" based on "techne." Because that would cover this, right? Programming is both a technical and a "metical" activity. And if you're an ancient Greek snob wondering why I'm not pronouncing it "metice," it's because "meetical" would sound really terrible.

Critical thinking still dominates. We have, in my kids' lifetime, experienced three neck punches to critical thinking. What happened with COVID and schools, what has been happening with social media and attention hacking for profit, and now the cognitive surrender people are making to AI.

If you subject yourself slightly less to this than those around you, you will have a competitive edge.

So, the tree makes me feel better. I am incredibly happy and honored to be with this group of people and to be able to speak to you today. And I think that as the Clojure community, you are uniquely well positioned to weather the craziness that we're in right now.

And so, it is challenging, but you're starting from the right place, and I hope some of the things that I said today maybe are novel. I hope some of them will stimulate conversation.

Thank you very much for coming to the Conj, and I will see you at the afterparty.
