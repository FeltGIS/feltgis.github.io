---
layout: post
title: Hybrid mobile development using Go/GoLang
subtitle: 
cover-img: /assets/img/forest-07.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [go, golang, mobile]
---

This is a story about a small tech startup using GoLang for their cross platform development. 

# Backdrop

My background is a mobile developer consultant, mostly on iOS, building apps for both enterprise and end users. So many times, I have had a discussion with my peer Android developer, talking about how a particular issue has been solved. The two apps should be more or less equal, and we need to avoid duplicating any mistakes. But, since most apps are just a UI-layer on top of some endpoint, it is usulally not that painful to build the lower layers of the app twice. 

At FeltGIS, though, our UI layer is quite simple, and we do have a lot of stuff going on under the hood. In addition to Android and iOS, we have software running in the cloud and on mini-computers. Since our system is quite distributed and disconnected, a lot of the same code is running across the various platforms. I do have some background from C++ programming, but I could not convince myself to go down that path. Time to look into what the hipsters are using, these days;)

# Rust, Go or Kotlin Native?

Screening the alternatives, three stood out as good match for our project: Rust, Go and Kotlin Native. Rust has performance, security, although it seems to take a bit more effort to become productive. Somebody wrote “In Rust, you use more time to build efficient stuff that will run fast and safe forever” and “in Go, you just write stuff that you might throw away because, you never know, you know”. Kotlin was compelling because it is widely adopted both on Android and in Java/backend stacks. I have to admit, though, that, coming from Swift, Kotlin, just.. Feels a bit weird (to get used to, I guess). 

The next idea was to field test the three.  I sat a junior developer to set up “hello world” apps, one for iOS and one for Android. It should basically just perform a network call, persist the result to some database, and display the result to some native UI-component on each platform. 

After a week or so he came back with the following:

|               | Android     | iOS         |
| ------------- | ----------- | ----------- |
| Kotlin Native | Very simple | Hard        |
| Go            | Very simple | Very simple |
| Rust          | Fair        | Simple      |

Now, a year has gone by, and I am sure the result would not be the same today. But the “scores” were based on how hard it was for a junior developer (with me as a sidekick) to get the hello world app up and running, including setting up the builds, iron out the snippets required to to the job. It might very well be that Kotlin Native on iOS is actually straight forward, but as either of us had too much experience with the platform, we just kept getting stuck. Our questions to Google was probably just poorly formulated, but we felt a bit stuck on an island for a few days. 

# Here we Go!

So we went for Go. I spent a day or so on [A Tour of Go](https://tour.golang.org/welcome/1), to get familiar with the language, and sat up GoLand (from JetBrains). If you are not familiar with Go, I´d say it feels like something they would use to introduce students to programming. Very few keywords to get lost in, and very clear syntax. It totally lacks generics, so coming from Swift you´ll feel quite lost for a while. No fancy functional-ish chaining 