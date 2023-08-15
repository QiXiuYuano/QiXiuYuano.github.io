---
title: "Making a Static Blog Dynamic: Websockets and Morphdom with a Go Server"
date: 2022-12-10T10:23:47-07:00
draft: false
tags: ["go", "websockets", "morphdom", "hugo", "fly.io", "cockroachdb"]
summary: Server side rendering is becoming more popular and has a number of advantages over alternatively heavy client frameworks.
---

## Inspiration and Introduction

Server side rendering is becoming more popular and has a number of advantages over alternatively heavy client frameworks.
One trend that has allowed server rendered content to be dynamic without serving a lot of Javascript uses a persistent websocket connection to synchronize state.
The server generates pieces of html that it sends back to the client which then is only responsible for patching the DOM with simple html.
A popular Javascript library to do this patching is [morphdom](https://github.com/patrick-steele-idem/morphdom) that aims to be lightweight and fast.

The following are frameworks that use this approach and handle a lot more complexity than will be mentioned here.
Check out their sites and documentation for more.

* [Phoenix LiveView](https://github.com/phoenixframework/phoenix_live_view)
* [Hotwire](https://hotwired.dev/)
* [Laravel Livewire](https://laravel-livewire.com/)

Static site generators such as Hugo make it easy to create and serve static web content that is easily cached and delivered cheaply.
In this post we will explore using a homegrown approach inspired by these frameworks to add dynamic content to this blog using Go and a little Javascript.

{{< lead >}}
Code for the blog is found at [github.com/sdehm/sdehm-blog](https://github.com/sdehm/sdehm-blog/tree/v0.1.0) while code for this dynamic content server is found at [github.com/sdehm/sdehm-blog-dynamic](https://github.com/sdehm/sdehm-blog-dynamic/tree/v0.1.0).
{{< /lead >}}

## Architecture Overview

The solution explored here includes a little bit of Javascript and CSS added to the static site and a single webserver written in Go.
The client side Javascript is responsible for opening the websocket connection, listening for user interaction, and patching the DOM when messages from the server are received.
The server will keep track of connections, listen for user interaction messages from the client, send messages with HTML updates to clients, and will also persist data as needed.

The stack for this implementation includes the following libraries and service providers.

* Server side websockets in Go with the [gobwas](https://github.com/gobwas/ws) websocket library
* Frontend DOM patching with [morphdom](https://github.com/patrick-steele-idem/morphdom)
* Data persisted in [CockroachDB](https://www.cockroachlabs.com/)
* Dynamic content server hosted on [Fly.io](https://fly.io/)
* The blog is a [Hugo](https://gohugo.io/) static site on GitHub Pages with the [Blowfish](https://github.com/nunocoracao/blowfish) theme

## Implementation

The dynamic components created by this implementation are a live view count and a comment section.

The live view count shows how many active connections there are on a given post.
This is shown in the top of each post page and shown per post on the [home](https://sdehm.dev) page or [posts](https://sdehm.dev/posts/) page.
The native [Blowfish](https://github.com/nunocoracao/blowfish) view count component is modified to show the live view count instead of the total view count.
{{< figure src="img/post_list.webp" title="Post List View Count" caption="Screenshot showing the live view count icon which indicates how many viewers are currently connected." >}}

The comments section shows a new comment form and a list of all comments for a given post with the newest shown at the top.
As users add comments they are updated live for anyone viewing the page.
{{< figure src="img/comments.webp" title="Post Comments" caption="Screenshot showing the comment section with a new comment form and a list of all comments the post." >}}

You can see these features on this post and others on this site.

### Client Side

The custom Javascript added to the static content is found [here](https://github.com/sdehm/sdehm-blog/blob/v0.1.0/assets/js/custom.js).
The custom CSS is found [here](https://github.com/sdehm/sdehm-blog/blob/v0.1.0/assets/css/custom.css).

#### `custom.js`

The socket's `onmessage` function set here will first parse the json sent by the server and will switch on the type.

The `connected` event sets the connection id that the server will use to identify the connection.
This message also includes the HTML content for the all of the comments for this particular post which is patched with morphdom.
Additionally an event listener is started to handle comment form submissions to be explained later.

{{< lead >}}
The Blowfish Hugo template allows creating a custom comment template which is used to create an empty placeholder div with an id of `comments` so it can be found later.
{{< /lead >}}

When the event handler receives a `morph` type event it updates the element with a matching id with server provided HTML.
Morphdom will handle diffing the changes and making the minimal updates required.
This will be used to update the live view count.

Similarly, when the message is a `prepend` type the DOM is updated with the new HTML however this time it is prepended to the element with a matching id rather than replaced with morphdom.
This allows new comments to show up at the top of the comment list.

```js
socket.onmessage = (event) => {
  const data = JSON.parse(event.data);

  switch (data.type) {
    case "connected":
      connectionId = data.connection_id;
      console.log("Connection ID: " + connectionId);
      morphdom(document.getElementById("comments"), data.html);
      document
        .getElementById("comment-form")
        .addEventListener("submit", handleCommentSubmit);
      break;
    case "morph":
      morphdom(document.getElementById(data.id), data.html);
      break;
    case "prepend":
      template = document.createElement("template");
      template.innerHTML = data.html;
      document.getElementById(data.id).prepend(template.content);
      break;
  }
};
```



## Conclusion

This approach to dynamic content on a static site allows for a lot of flexibility and is enjoyable to work with.
Having a persistent websocket connection opens up a lot of possibilities for real time dynamic content yet it remains fairly lightweight.
The native features of the Go language make it easy to implement a simple websocket server that can handle many connections.

As a potential downside, websockets are pretty well established but there is still the potential of clients who do not support them or have reasons to disable them.
If a user is not online then they will only receive the static content which makes it important to make sure that the static content can deliver appropriate value by itself.

## Areas of Improvement and Next Steps

A notable area of improvement is the lack of tests in this example.
It would be interesting to see how the architecture and design changes when tests are added.

A complete list of improvements and next steps includes:

* Tests
* Defined contract for messages and code generation for client and server code.
* Better error handling
* Observability
* Markdown comments
* Cached data store
* GitHub Actions deployments
* Advanced websockets features such as compression
* Non-websocket fallback for more robust support