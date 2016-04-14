# gian.js

The fat client-side library for next-generation WeFlex apps, inspired by "胖虎" in DORAEMON

## Usage

```js
var gian = require('@weflex/gian');
var client = new gian.GianClient('dev');
```

## Installation

```sh
$ npm install @weflex/gian --save
```

## Test

```sh
$ npm test
```

## API Documentation

- [Constructor](#constructor)
  - [`.getClient(env, options)`](#getclientenv-options)
- [Context](#context)
  - [`.request(endpoint, method, data)`](#requestendpoint-method-data)
  - [`.requestMiddleware(url, data)`](#requestmiddlewareurl-data)
  - [`.requestView(name, where, sortBy, limit, skip)`](#requestviewname-where-sortby-limit-skip)
  - [`.restify(model, properties)`](#restifymodel-properties)
- [RESTFul](#restful)
  - [`.list(filter)`](#listfilter)
  - [`.create(resouce)`](#createresouce)
  - [`.get(id)`](#getid)
  - [`.update(id, patches)`](#updateid-patches)
  - [`.upsert(resouce)`](#upsertresouce)
- [Organization](#organization)
- [Organization Member](#organization-member)
- [User](#user)
  - [`.login(username, password)`](#loginusername-password)
  - [`.logout()`](#logout)
  - [`.pending(onloggedIn)`](#pendingonloggedin)
  - [`.getCurrent()`](#getcurrent)
  - [`.getVenueById(id)`](#getvenuebyidid)
  - [`.setVenueById(id)`](#setvenuebyidid)
  - [`.smsRequest()`](#smsrequest)
  - [`.smsLogin()`](#smslogin)
  - [`.smsRegisterNewOrgAndVenue()`](#smsregisterneworgandvenue)
- [Venue](#venue)
- [Class](#class)
- [Class Template](#classtemplate)
- [Class Package](#classpackage)
- [Order](#order)
- [Payment](#payment)
- [ExternalResource](#externalresource)
- [Membership](#membership)
- [Installation](#installation)
- [Notification](#notification)

### Constructor

To initialize a `GianClient`:

```js
import { GianClient } from '@weflex/gian';
const client = new GianClient('dev'); // for dev
```

The arguments of `constructor`:

| property | value |
|----------|-------|
| env      | string in "dev", "staging" and "production" |
| options  | options |
| options.storage | default: localStorage |
| options.onAuthFail | a function fired when authorization failed |

To get the gateway info:

| property | description |
|----------|-------------|
|`client.gateway.api`|API gateway URI|
|`client.gateway.app`|APP gateway URI|
|`client.gateway.ws` |WebSocket gateway URI|

Or you can intialize a client with your customized gateway config:

```
const client = new GianClient({
  api: 'http://192.168.1.102',
  app: 'http://192.168.1.101',
  ws: 'ws://192.168.1.102'
});
```

Note: this is just for developing and debugging, don't use it in production.

### `.getClient(env, options)`

This library provides the cache way to get instance as well:

```js
const client = gian.getClient('dev', options);
```

### Context

Every new {GianClient} instance has a {Context} object, which does:

- parse gateway out from given `env` from `gateway.json`.
- manage the life cycle of session/localStorage.

> Note: this context shouldn't be directly accessed in user-land.

#### `.request(endpoint, method, data)`

Make an origin request with given parameters:

- `endpoint` {String} the url like "/api/users".
- `method` {String} method string like "get", "post".
- `data` {Object}

#### `.requestMiddleware(url, data)`

Make an origin request to get response with the following possible parameters:

- `url` {String} the path of middleware.
- `data` {Object} the object post to api server

#### `.requestView(name, where, sortBy, limit, skip)`

Make an origin request to get views with the following possible parameters:

- `name` {String} the name of view
- `where` {Object} the where object on this view
- `sortBy` {Object} the sortBy object on this view
- `limit` {Number} the limit on this view
- `skip` {Number} the skip on this view

#### `.restify(model, properties)`

Returns a {RESTFul} object and also extend by the 2nd argument `properties`.

- `model` {String} the model plural name to join in request url, like "users", "venues".
- `properties` {Object} the object will be attached to the returned object.

### RESTFul

Every {RESTFul} object is only returned and extended by the function [Context.prototype.restify]().

#### `.list(filter)`

List resources by the object `filter` and return a {Promise} object with results.

- `filter` {Object} The filter object, optional.

#### `.create(resouce)`

Create a resource with `resouce` and return a {Promise} with creating status.

- {Resource<T>} resouce - the resouce data that you want to create.

#### `.get(id)`

Get the {Resource<T>} object by `id` and return a {Promise} with `res.body`.

#### `.update(id, patches)`

Update the specified resouce by `id` and `patches`, it returns a {Promise} with updated resource/resouce.

- `id` {String} The id to find the resouce to be updated.
- `patches` {Object} Same to the type {Resource<T>}, but `required` rules.

#### `.upsert(resouce)`

Upsert a resouce.

- `resouce` {Resource<T>} The resouce to be upsert-ed.

#### `.remove(id)`

Remove a resouce instance by specified `id`.

- `id` {String} The resouce id.

### Organization

| property  | value                           |
|-----------|---------------------------------|
| base      | [`Context.RESTFul`](#restful)   |
| namespace | `GianClient.prototype.rest.org` |

### Organization Member

| property  | value                                 |
|-----------|---------------------------------------|
| base      | [`Context.RESTFul`](#restful)         |
| namespace | `GianClient.prototype.rest.orgMember` |

### User

| property  | value                            |
|-----------|----------------------------------|
| base      | [`Context.RESTFul`](#restful)    |
| namespace | `GianClient.prototype.rest.user` |

#### `.login(username, password)`

Login with `username` and `password`.

- `username` {String} The username string
- `password` {String} The password string

Example:

```js
try {
  await client.rest.user.login(username, password);
} catch (err) {
  alert(err);
}
```

#### `.logout()`

Logout

#### `.pending(onrenderCode, onloggedIn)`

Create a web socket connection to [passport-server](), and get notified on `onloggedIn` when
remote login gets done.

- `onrenderCode` {Function} The trigger when get random code from server.
- `onloggedIn` {Function} The callback when remote login gets done.

This returns a {Promise} with the generated id by [passport-server]() and composite the authorization
url.

```jsx
class LoginPage extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      url: null
    };
  }
  componentDidMount() {
    client.user.pending(
      this.onrenderCode.bind(this),
      this.onlogged.bind(this)
    );
  }
  onrenderCode(code) {
    this.setState({
      url: 'http://api.theweflex.com/auth/wechat?state=' + btoa('qrcode:' + code)
    });
  }
  onlogged() {
    alert('login successfully');
  }
  render() {
    return <QRCode value={this.state.url} size={128} />;
  }
}
```

#### `.getCurrent()`

Get the profile of logged user.

#### `.getVenueById(id)`

Get venue by id, if the `id` is omit, returns selected venue.

#### `.setVenueById(id)`

Set selected venue id, if the `id` is omit, will set from venues' first element.

#### `.smsRequest(phone)`

Make a request to send a message to given phone, this will require the following service:

- resque-workers:send

Before testing this functionality, make sure your redis and resque-workers are up correctly.

#### `.smsLogin(phone, smscode)`

Login with phone and what you got code from SMS:

```js
try {
  const payload = await user.smsLogin(phone, this.smscodeInput.value);
  // payload.userId
  // payload.passcode
} catch (err) {
  // handle errors from gateway or networks
}
```

You are able to save userId and passcode secretly for later sessions.

#### `.smsRegisterNewOrgAndVenue(phone, smscode, options)`

Register an organization and venue with a smscode, calling this function correctly will auto
login as well.

### Venue

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.venue` |

### Class

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.class` |

### Class Template

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.classTemplate` |

### Class Package

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.classPackage` |

### Order

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.order` |

### Payment

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.payment` |

### Coin

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.coin` |

#### `.batch(data, count)`

Batch `count` coins with `data`.

- `data` {Coin} the coin to be created
- `count` {Number} the number to create

```js
client.rest.coins.batch({
  owner: 'WeFlex',
  //...
}, 100);
```

### External Resource

| property  | value                                     |
|-----------|-------------------------------------------|
| base      | [`Context.RESTFul`](#restful)             |
| namespace | `GianClient.prototype.externalResource`   |

### Trainer

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.trainer` |

### Membership

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.membership` |

### Installation

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.installation` |

Register an installation namely device:

```js
client.create({
  appId: 'your appid',
  userId: 'your userId',
  deviceToken: 'your device token'
})
```

To show the complete model, visit [pancake:weflex/installation](https://github.com/weflex/pancake/blob/master/src/weflex/installation.json)

### Notification

| property  | value |
|-----------|-------|
| base      | [`Context.RESTFul`](#restful) |
| namespace | `GianClient.prototype.notification` |

## CHANGELOG

See [CHANGELOG.md](CHANGELOG.md)
