# Getting Started

In this page, you will learn what the two main languages used in Status
are, how to learn them, how to build Status, & finally how to find tasks
to work on.

Status is largely written in two languages: Clojurescript & Golang and
builds on React Native for UI. I'll talk about the rational behind these
design decisions and link to offsite resources on learning each of these
technologies.

## React Native

Status has always aspired to have a single, unified codebase for
multi-platform development. This is not so easy to achieve, as many of
the non-web-based solutions out there allow you to have a semi-single
codebase, but you find yourself creating a lot of seperate logic for
handling user interfaces on each platform - Status is 80% frontend code
and this presents a problem. We tried Xamarin & Java (EthereumJ was the
first implementation we got running on Android & iOS - via RoboVM), and
we quickly discovered this problem.

Mobile apps built on web-based technologies, such as those done in
Cordova, are great for short-term projects or mvp, but you will quickly
run into performance issues on resource-limited devices and may need to
rewrite you app. In our tests displaying webview DApps in a chat history
with iFrames and all the other bells and whistles we wanted turned out
to be a futile effort.

That limited our options to choosing between NativeScript & React
Native. We chose React Native because it is more mature and is being
used in production for popular apps like Facebook, Instagram, Airbnb,
Baidu & Discord. This gave us the impression that this framework was
here to stay with many Fortune 500 companies invested in its
continuance.

At this point all we had to do is merge Material & iOS design into our
unique look'and'feel so that we could minimise the amount of Android &
iOS specific code. Our amazing designer [Andrei
Mironov](https://dribbble.com/andmironov) elegantly solved the rest of
that puzzle.

## React Native Learning Resources

If you would like to start learning React Native, check out these
resources: [Use React Native](http://www.reactnative.com/books/) [React
Native
ReadTheDocs](https://facebook.github.io/react-native/docs/getting-started.html)
you might want to continue reading and learn more about re-natal,
re-frame & reagent.

  - Re-natal

re-natal is an invaluable tool to that automates the setup of your
re-frame React Native for Android & iOS, it
includes[figwheel](https://github.com/decker405/figwheel-react-native)
for easy development.

[re-natal README](https://github.com/drapanjanas/re-natal)

  - Re-frame

re-frame is simple but expressive library for writing Single Page
Applications in ClojureScript, using Reagent. It is a functional
framework for reactive 'MVC-style' applications. To learn more:

[re-frame README](https://github.com/Day8/re-frame) [re-frame
docs](https://github.com/Day8/re-frame/tree/master/docs#introduction) [A
Noob's walkthrough of a re-frame
Application](http://www.multunus.com/blog/2016/02/noobs-walkthrough-re-frame-app/)
[The Angular Phonecat tutorial in
re-frame](https://dhruvp.github.io/2015/03/07/re-frame/)

  - Reagent

Reagent provides a minimalistic interface between ClojureScript and
React. It allows you to define efficient React components using nothing
but plain ClojureScript functions and data, that describe your UI using
a Hiccup-like syntax. The goal of Reagent is to make it possible to
define arbitrarily complex UIs using just a couple of basic concepts,
and to be fast enough by default that you rarely have to care about
performance.

[Reagent Page](http://reagent-project.github.io) [Reagent
README](https://github.com/reagent-project/reagent) [Building single
apps with
Reagent](https://yogthos.net/posts/2014-07-15-Building-Single-Page-Apps-with-Reagent.html)
[You should be using FigWheel and Reagent and heres
why](http://timothypratley.blogspot.com/2015/07/you-should-be-using-figwheelreagent.html)
[Reagent Rocks](http://www.mattgreer.org/articles/reagent-rocks/)

## Clojurescript

“Lisp is worth learning for the profound enlightenment experience you
will have when you finally get it; that experience will make you a
better programmer for the rest of your days, even if you never actually
use Lisp itself a lot.” — Eric Raymond. To read more quotes on Lisp read
[Lisp, made with secret alien technology.](http://lispers.org)