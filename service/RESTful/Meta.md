- [STATUS OF THIS DOCUMENT](#sec-1)
- [BUILTINS](#sec-2)
  - [Number](#sec-2-1)
  - [String](#sec-2-2)
  - [Boolean](#sec-2-3)
  - [Object](#sec-2-4)
  - [Date](#sec-2-5)
- [METATYPES](#sec-3)
  - [Array](#sec-3-1)
  - [Enum](#sec-3-2)
  - [Range](#sec-3-3)
- [EMBEDDABLES](#sec-4)
  - [Time](#sec-4-1)
  - [Fullname](#sec-4-2)
  - [Location](#sec-4-3)
- [DEFAULTS](#sec-5)
- [MODELS](#sec-6)
- [LICENSE](#sec-7)


# STATUS OF THIS DOCUMENT<a id="sec-1" name="sec-1"></a>

This document is a `Draft` version.

# BUILTINS<a id="sec-2" name="sec-2"></a>

## Number<a id="sec-2-1" name="sec-2-1"></a>

See [Number on Mozila](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)

## String<a id="sec-2-2" name="sec-2-2"></a>

See [String on Mozila](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

## Boolean<a id="sec-2-3" name="sec-2-3"></a>

See [Boolean on Mozila](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

## Object<a id="sec-2-4" name="sec-2-4"></a>

See [Object on Mozila](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)

## Date<a id="sec-2-5" name="sec-2-5"></a>

See [Date on Mozila](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

# METATYPES<a id="sec-3" name="sec-3"></a>

## Array<a id="sec-3-1" name="sec-3-1"></a>

An `Array` is for presenting a collection which contains values of a specific type or `any`.

## Enum<a id="sec-3-2" name="sec-3-2"></a>

An `Enum` are specific type values with limited options, in this document we denote
them as the following:

```pancaked
Enum(1, 10)
```

## Range<a id="sec-3-3" name="sec-3-3"></a>

A `Range` are numeric values with upper and lower limits. Here in this
document, we denote them as:

```pancaked
Range(min, max)
```

# EMBEDDABLES<a id="sec-4" name="sec-4"></a>

## Time<a id="sec-4-1" name="sec-4-1"></a>

`Time` is an inline type as the following definition:

| attr   | type   | default | required | security  |
| ------ | ------ | ------- | -------- | --------- |
| minute | Number | -       | true     | rw-rw-r-- |
| hour   | Number | -       | true     | rw-rw-r-- |

## Fullname<a id="sec-4-2" name="sec-4-2"></a>

`Fullname` is an inline type as the following definition:

| attr  | type   | default | required | security  |
| ----- | ------ | ------- | -------- | --------- |
| first | String | -       | true     | rw-rw-r-- |
| last  | String | -       | true     | rw-rw-r-- |

## Location<a id="sec-4-3" name="sec-4-3"></a>

A `Location` is an inline object defined in s. It is
constructed as a pair of LATITUDE and LONGTITUDE.

| attr       | type   | default | required | security  |
| ---------- | ------ | ------- | -------- | --------- |
| latitude   | String | -       | true     | rw-rw-r-- |
| longtitude | String | -       | true     | rw-rw-r-- |

# DEFAULTS<a id="sec-5" name="sec-5"></a>

If an attribute is not `required` (and thus `optional`), it would be
quite natural to have a default value, and these default values are at
most cases the same (per type).

Types also has default values. At most cases, default value of an
attribute fallbacks to a default value of its type. In all the table
below, we are using an `-` notion to represent such a case.

Here are default value of types we used in this document.

| type   | default |
| ------ | ------- |
| Array  | []      |
| Date   | now     |
| Number | 0       |
| String | -       |

**NOTE**: If an attribute is required, then it doesn't make sense to
provide a default value for it.

# MODELS<a id="sec-6" name="sec-6"></a>

There is no pre-defined or meta models in this document.

# LICENSE<a id="sec-7" name="sec-7"></a>

WeFlex Copyright
