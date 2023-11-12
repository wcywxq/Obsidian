---
title: graphql
url: https://www.yuque.com/wcywxq/xxble8/cnpgks
---

graphql 是一种 api 查询语言。 <a name="75e71c39"></a>

## 什么是 graphql ?

> `graphql` 是一个用于 `API` 的查询语言，是一个使用基于类型系统来执行查询的服务端运行时的数据（类型系统由你的数据定义）。`graphql` 并没有和任何特定数据库或者存储引擎进行绑定，而是依靠现有的代码

<a name="1fa42afe"></a>

## 入门

> 一个 `graphql` 服务是通过定义类型和类型上的字段来创建的，然后给每个类型上的每个字段提供解析函数。例如，一个 `graphql` 服务告诉我们当前登陆用户是`me`，这个用户名称可能像这样：

```graphql
type Query {
    me: User
}

type User {
    id: ID
    name: String
}
```

> 一并的还有每个类型上字段的解析函数

```javascript
function Query_me(request) {
    return request.auth.user
}

function User_name() {
    return user.getName()
}
```

> 一旦一个 `graphql` 服务运行起来（通常在 `web` 服务的一个 `URL` 上），它就能接收 `graphql` 查询，并验证和执行。接收到的查询首先会被检查确保它只引用了已定义的类型和字段，然后运行执行的解析函数来生成结果。

```graphql
{
    me {
        name
    }
}
```

> 会产生这样的 `JSON` 结果：

```graphql
{
    "me": {
        "name": "Luke Skywalker"
    }
}
```

<a name="7170e01a"></a>

## 查询和变更

<a name="daeef0b1"></a>

### 字段 Fields

```graphql
{
  hero {
    name
  }
}
# 生成
{
  "data": {
    "hero": {
      "name": "R2-D2"
    }
  }
}
```

```graphql
{
  hero {
    name
    # 查询可以有备注！
    friends {
      name
    }
  }
}
# 生成
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

<a name="4a44f18e"></a>

### 参数 Arguements

```graphql
{
  human(id: "1000") {
    name
    height
  }
}
# 生成
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 1.72
    }
  }
}
```

```graphql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
# 生成
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448
    }
  }
}
```

<a name="ef617325"></a>

### 别名 Aliases

```graphql
{
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}
# 生成
{
  "data": {
    "empireHero": {
      "name": "Luke Skywalker"
    },
    "jediHero": {
      "name": "R2-D2"
    }
  }
}
```

<a name="ee346038"></a>

### 片段 Fragments

```graphql
{
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  appearsIn
  friends {
    name,
    id
  }
}

# 生成
{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Han Solo",
          "id": "1002"
        },
        {
          "name": "Leia Organa",
          "id": "1003"
        },
        {
          "name": "C-3PO",
          "id": "2000"
        },
        {
          "name": "R2-D2",
          "id": "2001"
        }
      ]
    },
    "rightComparison": {
      "name": "R2-D2",
      "appearsIn": [
        "NEWHOPE",
        "EMPIRE",
        "JEDI"
      ],
      "friends": [
        {
          "name": "Luke Skywalker",
          "id": "1000"
        },
        {
          "name": "Han Solo",
          "id": "1002"
        },
        {
          "name": "Leia Organa",
          "id": "1003"
        }
      ]
    }
  }
}
```

<a name="51ba75fd"></a>

#### 在片段内使用变量

```graphql
# $first是变量，更改数值之后生成数据会有变化
query HeroComparison($first: Int = 4) {
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}

# 生成
{
  "data": {
    "leftComparison": {
      "name": "Luke Skywalker",
      "friendsConnection": {
        "totalCount": 4,
        "edges": [
          {
            "node": {
              "name": "Han Solo"
            }
          },
          {
            "node": {
              "name": "Leia Organa"
            }
          },
          {
            "node": {
              "name": "C-3PO"
            }
          },
          {
            "node": {
              "name": "R2-D2"
            }
          }
        ]
      }
    },
    "rightComparison": {
      "name": "R2-D2",
      "friendsConnection": {
        "totalCount": 3,
        "edges": [
          {
            "node": {
              "name": "Luke Skywalker"
            }
          },
          {
            "node": {
              "name": "Han Solo"
            }
          },
          {
            "node": {
              "name": "Leia Organa"
            }
          }
        ]
      }
    }
  }
}
```

<a name="a31cc404"></a>

### 操作名称 Operation name

```graphql
# 示例
query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}
# 生成
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

- 操作类型
  - `query`
  - `mutation`
  - `subscription`
- 操作名称

> 有意义和明确的名称

<a name="1da8f319"></a>

### 变量 Variables

- 使用变量之前，我们得做三件事

1. 使用 `$variableName` 替代查询中的静态值。
2. 声明 `$variableName` 为查询接受的变量之一。
3. 将 `variableName: value` 通过传输专用（通常是 JSON）的分离的变量字典中。

全部做完之后就像这个样子：

```graphql
# { "graphiql": true, "variables": { "episode": JEDI } }
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

<a name="5aa3dffc"></a>

#### 变量定义 Variable definitions

> 1. 变量前缀必须为 `$`，后跟其类型
> 2. 所有声明的变量都必须是 *标量*、*枚举型* 或者 *输入对象类型*。
> 3. 变量定义可以是可选的或者必要的。上例中，`Episode` 后并没有 `!`，因此其是可选的。

<a name="a4a32b57"></a>

#### 默认变量 Default variables

> 可以通过在查询中的类型定义后面附带默认值的方式，将默认值赋给变量。

```graphql
query HeroNameAndFriends($episode: Episode = "JEDI") {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

<a name="8594c190"></a>

### 指令（`Directives`）

- `@include(if: Boolean)` 仅在参数为 `true` 时，包含此字段。
- `@skip(if: Boolean)` 如果参数为 `true`，跳过此字段。

```graphql
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}

# 传递参数, 修改 withFriends 字段的值会生成不同的结果
{
  "episode": "JEDI",
  "withFriends": true
}

# 生成
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

<a name="e07f6c14"></a>

### 变更（Mutations）

> `GraphQL` 的大部分讨论集中在数据获取，但是任何完整的数据平台也都需要一个改变服务端数据的方法。

> `REST` 中，任何请求都可能最后导致一些服务端副作用，但是约定上建议不要使用 `GET` 请求来修改数据。`GraphQL` 也是类似 —— 技术上而言，任何查询都可以被实现为导致数据写入。然而，建一个约定来规范任何导致写入的操作都应该显式通过变更（`mutation`）来发送。

> 就如同查询一样，如果任何变更字段返回一个对象类型，你也能请求其嵌套字段。获取一个对象变更后的新状态也是十分有用的。我们来看看一个变更例子：

```graphql
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}

# 变更的数据
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}

# 生成
{
  "data": {
    "createReview": {
      "stars": 5,
      "commentary": "This is a great movie!"
    }
  }
}
```

> 注意 `createReview` 字段如何返回了新建的 `review` 的 `stars` 和 `commentary` 字段。这在变更已有数据时特别有用，例如，当一个字段自增的时候，我们可以在一个请求中变更并查询这个字段的新值。

> 你也可能注意到，这个例子中，我们传递的 `review` 变量并非标量。它是一个\_输入对象类型\_，一种特殊的对象类型，可以作为参数传递。

<a name="687c460b"></a>

#### 变更中的多个字段（`Multiple fields in mutations`）

> 一个变更也能包含多个字段，一如查询。查询和变更之间名称之外的一个重要区别是：*查询字段时，是并行执行，而变更字段时，是线性执行，一个接着一个。*

> 这意味着如果我们一个请求中发送了两个 `incrementCredits` 变更，第一个保证在第二个之前执行，以确保我们不会出现竞态。

<a name="3b76c582"></a>

### 内联片段（`Inline Fragments`）

> 如果你查询的字段返回的是接口或者联合类型，那么你可能需要使用\_内联片段\_来取出下层具体类型的数据：

```graphql
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}

# 传递的变量
{
  "ep": "JEDI"
}

# 生成
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "primaryFunction": "Astromech"
    }
  }
}
```

> 这个查询中，`hero` 字段返回 `Character` 类型，取决于 `episode` 参数，其可能是 `Human` 或者 `Droid` 类型。在直接选择的情况下，你只能请求 `Character` 上存在的字段，譬如 `name`。

> 如果要请求具体类型上的字段，你需要使用一个类型条件**内联片段**。因为第一个片段标注为 `... on Droid`，`primaryFunction` 仅在 `hero` 返回的 `Character` 为 `Droid` 类型时才会执行。同理适用于 `Human` 类型的 `height` 字段。

> 具名片段也可以用于同样的情况，因为具名片段总是附带了一个类型。

<a name="8651d705"></a>

### 元字段（`Meta fields`）

> 某些情况下，你并不知道你将从 `GraphQL` 服务获得什么类型，这时候你就需要一些方法在客户端来决定如何处理这些数据。`GraphQL` 允许你在查询的任何位置请求 `__typename`，一个元字段，以获得那个位置的对象类型名称。

```graphql
{
  search(text: "an") {
    __typename
    ... on Human {
      name
    }
    ... on Droid {
      name
    }
    ... on Starship {
      name
    }
  }
}
# 生成
{
  "data": {
    "search": [
      {
        "__typename": "Human",
        "name": "Han Solo"
      },
      {
        "__typename": "Human",
        "name": "Leia Organa"
      },
      {
        "__typename": "Starship",
        "name": "TIE Advanced x1"
      }
    ]
  }
}
```

> 上面的查询中，`search` 返回了一个联合类型，其可能是三种选项之一。没有 `__typename` 字段的情况下，几乎不可能在客户端分辨开这三个不同的类型。

> `GraphQL` 服务提供了不少元字段，剩下的部分用于描述 [内省](https://graphql.cn/learn/introspection/) 系统
