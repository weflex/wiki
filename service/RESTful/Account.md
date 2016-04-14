- [STATUS OF THIS DOCUMENT](#sec-1)
- [OVERVIEW](#sec-2)
  - [Role and Roles](#sec-2-1)
- [MODELS](#sec-3)
  - [Users](#sec-3-1)
  - [Working with third-party service](#sec-3-2)
  - [Working with access token](#sec-3-3)
  - [Weflex UserCredential](#sec-3-4)
- [LICENSE](#sec-4)


# STATUS OF THIS DOCUMENT<a id="sec-1" name="sec-1"></a>

This document is a Draft version.

# OVERVIEW<a id="sec-2" name="sec-2"></a>

To explain how ACL rules are for each attributions, We humbly borrowed
file permission mode from \_nix file system.

Each file permission mode is made up of 9 bits in total, with each 3
bits describes access rights (read, write and execute) for
*Administrative Users* it belongs to, its owner and other users.

```ditaa
read  write  execute      access rights
   ^    ^    ^
   |__  |  __|
      \ | /
       rw-rw-r--
       ---===+++
  admin_/  |  \_others   user types
         owner
```

So a security mode like `rw-rw-r--` means this file could be read by
all users, but only its administrative users and its owners could
modify it, and no one could execute it. For a more detailed definition
of who those admins/owners are for specific models, see 1.

When we apply this schematic to object properties, it's quite obvious
how read and write would work. ~~As for execution, it would be quite a
complex story. Here I would rather leave it as reserved.~~

To better explain who admin/owner/other are for each model, we have
made a rule table like below.

A call of `$root()` evaluates to *System Level Administrator*
controlled by WeFlex.

A `$` evaluates to a instance of model specified in the left-most
column.

A `$admin()` function call evaluates to content of `Admin` column of
the content. Similarly, `$owner()` evaluates to its `Owner`
counterpart.

A `&` combines values of its left and right sides into a flat array.

| MODEL \\ ROLE | Admin                             | Owner                                      |
| ------------- | --------------------------------- | ------------------------------------------ |
| Org           | `$root()`                         | `$.administrator`                          |
| Venue         | `$admin($.org) & $owner($.org)`   | `$.administrator`                          |
| ClassTemplate | `$admin($.venue) & $owner($.org)` | `$owner($.venue)`                          |
| Class         | `$admin($.template)`              | `$owner($.template)`                       |
| ClassPackage  | `$admin($.org) & $admin($.venue)` | `$owner($.venue) & $owner($.org)`          |
| Membership    | `$admin($.classPackage)`          | `$owner($.classPackage) & $.user`          |
| Trainer       | `$admin($.org) & $admin($.venue)` | `$owner($.org) & $owner($.venue) & $.user` |

## Role and Roles<a id="sec-2-1" name="sec-2-1"></a>

We did borrow our `Role` model from Loopback's built-in `Role` model 
as the following definition:

| attr        | type   | default | required | security  |
| ----------- | ------ | ------- | -------- | --------- |
| id          | String | -       | true     | rw-rw-r-- |
| name        | String | -       | true     | rw-rw-r-- |
| description | String | -       | false    | rw-rw-r-- |

And the pre-defined roles for every organization is defined as the below:

| name    | description               |
| ------- | ------------------------- |
| $owner  | the owner of org or venue |
| $admin  | the admin of org or venue |
| trainer | the trainer               |

# MODELS<a id="sec-3" name="sec-3"></a>

## Users<a id="sec-3-1" name="sec-3-1"></a>

A `WeflexUser` is a user of this system with the following informations:

| attr          | type                          | default | required | unique | security  |
| ------------- | ----------------------------- | ------- | -------- | ------ | --------- |
| nickname      | String                        | -       | false    | -      | rw-rw-r-- |
| sex           | Enum(female, male)            | -       | false    | -      | rw-rw-r-- |
| province      | String                        | -       | false    | -      | rw-rw-r-- |
| city          | String                        | -       | false    | -      | rw-rw-r-- |
| country       | String                        | -       | false    | -      | rw-rw-r-- |
| avatar        | ExternalResource              | -       | false    | -      | rw-rw-r-- |
| phone         | String                        | -       | false    | true   | rw-rw-r-- |
| collaborators | HasMany(Collaborator)         | -       | false    | -      | rw-rw---- |
| identities    | HasMany(WeflexUserIdentity)   | -       | false    | -      | rw-rw---- |
| credentials   | HasMany(WeflexUserCredential) | -       | false    | -      | rw-rw---- |
| accessTokens  | HasMany(WeflexAccessToken)    | -       | false    | -      | rw-rw---- |

## Working with third-party service<a id="sec-3-2" name="sec-3-2"></a>

| attr | type       | default | required | security  |
| ---- | ---------- | ------- | -------- | --------- |
| user | WeflexUser | -       | false    | rw-rw---- |

## Working with access token<a id="sec-3-3" name="sec-3-3"></a>

| attr | type       | default | required | security  |
| ---- | ---------- | ------- | -------- | --------- |
| user | WeflexUser | -       | false    | rw-rw---- |

## Weflex UserCredential<a id="sec-3-4" name="sec-3-4"></a>

| attr | type       | default | required | security  |
| ---- | ---------- | ------- | -------- | --------- |
| user | WeflexUser | -       | false    | rw-rw---- |

# LICENSE<a id="sec-4" name="sec-4"></a>

WeFlex Copyright
