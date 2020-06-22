# Request Check

You should not always believe the data in a request is what you think it is. Hopefully, you will validate data when it comes to your controllers. This module helps with that. I found that many of the validators out there are either incomplete or not fully customizable. Therefore, I built my own. It is rather simple and it works.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![npm version](https://badge.fury.io/js/request-check.svg)](https://badge.fury.io/js/request-check) [![Build Status](https://travis-ci.org/felipezarco/request-check.svg?branch=master)](https://travis-ci.org/felipezarco/request-check) [![Coverage Status](https://coveralls.io/repos/github/felipezarco/request-check/badge.svg?branch=master)](https://coveralls.io/github/felipezarco/request-check?branch=master) ![Dependencies](https://img.shields.io/david/felipezarco/request-check) ![Downloads](https://img.shields.io/npm/dw/request-check)


[![npm](https://nodei.co/npm/request-check.png)](https://www.npmjs.com/package/request-check)


## Install

Add `request-check` with your favorite package manager:

```
  yarn add request-check
```

## Usage

```typescript

  import rc from 'request-check'

  import { Request, Response } from 'express'

  class UserController {

    async create(request: Request, response: Response) {
      
      const { email, name } = request.body

      const invalid = rc.check({email}, {name})

      if(invalid) response.status(400).json({ invalid })   

      // ...

    }
  } 

```

### Basic Check

This line checks whether those two variables are set.

```javascript
  const invalid = rc.check({email}, {name})
```

Check will return an `Array` of objects with `field` and `message` **or** it will return `undefined` 

In the above example, if none of the variables are given, invalid will contain:

```javascript
[
  { field: 'name', message: 'The field name is required!' },
  { field: 'email', message: 'The field email is required!' }
]
```

If both fields are given, invalid will be **undefined**.

### Validations

In addition to check if the variable is set, `check` will also look for a rule definition for that variable.

This is how you can add a rule:

```typescript
rc.addRule('email', {
  validator: (email: any) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(String(email)), 
  message: 'The email given is not valid!'
})
```

Suppose this variables values:

```javascript
const email = 'felipezarco@hotmail'
const name = undefined
```

Now, when you call

```javascript
  const invalid = rc.check({email}, {name})
```

The output stored in `invalid` will contain:

```javascript
[
  { field: 'name', message: 'The field name is required!' },
  { field: 'email', message: 'The email given is not valid!' }
]
```

### Configuration

You can change the default required message by adding a single line of code:

```javascript
rc.requiredMessage = 'The field :name was not given =(!'
```

The symbol `:name` will be replaced with the field name. (Its use is optional)













