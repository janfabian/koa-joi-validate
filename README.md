# Koa-Joi-Validate

A tiny module to provide Joi validation middleware within a Koa server.

Calling the module allows you to easily generate [Koa]https://github.com/koajs/koa) middleware to validate incoming requests using [Joi](https://github.com/hapijs/joi).

> This module is in mid-development, I built it in order to allow utility class from another in-development app to be reusable across different projects.  I don't really recommend using it in your projects yet, since it has not yet been fully tested.  It will likely be ready within a few weeks.


### Install

```
npm install koa-joi-validate
```

### Import

```
const OTP = require('koa-joi-validate')
```

### Usage

To use the module, call `validate` with an object containing Joi validation objects for the requestion headers, URL query, URL params, and post body.

The following basic example will verify that any request to the server contains a properly formatted request ID header and user ID url query parameter.
```javascript
const Koa = require('koa')
const joi = require('joi')
const validate = require('koa-joi-validate')

const app = new Koa()

app.use(validate({
  headers: {
    // Request headers Joi validation object
    "x-request-id": joi.string().alphanum().length(32)
  },
  query: {
    // URL query string Joi validation object
    userid: joi.string().required()
  },
  params: {
    // URL path parameters Joi validation object
  }
  body: {
    // POST body Joi validation object
  }
}))

app.use(async ctx => {
  ctx.body = 'Hello World';
});


app.listen(5000)
```

Here is another basic example, mounting a validator on a specific route using [koa-router](https://github.com/alexmingoia/koa-router).
```javascript
const router = new Router()

const loginValidator = validate({
  body: {
    username: Joi.string().required(),
    password: Joi.string().required()
  }
})

router.post('/login', loginValidator, async ctx => {
  const { username, password } = ctx.body
  const response = await login(username, password)
  ctx.body = response
})
```

For more examples of the (very powerful) validation capabilities of Joi, view the official documentation - https://github.com/hapijs/joi

If the validation fails, an HTTP 400 response will be returned to the client, along with a short human-readable error message explaining why the request was rejected.