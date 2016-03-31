WeFlex Developers 101
====

This document provides the most fundamental knowledge about WeFlex Developers. We wrote this document as we hope this document could help you build an image of who we are, what we do and how we do it, so that you could better working with us.

To begin with, you can browse our [organization Github](https://github.com/weflex). As you may notice:

1. Most of our projects are based on Node.js;
2. We have a lot of private repos and even more public repos.

Node.js is the major technology we use to build our apps. It might not be perfect but it evolves and improves so rapidly, as tens of thousands of developers contributing to its ecosystem everyday. There's no reason that with such an active community, this technology couldn't thrive.

Here's a list of technology stack we have been using so far (and still using):

- _Javascript_ is the most common programming language;
- _React_ is our most-used client rendering library;
- _Loopback_ is server framework;
- _MongoDB_ for persist data storage;
- _Docker_ containers as service runtime;
- Other *nix tools such as Make, BASH, etc;
- _Markdown_ for normal documentation;
- _Org_ for structural documents that for human to read and for machine to execute;


Some Important Private Projects
----

- [weflex/gateway](https://github.com/weflex/gateway)

Gateway is our server program. This server provides data and transactional services in JSON format.

Plain CRUD (Create, Read, Update and Destroy) operations on models are implemented in RESTful style and model schematics defined under `common/models/`.

Beside RESTful APIs, there's also transactional APIs that handles complex transactions such as notification, payment and passcode authentication. These APIs are called middlewares are declared under `server/middleware.json`.


- [weflex/gian](https://github.com/weflex/gian)

RESTful API is great as it abstracts Database manipulation into HTTP Requests/Responses. On the other hand, it's still too low-level thus couldn't be used as ideal building-blocks for clients. In order to help client programmers to be more focused on business logics and thus be more efficient, we built Gian. Gian is a client wrapper that builds a connection between HTTP Requests and normal Javascript Object manipulations. See [README file](https://github.com/weflex/gian/blob/master/README.md) for its documentation.


One More Thing
----

Before you leave and proceed to other part of this wiki, to checkout our Expert List:

| Name         | Github      | Field of Experty                                     |
| ------------ | ----------- | ---------------------------------------------------- |
| Scott Wang   | @scottoasis | Deployment, Docker, Some Old Linux Stuffs, OrgMode   |
| Yorkie Liu   | @yorkie     | Node.js, Loopback, Redis, React, Github              |
| Dai Xi       | @Xidai      | WeFlex Product, AngularJS, Loopback                  |
| Fancy Pan    | @omohiko    | WeFlex Product, Prototyping, Information Architecture|

So if you hit any problem while reading this wiki or doing your work, feel free to ask the right person questions on Github or on Slack.
