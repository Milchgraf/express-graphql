# Express, GraphQL, MongoDB, React - GETTING STARTED

## Required Dependencies for GraphQL-Server:

1. express
2. graphql
3. express-graphql
4. mongoose
5. cors (optional)

`npm i express graphql express-graphql mongoose cors`

## Create app.js file

### 1. import dependencies

```javascript
const express = require('express');
const graphqlHTTP = require('express-graphql');
const mongoose =  require('mongoose');
const cors = require('cors');
```

### 2. import GraphQL-Schema

```javascript
const schema = require('./schema/schema');
```

### 3. use express

```javascript
const app = express();
```

### 4. connect to mongodb database

```javascript
mongoose.connect('mongodb://localhost:27017/gql-demo');
mongoose.connection.once('open', () => {
  console.log('connected to mlab database..');
});
```

### 5. use graphql endpoint with schema as option

```javascript
app.use('/graphql', graphqlHTTP({
  schema,
  
  // optional UI:
  graphiql: true
}));
```

### 6. start server

```javascript
app.listen(4000, () => {
  console.log('Listen on port 4000..');
});
```

### [OPTIONAL] allow cors

```javascript
app.use(cors());
```

## Create schema.js file

### 1. import dependencies

```javascript
const graphql = require('graphql');
const Book = require('../models/book');
const Author = require('../models/author');
```

### 2. import GraphQL-Datatypes

```javascript
const { GraphQLObjectType, GraphQLString, GraphQLSchema, GraphQLID, GraphQLInt, GraphQLList, GraphQLNonNull } = graphql;
```

### 3. create some types i.e. BookType (simple get operation)

```javascript
const BookType = new GraphQLObjectType({
  name: 'Book',
  fields: () => ({
    id: { type: GraphQLID },
    name: { type: GraphQLString },
    genre: { type: GraphQLString },
    author: {
      type: AuthorType,
      resolve(parent, args) {
        return Author.findById(parent.authorId);
      }
    }
  })
});
```


















