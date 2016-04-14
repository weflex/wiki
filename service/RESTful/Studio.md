- [STATUS OF THIS DOCUMENT](#sec-1)
- [EMBEDDABLES](#sec-2)
  - [Amenities Setting in Venue](#sec-2-1)
  - [Class Spots in Class Template](#sec-2-2)
  - [Lifetime of a ClassPackage](#sec-2-3)
- [MODELS](#sec-3)
  - [Organization](#sec-3-1)
  - [Venue](#sec-3-2)
  - [Collaborator](#sec-3-3)
    - [How roles works with `Collaborator`](#sec-3-3-1)
  - [Class](#sec-3-4)
  - [Class Template](#sec-3-5)
  - [Class Package](#sec-3-6)
  - [Member in venue and organization](#sec-3-7)
  - [Membership](#sec-3-8)
  - [Order](#sec-3-9)
  - [Order Log](#sec-3-10)
  - [Payment](#sec-3-11)
  - [Installation](#sec-3-12)
  - [Notification](#sec-3-13)
  - [External Resource](#sec-3-14)
- [LICENSE](#sec-4)


# STATUS OF THIS DOCUMENT<a id="sec-1" name="sec-1"></a>

This document is a Draft version.

# EMBEDDABLES<a id="sec-2" name="sec-2"></a>

## Amenities Setting in Venue<a id="sec-2-1" name="sec-2-1"></a>

An `AmenityMap` is an inline object defined in 5s. It is
constructed as a pair of the following fields.

| attr         | type    | default | required | security  |
| ------------ | ------- | ------- | -------- | --------- |
| changingRoom | Boolean | false   | false    | rw-rw-r-- |
| parking      | Boolean | false   | false    | rw-rw-r-- |
| shower       | Boolean | false   | false    | rw-rw-r-- |
| toilet       | Boolean | false   | false    | rw-rw-r-- |
| wifi         | Boolean | false   | false    | rw-rw-r-- |

## Class Spots in Class Template<a id="sec-2-2" name="sec-2-2"></a>

A `ClassSpots` shows how much space is left for the class. It is
modeled as a [fraction](https://en.wikipedia.org/wiki/Fraction_(mathematics)) number, 
with AVAILABLE as its numerator and TOTAL as its denominator.

| attr      | type   | default | required | security  |
| --------- | ------ | ------- | -------- | --------- |
| available | Number | -       | true     | rw-rw-r-- |
| total     | Number | -       | true     | rw-rw-r-- |

## Lifetime of a ClassPackage<a id="sec-2-3" name="sec-2-3"></a>

| attr  | type                 | default | required | security  |
| ----- | -------------------- | ------- | -------- | --------- |
| value | Number               | 0       | false    | rw-rw-r-- |
| scale | Enum(year,month,day) | day     | false    | rw-rw-r-- |

# MODELS<a id="sec-3" name="sec-3"></a>

## Organization<a id="sec-3-1" name="sec-3-1"></a>

An `Org` is a management structure for 5s. It represents a studio
organization with NAME, managed by ADMINISTRATOR and has at least one
`Venue` location.

| attr    | type                  | default | required | security  |
| ------- | --------------------- | ------- | -------- | --------- |
| name    | String                | -       | true     | rw-rw-r-- |
| logo    | ExternalResource      | -       | false    | rw-rw-r-- |
| banner  | ExternalResource      | -       | false    | rw-rw-r-- |
| venues  | HasMany(Venue)        | -       | false    | rw-rw-r-- |
| members | HasMany(Collaborator) | -       | false    | rw-rw-r-- |

## Venue<a id="sec-3-2" name="sec-3-2"></a>

A `Venue` is a studio location where 7es were given and
9s were sold.

To define its access control, a `Venue` must be specified with an ORG
and an ADMINISTRATOR. It also requires a venue NAME and ADDRESS, which
converts to LOC and MAPID. Other fields available includes a text
DESCRIPTION, a PHONE number, a list of AMENITIES it provides and a
list of URIs of PHOTOS.

NOTE: in this document, a `Venue` may also refered as a `studio`.

| attr          | type                  | default | required | security  |
| ------------- | --------------------- | ------- | -------- | --------- |
| org           | Org                   | -       | true     | rw-rw-r-- |
| name          | String                | -       | true     | rw-rw-r-- |
| address       | String                | -       | true     | rw-rw-r-- |
| location      | Location              | -       | true     | rw-rw-r-- |
| mapId         | String                | -       | true     | rw-rw-r-- |
| description   | String                | -       | false    | rw-rw-r-- |
| phone         | String                | -       | false    | rw-rw-r-- |
| amenities     | AmenityMap            | -       | false    | rw-rw-r-- |
| members       | HasMany(Member)       | -       | false    | rw-rw-r-- |
| collaborators | HasMany(Collaborator) | -       | false    | rw-rw-r-- |
| notifications | HasMany(Notification) | -       | false    | rw-rw-r-- |

## Collaborator<a id="sec-3-3" name="sec-3-3"></a>

A `Collaborator` is a structure for managing the relationships between
the `Org`, `Venue` and `WeflexUser`:

| attr        | type          | default | required | security  |
| ----------- | ------------- | ------- | -------- | --------- |
| org         | Org           | -       | true     | rw-rw---- |
| venue       | Venue         | -       | true     | rw-rw---- |
| user        | WeflexUser    | -       | true     | rw-rw---- |
| fullname    | Fullname      | -       | false    | rw-rw---- |
| languages   | Array(String) | -       | false    | rw-rw---- |
| roles       | Array(Role)   | -       | false    | rw-rw---- |
| description | String        | -       | false    | rw-rw---- |
| phone       | String        | -       | false    | rw-rw---- |

Every collaborator must be invited by an existed user like an organization `$admin`, the
invitation should contains the following fields:

-   `fullname` the full name of this collaborator.
-   `languages` the languages of this collaborator.
-   `description` the description of this collaborator.
-   `phone` the phone of this collaborator, the WeFlex system will send invitation via this phone number.

The `venue` is optional, because an organization might have no any venue, at that time 
these collaborators of the organization would not have any grant with a venue.

A collaborator without `venue` would have corresponding grants to all the venues under this organization.
This collaborator we call it organization collaborator.

A team owner to manage his or her collaborators also can specifically add a collaborator into a specific venue,
we call it venue collaborator. A venue collaborator will not have rights to see the any information of the
organization and other venues.

### How roles works with `Collaborator`<a id="sec-3-3-1" name="sec-3-3-1"></a>

There is a special role `trainer` which is used at creating 7 and 8 because
every fitness class requires one or more trainers there. Therefore the management system for members
should have the `roles` field for the following rules:

-   this field is a double-dimensional sparse array.
-   one of its elements should be in `$owner`, `$admin` or `null`.
-   another one of its elements should be `trainer` or `null`.
-   this array must have one element at least.

```picture
+--------------------+-------------+
| 0                  | 1           |
+--------------------+-------------+
| {'$owner'?'$admin' | {'trainer'} |
+--------------------+-------------+
```

## Class<a id="sec-3-4" name="sec-3-4"></a>

Publishes a `Class` instance. Any `Class`, from now on, must inherite
from a TEMPLATE, and then supplement it with a DATE, a FROM time, a TO
time and a SPOTS info.

TRAINER is not required. If it's set to `null`, it should fallbacks to
TRAINER of the TEMPLATE.

| attr     | type             | default | required | security  |
| -------- | ---------------- | ------- | -------- | --------- |
| template | ClassTemplate    | -       | true     | rw-rw-r-- |
| date     | Date             | -       | true     | rw-rw-r-- |
| from     | Time             | -       | true     | rw-rw-r-- |
| to       | Time             | -       | true     | rw-rw-r-- |
| spots    | ClassSpotsConfig | -       | true     | rw-rw-r-- |
| trainer  | Collaborator     | null    | false    | rw-rw-r-- |
| orders   | HasMany(Order)   | []      | false    | rw-rw---- |

## Class Template<a id="sec-3-5" name="sec-3-5"></a>

A `ClassTemplate` is a reusable template for publishing 7es of
NAME, at VENUE, teaching by TRAINER. PRICE and DESCRIPTIONS are also
required as they are key element of defining a `ClassTemplate`.

| attr        | type                    | default | required | security  |
| ----------- | ----------------------- | ------- | -------- | --------- |
| venue       | Venue                   | -       | true     | rw-rw-r-- |
| trainer     | Collaborator            | -       | false    | rw-rw-r-- |
| name        | String                  | -       | true     | rw-rw-r-- |
| price       | Number                  | -       | false    | rw-rw-r-- |
| photos      | Array(ExternalResource) | []      | false    | rw-rw-r-- |
| cover       | ExternalResource        | null    | false    | rw-rw-r-- |
| description | String                  | -       | true     | rw-rw-r-- |
| duration    | Number                  | 60      | false    | rw-rw-r-- |
| classes     | HasMany(Class)          | -       | false    | rw-rw-r-- |

## Class Package<a id="sec-3-6" name="sec-3-6"></a>

A `ClassPackage` is either a *multiple* or an *unlimited* package,
specified in ACCESSTYPE.

A *multiple* package is kind of packages that will valid for 7es
for multiple PASSES.

On the other hand, a *unlimited* package remains as valid for a given
LIFETIME, no matter how many times you attended.

Sometimes a *multiple* package also have a LIFETIME limitation. This
is mostly up to studio policies.

Sometimes clients may ask for an extension for their memberships. We
will reach this topic as soon as we are onto 11s. To
specify how many times at most a client could ask for extension on
this `ClassPackage`, set up EXTENSIBLE field.

This could be achieved via updating its LIFETIME, and this extension
could be done for as much as EXTENSIBLE times.

| attr        | type                     | default | required | security  |
| ----------- | ------------------------ | ------- | -------- | --------- |
| org         | Org                      | null    | false    | rw-rw---- |
| venue       | Venue                    | -       | true     | rw-rw---- |
| name        | String                   | -       | true     | rw-rw-r-- |
| category    | Enum(group,private)      | -       | true     | rw-rw-r-- |
| accessType  | Enum(multiple,unlimited) | -       | true     | rw-rw-r-- |
| extensible  | Number                   | -       | true     | rw-rw-r-- |
| description | String                   | -       | false    | rw-rw-r-- |
| passes      | Number                   | -       | false    | rw-rw-r-- |
| lifetime    | Lifetime                 | null    | false    | rw-rw-r-- |
| price       | Number                   | -       | true     | rw-rw-r-- |
| color       | String                   | -       | true     | rw-rw-r-- |
| memberships | HasMany(Membership)      | -       | false    | rw-rw-r-- |

TODO: Document about how to share a `ClassPackage` among a studio Org.

## Member in venue and organization<a id="sec-3-7" name="sec-3-7"></a>

The `Member` is the data that could be managed by venue and organization's owner
and admin.

| attr        | type                | default | required | security        |
| ----------- | ------------------- | ------- | -------- | --------------- |
| user        | WeflexUser          | -       | true     | rw-rw----       |
| venue       | Venue               | -       | true     | rw-rw----       |
| avatar      | ExternalResource    | -       | false    | rw-rw----       |
| nickname    | String              | -       | false    | rw-rw----       |
| phone       | String              | -       | false    | rw-rw----       |
| email       | String              | -       | false    | rw-rw----       |
| source      | String              | -       | false    | rw-rw----       |
| comment     | String              | -       | false    | rw-rw----       |
| createdAt   | Date                | now     | false    | r&#x2013;r----- |
| memberships | HasMany(Membership) | -       | false    | rw-rw----       |

## Membership<a id="sec-3-8" name="sec-3-8"></a>

A `Membership` is how an instance of `Member` subscribes to a studio's package
services.

| attr       | type             | default | required | security              |
| ---------- | ---------------- | ------- | -------- | --------------------- |
| package    | ClassPackage     | -       | true     | rw-rw-r--             |
| member     | Member           | -       | true     | rw-rw-r--             |
| correction | Object           | -       | false    | rw-rw----             |
| createdAt  | Date             | now     | false    | r&#x2013;r&#x2013;r-- |
| payments   | HasMany(Payment) | -       | false    | rw-rw----             |

`correction=s composed as Integers. For example, a =correction` object below 
would produce a `correction` of `-9`.

```js
{
  "value": 9,
  "positive": false
}
```

## Order<a id="sec-3-9" name="sec-3-9"></a>

| attr      | type              | default | required | security  |
| --------- | ----------------- | ------- | -------- | --------- |
| class     | Class             | -       | true     | rw-rw---- |
| user      | WeflexUser        | -       | true     | rw-rw---- |
| passcode  | String            | 123456  | false    | rw-rw---- |
| createdAt | Date              | now     | false    | rw-rw---- |
| payments  | HasMany(Payment)  | -       | true     | rw-rw---- |
| history   | HasMany(OrderLog) | -       | false    | rw-rw---- |

## Order Log<a id="sec-3-10" name="sec-3-10"></a>

An `Order Log` is a records collection, which has the following fields:

| attr      | type   | default | required | security  |
| --------- | ------ | ------- | -------- | --------- |
| order     | Order  | -       | true     | rw-rw---- |
| createdAt | Date   | now     | true     | rw-rw---- |
| status    | String | -       | true     | rw-rw---- |

This collection is for logging the changes of corresponding order.

## Payment<a id="sec-3-11" name="sec-3-11"></a>

| attr       | type       | default | required | security  |
| ---------- | ---------- | ------- | -------- | --------- |
| fee        | Number     | -       | true     | rw-rw---- |
| currency   | String     | -       | true     | rw-rw---- |
| method     | String     | -       | true     | rw-rw---- |
| order      | Order      | -       | true     | rw-rw---- |
| membership | Membership | -       | false    | rw-rw---- |
| createdAt  | Date       | now     | true     | rw-rw---- |
| \_raw      | Object     | null    | false    | rw-rw---- |

The `Payment` model stores data in the above structure, every object in this
table stands for a payment action with the following methods:

-   cash: if the `method` is a cash, the `membership` and `_raw` should not be
    set.
-   membership: in `membership` way, the `_raw` still should be an empty object
    or just a copy from the membership at that moment.
-   third-party payment service: like wechat, alipay or ping++, in this way,
    we should set `fee`, `currency` from the `_raw` object.

## Installation<a id="sec-3-12" name="sec-3-12"></a>

| attr          | type          | default | required | security  |
| ------------- | ------------- | ------- | -------- | --------- |
| id            | String        | -       | true     | rw-rw---- |
| appId         | String        | -       | true     | rw-rw---- |
| appVersion    | String        | -       | false    | rw-rw---- |
| badge         | Number        | 0       | false    | rw-rw---- |
| deviceToken   | String        | -       | false    | rw-rw---- |
| deviceType    | String        | -       | true     | rw-rw---- |
| status        | String        | -       | false    | rw-rw---- |
| subscriptions | Array(String) | -       | false    | rw-rw---- |
| timeZone      | String        | -       | false    | rw-rw---- |
| user          | WeflexUser    | -       | false    | rw-rw---- |
| createdAt     | Date          | now     | false    | rw-rw---- |
| modifiedAt    | Date          | now     | false    | rw-rw---- |

## Notification<a id="sec-3-13" name="sec-3-13"></a>

| attr               | type              | default | required | security  |
| ------------------ | ----------------- | ------- | -------- | --------- |
| status             | Enum(read,unread) | -       | true     | rw-rw---- |
| category           | String            | -       | true     | rw-rw---- |
| alert              | String            | -       | false    | rw-rw---- |
| badge              | String            | -       | false    | rw-rw---- |
| sound              | Number            | -       | false    | rw-rw---- |
| delayWhileIdle     | Boolean           | -       | false    | rw-rw---- |
| deviceToken        | String            | -       | false    | rw-rw---- |
| expirationInterval | Number            | -       | false    | rw-rw---- |
| expirationTime     | Date              | -       | false    | rw-rw---- |
| scheduledTime      | Date              | -       | false    | rw-rw---- |
| venue              | Venue             | -       | false    | rw-rw---- |
| createdAt          | Date              | now     | false    | rw-rw---- |
| modifiedAt         | Date              | now     | false    | rw-rw---- |

## External Resource<a id="sec-3-14" name="sec-3-14"></a>

| attr  | type        | default | required | security  |
| ----- | ----------- | ------- | -------- | --------- |
| type  | Enum(image) | image   | false    | rw-rw-r-- |
| uri   | String      | -       | true     | rw-rw-r-- |
| user  | WeflexUser  | -       | true     | rw-rw-r-- |
| venue | Venue       | -       | false    | rw-rw-r-- |

# LICENSE<a id="sec-4" name="sec-4"></a>

WeFlex Copyright
