---
layout: post
title: Hybrid mobile development using Go/GoLang
subtitle: 
cover-img: /assets/img/forest-07.jpg
thumbnail-img: /assets/img/2go.png
share-img: /assets/img/2go.png
tags: [go, golang, mobile]
---

This is a story about a small tech startup using Go for their cross platform (mobile) development. 

# Backdrop

My background is a mobile developer consultant, mostly on iOS, building apps for both in-house/enterprise and end users. Many times, I have had a discussion with my peer Android developer, talking about how a particular issue has been solved. The two apps should be more or less equal, and we need to avoid duplicating any mistakes. But, since most apps are just a UI-layer on top of some endpoint, it is usually not that painful to build the lower layers of the app twice. 

Example scenarios

* Offline: The app needs to talk to some endpoint, and, to some degree, it must persist some stuff locally on the device. Perhaps the app needs to have some sort of offline support. 
* Model massage: The endpoints does not match the mobile user interface. Typically hard to get stuff changed on the server, so the mobile apps have to perform data model *fetching*, *zipping* and *massaging* to create some some sort of sensible view models.
* Logic: Perhaps the app has some user input validation. If the user enters *this*, check server for *that*, make sure something validates *here* and notify services *there*.  

Most "native" mobile developers I know are consultants, and they will happily build all of this in parallell with their peer Android/iOS contultants, it makse total sen$e.  

At FeltGIS, though, our UI layer is quite simple, but we do have a lot of stuff going on under the hood. -Btw, FeltGIS is a small Norwegian tech startup. We are building tools to digitize and improve forestry and mobile/offline/field workflows. 

In addition to Android and iOS, we have software running in the cloud and on mini-computers. Since our system is quite distributed and disconnected, a lot of the same code is running across the various platforms. My first job actually did involve a lot of C++, and if had not been a source of [evilness](https://sites.google.com/site/dustingetz/dev/cpp-is-evil), it would be a perfecly viable option.. Also, as a startup, we have to conisder cost efficiency. Every "native mobile" developer knows that cross platform development is not really cheaper (it´s just management believing so, isn´t it) - but in our case, it kind of made a lot of sense. Our core development team should share as much code as possible, while me might rely on hired consultants for more peripheral stuff (integrations, custom UIs, etc).

Time to look into what the hipsters are using, these days;)

# Rust, Go or Kotlin Native?

Screening the alternatives, three stood out as the most interesting match for our project: Rust, Go and Kotlin Native. Rust has performance, security, although it seems to take a bit more effort to become productive. Somebody wrote “In Rust, you use more time to build efficient stuff that will run fast and safe forever” and “in Go, you just write stuff that you might throw away because, you never know, you know”. Kotlin was compelling because it is widely adopted both on Android and in Java/backend stacks. I have to admit, though, that, coming from Swift, Kotlin, just.. Feels a bit weird (to get used to, I guess). 

The next idea was to field test the three.  I sat a junior developer to set up “hello world” apps, one for iOS and one for Android. It should basically just perform a network call, persist the result to some database, and display the result to some native UI-component on each platform. 

After a week or so he came back with the following:

|               | Android     | iOS         |
| ------------- | ----------- | ----------- |
| Kotlin Native | Very simple | Hard        |
| Go            | Very simple | Very simple |
| Rust          | Fair        | Simple      |

Now, a year has gone by, and I am sure the result would not be the same today. But the “scores” were based on how hard it was for a junior developer (with me as a sidekick) to get the hello world app up and running, including setting up the builds, iron out the snippets required to to the job. It might very well be that Kotlin Native on iOS is actually straight forward, but as either of us had too much experience with the platform, we just kept getting stuck. Our questions to Google/SO was probably just poorly formulated, but we felt a bit stuck on an island for a few days. 

I love Swift and I´m sure I would fall in love with Rust ([and its performance](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/go-rust.html)) as well, but just doing some random reading about the two, Go just seemed like a more fun party to attend.. Not to forget, [GoMobile](https://pkg.go.dev/golang.org/x/mobile/cmd/gomobile), a tool to help integration your Go-stuff into mobile apps, made the intitial steps very simple. 

![](/assets/img/2go.png)

# Here we Go!

So we went for Go. I spent a day or so on [A Tour of Go](https://tour.golang.org/welcome/1), to get familiar with the language, and sat up GoLand (from JetBrains), and wrote some very simple build scripts to update the library on the Android/iOS projects. 

If you are not familiar with Go, I´d say it feels like something they would use to introduce students to programming. Very few keywords to get lost in, and very clear syntax. Easy to focus on what really matters, to solve what´s at hand, and trying to do so as neat as possible. It totally lacks generics, so coming from Swift you´ll feel quite lost for a while. No fancy functional-ish chaining of operations, no generics - very little extra sugar to the language. 

What was most intriguing to me, is how cheap a (execution-) thread is (they are called Goroutines in Go, and is not related to CPU-threads). Since they are super light weight and basically free, you don´t have to leave your thread while waiting for stuff, you just ..wait ([CSP-style concurrency](https://en.wikipedia.org/wiki/Communicating_sequential_processes)). This has a profound effect on code readability and ease of debugging. Now Go will soon support generics, I´ll admit I am looking forward to it. Also, it includes some powerful primitives to handle communication between Go-routines (channels) and a rich standard library. 

Building is fast. Cross compiling is awesome. Unit testing is easy and motivating. Hunting memory leeks is fun. Also, interfacing with C is super-easy, if you ever need to work a bit closer to the iron. 

# One year later

Even though it only took a day to learn the basics to start fiddling with real stuff in Go, it does take a while to get used to and we are still learning and improving ourselves. The code base has evolved and is now a farily solid repo with multiple build targets in various environment flavours. 

Go is compelling in many ways for cross platform (mobile) development, and some highlights for us 

* Easy to get going from scratch
* "Nobody knows it" (learning together is fun)
* Easy to learn (not too many complex concepts to wrap our heads around)
* Common ground (the simplicity makes Go feel neutral)

Now I realize this post is pretty opinionated and optimistic about the outcome. Unfortunately, I don´t have access to the other parallell universes where the we went for all the other stacks, but I´m pretty sure they are all writing an equivalent "success story" as seen from their perspective. 

For mobile development, having two code bases may alerady be challenging enough, so why go for three? -Why not just use Flutter or Xamarin? Well, you´re all right, great tools, and making taking these kind of decisions are hard and should be aligned with the project and product´s requirements, team profile, other tech stacks, etc. 

