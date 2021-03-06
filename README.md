# Paymongo for Node.js

Lighweight (6.6kB) node.js client for Paymongo API.

As seen on Paymongo's [community-made libraries](https://developers.paymongo.com/docs/community-made-libraries). :heart:

If you like this project, please give it a star, and consider following the author. :)

![npm bundle size](https://img.shields.io/bundlephobia/min/paymongo?style=flat-square) ![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/jofftiquez/paymongo/Deploy/master?label=test&style=flat-square) ![npm](https://img.shields.io/npm/v/paymongo?style=flat-square) ![npm](https://img.shields.io/npm/dw/paymongo?style=flat-square)

- [Paymongo for Node.js](#paymongo-for-nodejs)
    - [Installation](#installation)
    - [Usage](#usage)
  - [Tokens](#tokens)
    - [Create Token](#create-token)
    - [Get Token](#get-token)
  - [Sources](#sources)
    - [Create a Source](#create-a-source)
  - [Payments](#payments)
    - [Create Payment](#create-payment)
    - [Get Payment](#get-payment)
    - [Get Payments](#get-payments)
  - [Webhooks](#webhooks)
    - [Create a Webhook](#create-a-webhook)
    - [List all Webhooks](#list-all-webhooks)
    - [Toggle a Webhook (Enable or Disable a Webhook)](#toggle-a-webhook-enable-or-disable-a-webhook)
    - [Update a Webhook](#update-a-webhook)

### Installation

```bash
$ npm install paymongo
```

```bash
$ yarn add paymongo
```

### Usage

```javascript
import Paymongo from 'paymongo';

// Retrieve the secret key from your paymongo 
// dashboard under developers tab.
const paymongo = new Paymongo(process.env.SECRET_KEY);
```

## Tokens

### Create Token

> ### .createToken(payload)

Creates a one-time use token representing your customer's credit card details. **NOTE: This token can only be used once to create a Payment**. You must create separate tokens for every payment attempt.

**Payload**

Refer to [Paymongo documentation](https://developers.paymongo.com/reference#create-a-token) for payload guidelines.

**Sample**

```js
const payload = {
  data: {
    attributes: {
      number: '4242424242424242',
      exp_month: 1,
      exp_year: 23,
      cvc: '111'
    }
  }
};

const token = await paymongo.createToken(payload);
```

### Get Token

> ### .getToken(id)

Retrieve a token given an ID. The prefix for the id is `tok_` followed by a unique hash representing the token.

Just pass the token id to `.getToken(tokenId)`.

**Sample**

```js
const token = await paymongo.getToken('tok_6SGC9TBsjduCnV6HiAmXgctt');
```

## Sources

### Create a Source

> ### .createSource(payload)

Creates a Source

Refer to [Paymongo documentation](https://developers.paymongo.com/reference#post_sources-1) for payload guidelines.

**Sample**

```js
const payload = {
  data: {
    attributes: {
      type: 'gcash', // The only option for now accordint to their support
      amount: 1000,
      currency: 'php',
      redirect: {
        success: 'https://your-website.com/success',
        failed: 'https://your-website.com/failed'
      }
    }
  }
};

const source = await paymongo.createSource(payload);
```

## Payments

### Create Payment

> ### .createPayment(payload)

To charge a payment source, you must create a Payment object. When in test mode, your payment sources won't actually be charged. You can select specific payment sources for different success and failure scenarios.

**Payload**

Refer to [Paymongo documentation](https://developers.paymongo.com/reference#create-a-payment) for payload guidelines.

**Sample**

```js
const payload = {
  data: {
    attributes: {
      amount: 10000,
      currency: 'PHP',
      description: '1 month premium subscription',
      statement_descriptor: 'MYCURE Premium',
      source: {
        id: 'tok_6SGC9TBsjduCnV6HiAmXgctt',
        type: 'token'
      }
    }
  }
};

const payment = await paymongo.createPayment(payload);
```

### Get Payment

> ### .getPayment(id)

You can retrieve a Payment by providing a payment ID. The prefix for the id is `pay_` followed by a unique hash representing the payment.

Just pass the payment id to `.getPayment(paymentid)`.

**Sample**

```js
const payment = await paymongo.getPayment('pay_SMizRExCTxdZAbnTaRiRrExN');
```

### Get Payments

> ### .getPayments()

Returns all the payments you previously created, with the most recent payments returned first. Currently, all payments are returned as one batch. We will be introducing pagination and limits in the next iteration of the API.

Just pass the payment id to `.getPayments()`.

**Sample**

```js
const payments = await paymongo.getPayments();
```

## Webhooks

### Create a Webhook

> ### .createWebhook(payload)

Creates a webhook.

**Payload**

Refer to [Paymongo documentation](https://developers.paymongo.com/reference#post_webhooks-1) for payload guidelines.

**Sample**

```js
const payload = {
  data: {
    attributes: {
      url: 'https://your-url.com',
      events: [
        // It is not specified in their official documentation
        // what the available events are yet.
      ]
    }
  }
};

const webhook = await paymongo.createWebhook(payload);
```

### List all Webhooks

> ### .getWebhooks()

Returns all the webhooks you previously created, with the most recent webhooks returned first.

**Sample**

```js
const webhooks = await paymongo.getWebhooks();
```

### Toggle a Webhook (Enable or Disable a Webhook)

> ### .toggleWebhook(action, id)

Enables or disables a webhook.

**Params**

| Param | Description |
| ----- | ----------- |
| action:string | Action type, value must be one of 'enable' or 'disable'. |
| id:string | The webhook id. |

**Sample**

*Enable*

```js
const webhook = await paymongo.toggleWebhook('enable', 'webhook-123');
```

*Disable*

```js
const webhook = await paymongo.toggleWebhook('disable', 'webhook-123');
```

### Update a Webhook

> ### .updateWebhook(payload)

Updates a Webhook.

**Payload**

Refer to [Paymongo documentation](https://developers.paymongo.com/reference#post_webhooks-1) for payload guidelines.

```js
const payload = {
  data: {
    attributes: {
      url: 'https://your-url.com'
    }
  }
};

const webhook = await paymongo.updateWebhook(payload);
```


Made with :heart: by Jofferson Ramirez Tiquez
