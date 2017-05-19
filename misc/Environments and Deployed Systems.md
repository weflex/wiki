# Current Environments and Systems

At this moment, we have two system up and running in our deployment environments: the Legacy Wechat System (the "Legacy System") and the Studio System.

~~We are going to merge Legacy System into the Studio System no later than end of June, 2016, but till this moment, they're still two separate systems running on their own right.~~

## The Legacy System

Since the Legacy System is deprecated and we're no longer modify and release it as often as the Studio System, it's only up and running in production.

| App    | Address           | Repository   |
| ------ | ----------------- | ------------ |
| API    | api.theweflex.com | weflex/api   |
| WeChat | www.theweflex.com | weflex/wechat|


## The Studio System

Compared to the Legacy System, the Studio System is a more active and unstable system. Thus we have it both in production and in staging.

### Production Environment

| App     | Address               | Repository           |
| ------- | --------------------- | -------------------- |
| Gateway | api.getweflex.com     | weflex/gateway       |
| Studio  | studio.theweflex.com  | weflex/studio-web    |
| Booking | booking.theweflex.com | weflex/booking-web   |

### Staging Environment

| App     | Address           | Repository     |
| ------- | ----------------- | -------------- |
| Gateway | dev.theweflex.com | weflex/gateway |

Staging 的 Studio 和 Booking 也部署在 dev.theweflex.com 上，但是需要链接 DNS 才能访问。

### Docker Registry

| App      | Address              |
| -------- | -------------------- |
| Registry | docker.theweflex.com |

