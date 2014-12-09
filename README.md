![logo](./logo50x50.png) Lambdaws
====================================

[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/mentum/lambdaws?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Build Status](https://img.shields.io/travis/mentum/lambdaws.svg?style=flat)](https://travis-ci.org/mentum/lambdaws)

Lambdaws makes it trivial to build highly scalable with high availability applications. Built on top of AWS Lambda.

## Installation

```npm install lambdaws```

## Usage

```λ``` takes a function accepting a callback and deploy it to AWS Lambda. If you call cloudedFunction it will run in the cloud.

```js
var λ = require('lambdaws').create;

var calculator = function(a, b, callback) { callback(a+b) };

var cloudedCalculator = λ(calculator);

cloudedCalculator(5, 2, function(data) { // Calls the function in the cloud, it doesn't run locally
	console.log(data); // Prints 7
});
```

### Overriding default settings

```js
λ(yourFunc, {
	memory: 256, // mb
	description: 'Description of your function',
	timeout: 10 // seconds
});
```

### Setting your AWS credentials

```js
var lambdaws = require('lambdaws');

lambdaws.config({
	accessKey: '', // string, AWS AccessKeyId
	secretKey: '', // string, AWS AccessKeySecret
	role: '' // string, AWS ARN. Must have full access to SQS
});

lambdaws.start();
```

Your AWS user credentials must have access to Lambda, SQS and S3.

### Full working example

See ```test/example.js```

## Limitations

The same [constraints](http://docs.aws.amazon.com/lambda/latest/dg/limits.html) imposed by AWS Lambda apply. Your function should also be stateless and independant of the underlying architecture. Be also aware that any variable that your function uses must be declared by your function. Global and outer scope variables are not uploaded to AWS Lambda.
