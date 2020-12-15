# Crud Example Frontend Vue

## Introduction

Vue server

### Available resources

The following resources are available:

+ ``GET /api/posts`` (``JSON``) : getAllPosts
+ ``GET /api/posts/:id`` (``JSON``) : getPostById
+ ``POST /api/posts`` : createPost
+ ``PUT /api/posts/:id`` : updatePost
+ ``DELETE /api/posts/:id`` : deletePost
+ ``DELETE /api/posts`` : deleteAllPosts
+ ``GET /api/posts/status`` (``JSON``) : findByStatus

### Front-end

The front-end is using an [Vue.js](https://vuejs.org/).

## How to build and execute it

### Requirements

+ git
+ node.js

#### Project setup

```
npm install
```

#### Compiles and hot-reloads for development

```
npm run serve
```

#### Compiles and minifies for production

```
npm run build
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
