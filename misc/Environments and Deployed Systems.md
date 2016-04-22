# Current Environments and Systems

At this moment, we have two system up and running in our deployment environments: the Legacy Wechat System (the "Legacy System") and the Studio System.

We are going to merge Legacy System into the Studio System no later than end of June, 2016, but till this moment, they're still two separate systems running on their own right.

## The Legacy System

Since the Legacy System is deprecated and we're no longer modify and release it as often as the Studio System, it's only up and running in production.

| Server | Address           | Repository   |
| ------ | ----------------- | ------------ |
| API    | api.theweflex.com | weflex/api   |
| Client | www.theweflex.com | weflex/wechat|


## The Studio System

Compared to the Legacy System, the Studio System is a more active and unstable system. Thus we have it both in production and in staging.

### Production Environment

| Server | Address            | Repository           |
| ------ | ------------------ | -------------------- |
| API    | api.getweflex.com  | weflex/gateway       |
| Client | demo.getweflex.com | weflex/studio-desktop|

### Staging Environment

| Server | Address           | Repository     |
| ------ | ----------------- | -------------- |
| API    | dev.getweflex.com | weflex/gateway |
