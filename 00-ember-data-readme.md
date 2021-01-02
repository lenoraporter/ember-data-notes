# Learnings from Ember Data ReadMe

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

`Cool, I kinda remember ActiveRecord but what does ORM stand for?`

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
