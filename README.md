# ember-data-notes

So, I don't understand Ember Data and I'm pissed. I'm tired of just not knowing so I'm going to learn out loud.

Resources I've found or purchased to learn more about Ember Data:
- [Pro Ember Data: Getting Ember Data to Work with Your API
Tang, David](https://www.amazon.com/Pro-Ember-Data-Getting-Work/dp/1484265602)
- [Ember Data: A Comprehensive Tutorial for the ember-data Library](https://www.toptal.com/emberjs/a-thorough-guide-to-ember-data)
- [Ember.js tutorial for beginners #10 Ember data, store, adapter, serializer (2020)](https://www.youtube.com/watch?v=Le0ifGiNyq4)
- [Ember Data Repository](https://github.com/emberjs/data)
- [Ember Data Guides](https://guides.emberjs.com/release/models/)

<ins>Initial questions</ins>
1. What is Ember Data?
2. Why should we use it?
3. How does it look?
4. How to use it?
5. How to fetch data from a Rails API?
6. How to make a small API and practice using Ember Data?
7. What's ember-concurrency? Does that have anything to do with Ember data?
8. What are the most efficient ways of using Ember Data?
9. What are some gotchas or things I should watch out for?
10. How can I get Ember Data wrong?
11. What ways should I not use Ember Data?
12. Who are some folks who know Ember Data well?
13. How to contribute to Ember Data as an EXTREME newbie?
14. Who hates Ember Data? What are they saying?
15. Who loves Ember Data? What are they saying?
16. Can I explain Ember Data to my mom?
17. What are some real world examples of using Ember Data in the wild?
18. What are a few best practices we should all live by while using Ember Data?

Let's start with Ember Data's README. I hate reading btw but here we go 😩. I will include my "dumb" thoughts and questions as well.

## Ember Data README https://github.com/emberjs/data

### What is Ember Data?


<strong>Ember data is a data persistence library for Ember.js.</strong>



`Stop right there!? What is the purpose of a data persistent library? Why is that being called out? What makes that important?`

`According to the book [JavaScript Cookbook](https://www.oreilly.com/library/view/javascript-cookbook/9781449390211/ch20.html), users` `expect to be able to close their browser window, reopen it, and see information they've saved prior. For example, shopping cart items` `should remain in the users shopping cart even if they've closed the browser. It is important to have a library the ensures this` `presistent state.`


_Okay, let's continue reading._

`ember-data` is designed to be agnostic to the underlying persistence mechanism, so it works just as well with `JSON API` over `HTTP` as it does with streaming `WebSockets` or local `IndexedDB` storage.


`What's WebSockets? I know about local storage but what is local IndexedDB storage?`

`According to [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API), The <strong>WebSocket API</strong> is an` `advanced technology that makes it possible to open a two-way interactive communication session between the user's browser and a` `server. With this API, you can send messages to a server and receive event-driven responses without having to poll the server for a` `reply.`

`<strong>IndexedDB</strong> is a low-level API for client-side storage of significant amounts of structured data, including` `files/blobs. This API uses indexes to enable high-performance searches of this data. While Web Storage is useful for storing smaller` `amounts of data, it is less useful for storing larger amounts of structured data. IndexedDB provides a solution.`


Okay, let's continue.

It provides many of the facilities you'd find in server-side `ORM`s like `ActiveRecord`, but is designed specifically for the unique environment of `JavaScript` in the browser.

`Cool, I kinda remember ActiveRecord but what does `ORM` stand for?`

`Object Relational Mapping (ORM) is the technique of accessing a relational database using an object-oriented programming language.` `Object Relational Mapping is a way for our Javascript programs to manage database data by "mapping" database tables to classes and` `instances of classes to rows in those tables.`

Great article about `ORM`s to read later - https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/


### Installation

`ember-data` is installed by default for new applications generated with `ember-cli`.

If you wish to add `ember-data` to an `addon` or `application`, you can do so by running
the following command, which will use `yarn` or `npm` to install `ember-data` as a `devDependency`.

```no-highlight
ember install ember-data
```

Similarly, if you have generated a new `Ember` application using `ember-cli` but do 
not wish to use `ember-data`, remove `ember-data` from your `package.json`.

## Ember Data Guides https://guides.emberjs.com/release/models/

