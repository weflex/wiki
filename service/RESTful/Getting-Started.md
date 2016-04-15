# 如何使用 RESTful 接口

WeFlex 使用 [strongloop/loopback] 作为 RESTful 接口的生成器，因此完整的文档可以参考：

- [https://docs.strongloop.com/display/public/LB/Where+filter](https://docs.strongloop.com/display/public/LB/Where+filter)

这里为了帮助非 WeFlex 成员或者是对于 loopback 并不熟悉的人快速入门，这里将会做一个简单的描述。

### 概述

WeFlex 资源服务器是由各种不同的资源组成，例如`WeflexUser`和`Class`，所有的资源都定义在了 [service/RESTful] 下，
这里不做复述。每一类资源都有各自的名字、属性和方法，名字和属性都是在 [service/RESTful] 中可见，方法却是固定的。

### 资源的方法

在请求所有资源时，都需要通过登陆接口获取的`access_token`的值加在请求的`query`中，如下：

```
GET /classes?access_token=your_access_token
```

并且要在请求头（Headers）中设置`Content-type`为`application/json`，如果是 POST/PUT 请求的话，
还需要确认 Body 的内容为 JSON 格式，而不是`foo=bar&sam=jam`这样的 FORM 格式。

#### find

描述：查询满足条件的实例列表。HTTP请求格式如下：

| 名字       | 值域       |
|-----------|------------|
| method    | GET        |
| pathname  | /:resource |

##### 如何构建查询语句

在`find`和之后的`findOne`中，我们都需要通过特定的条件去决定返回的数据，以及其数据格式，这里我们借用了[strongloop/loopback]的`filter`
参数，该参数将添加至`query`中，如：

```sh
GET /classes?access_token=your_access_token&filter={"where":{"title":"studio"}}
```

`filter`参数接受一个`JSON`格式如下：

```js
{
  "where": {
    "field": "value"
  },
  "limit": 1, // 这里接受一个数字，表示返回的列表长度
  "skip":  1, // 这里接受一个数字，表示返回的列表开始位置
  "include": [] // 接受数组，当需要进行一些`JOIN`操作时使用，将在之后详细说明
}
```

除此之外，因为`JSON`并不是一个合法的`query`值，因此需要对`JSON`进行URI编码。

**使用include来查询关联数据集**

比如每一个`Class`（课程）实例都对应于一个`Order`的集合，你可以在模型定义文档中找到：

> | orders   | HasMany(Order)   | []      | false    | rw-rw---- |

这里我们可以通过如下的请求来获取课程的订单列表：

```sh
GET /classes?access_token=your_access_token&filter={"include":["orders"]}
```

将会在返回的每个课程实例中带上对象的`orders`，如：

```js
[
  {
    "title": "fly-yoga",
    "orders": [
      {
        "userId": "xxxx",  // 代表创建订单的用户
        "passcode": "111111"
      }
    ]
  }
]
```

由于订单中也有保存着`Order`与`User`的关系，因此如果想让每个订单中可以附带用户的信息：

```sh
GET /classes?access_token=your_access_token&filter={"include":[{"orders":["user"]}]}
```

看起来有些复杂，展开`filter`的话如下：

```json
{
  "include": [
    {
      "orders": ["user"]
    }
  ]
}
```

#### findOne

描述：获取某个特定的资源实例。

| 名字       | 值域           |
|-----------|----------------|
| method    | GET            |
| pathname  | /:resource/:id |

#### create

描述：创建一个或多个资源实例。

| 名字       | 值域       |
|-----------|------------|
| method    | POST       |
| pathname  | /:resource |

按照如下实例发送请求，并将实例的内容使用如下格式发送：

```
POST /classes?access_token=your_access_token
Content-Type: application/json

{
  "title": "mit studio",
  // 等
}
```

#### update

描述：更新指定的某一个资源实例。

| 名字       | 值域           |
|-----------|----------------|
| method    | put            |
| pathname  | /:resource/:id |

参照`create`方法

#### delete

描述：删除某个指定的资源实例。

| 名字       | 值域           |
|-----------|----------------|
| method    | delete         |
| pathname  | /:resource/:id |

# 版权

所有权归 WeFlex 所有

[service/RESTful]: ../RESTful
[strongloop/loopback]: https://github.com/strongloop/loopback
