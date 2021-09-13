---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
---

# Graphql + Primsa (+ Elixir)

Research Week

---

# Graphql

## What is Graphql

> GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data.

- ภาษาที่เอาไว้ query ข้อมูลทำหน้าที่เป็นตัวกลางในการจัดการข้อมูลต่างๆ เพื่อให้เราได้ข้อมูลที่มีประสิทธิภาพมากขึ้น

---

# Graphql vs REST Api

<img src="http://www.somkiat.cc/wp-content/uploads/2017/07/rest_graphql.png" alt="rest_grapql" />

- รูปจาก [http://www.somkiat.cc](http://www.somkiat.cc)

---

# Problem with REST

- End point เยอะมีความซับซ้อน
- Over-fetching
- Under-fetching

---

# Problem with REST (Cont.)

## End point

- /itemset/major
- /itemset/minor
- /itemset/patch

---

# Problem with REST (Cont.)

## Over-fetching

```jsx
export default () => {
  // response
  // { name: "ju", id: 1, email: "test@gmail.com", tel: "0800000000"}

  const userData = axios.get('/users')

  return (
    <div>
      <p>{userData.name}</p>
      <p><{userData.id}</p>
    </div>
  )
}
```

---

# Problem with REST (Cont.)

## Under-fetching

```jsx
export default () => {
  // response user
  // { name: "ju", id: 1, email: "test@gmail.com", tel: "0800000000"}

  // response idol
  // { name: 'iu', song: "bbibbi,fluu", user_id: 1 }

  const userData = axios.get('/users')
  const kPopData = axios.get('/idol/user/1')

  return (
    <div>
      <p>{userData.name}</p>
      <p><{user.id}</p>
      <p>{kPopData.name}</p>
    </div>
  )
}
```

---

# Graphql

## Pros.

- single endpoint -> /graphql
- ask for what you need, get exactly that
- agnostic client framework (react, vue, stencil, svelte, etc)
- agnostic server framework (nodejs, elixir, rust, go, apollo, etc)
- strongly typed
- awesome playground for dev process
- schema document
- so many tools 😒

## Example

[จิ้ม 😏](http:localhost:4000)

---

# Graphql

## Cons.

- so many tools for implement
- n + 1 problem ???

---

# In Deep With Graphql

## Schema

- **describe the shape of your available data**. The schema also specifies exactly which **queries** and **mutations** are available for clients to execute.

---

# Schema Definition Language (SDL)

## basic type

- String, Int, Float,
- Int
- Float
- Boolean,
- ID

```graphql
# schema.graphql

type Book {
  // field
  title: String! # not null
  author: Author
}

type Author {
  name: String;
  books: [Book]
}
```

---

# Schema Definition Language (SDL) (Cont.)

**Query** type

- special object type that defines all of the top-level entry points for queries that clients execute against your server

```graphql
# schema.graphql
.
.

type Query {
  books: [Book]
  authors: [Author]
}
```

---

# Schema Definition Language (SDL) (Cont.)

**Mutation** type

- type defines entry points for write operations

```graphql
# schema.graphql
.
.

type Query {
  addBook(title: String, author: String): Book
}
```

---

# Coding Section

Start With **apollo-server**

```bash
pnpm add  apollo-server graphql
```

---

# Start With **apollo-server** (Cont.)

```javascript
const { ApolloServer, gql } = require("apollo-server");

// The GraphQL schema
const typeDefs = gql`
  type Query {
    "A simple type for getting started!"
    hello: String
  }
`;

// A map of functions which return data for the schema.
const resolvers = {
  Query: {
    hello: () => "world",
  },
};

const server = new ApolloServer({
  typeDefs,
  resolvers,
});

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`);
});
```

---
