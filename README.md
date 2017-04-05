# api documentation for  [amazon-product-api (v0.4.3)](https://github.com/t3chnoboy/amazon-product-api)  [![npm package](https://img.shields.io/npm/v/npmdoc-amazon-product-api.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-amazon-product-api) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-amazon-product-api.svg)](https://travis-ci.org/npmdoc/node-npmdoc-amazon-product-api)
#### Amazon Product Advertising API client

[![NPM](https://nodei.co/npm/amazon-product-api.png?downloads=true)](https://www.npmjs.com/package/amazon-product-api)

[![apidoc](https://npmdoc.github.io/node-npmdoc-amazon-product-api/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-amazon-product-api_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-amazon-product-api/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-amazon-product-api/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-amazon-product-api/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Dmitry Mazuro",
        "email": "dmitry.mazuro@icloud.com"
    },
    "bugs": {
        "url": "https://github.com/t3chnoboy/amazon-product-api/issues"
    },
    "dependencies": {
        "es6-promise": "3.0.2",
        "request": "2.53.0",
        "xml2js": "0.4.5"
    },
    "description": "Amazon Product Advertising API client",
    "devDependencies": {
        "coffee-script": "1.7.1",
        "mocha": "1.20.1",
        "should": "4.0.4"
    },
    "directories": {
        "lib": "./lib/"
    },
    "dist": {
        "shasum": "9df440e71ecbc8da5fc985a9fc3d5a4171e11f1c",
        "tarball": "https://registry.npmjs.org/amazon-product-api/-/amazon-product-api-0.4.3.tgz"
    },
    "gitHead": "0f7094bd37ed3161a2ba7318a21672fdcb3ead80",
    "homepage": "https://github.com/t3chnoboy/amazon-product-api",
    "keywords": [
        "amazon",
        "aws",
        "product",
        "ads",
        "advertising"
    ],
    "license": "BSD-2-Clause",
    "main": "./lib/index.js",
    "maintainers": [
        {
            "name": "t3chnoboy",
            "email": "dmitry.mazuro@icloud.com"
        },
        {
            "name": "mastert",
            "email": "simonthiboutot.qc@gmail.com"
        }
    ],
    "name": "amazon-product-api",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/t3chnoboy/amazon-product-api.git"
    },
    "scripts": {
        "test": "mocha"
    },
    "version": "0.4.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module amazon-product-api](#apidoc.module.amazon-product-api)
1.  [function <span class="apidocSignatureSpan">amazon-product-api.</span>createClient (credentials)](#apidoc.element.amazon-product-api.createClient)
1.  object <span class="apidocSignatureSpan">amazon-product-api.</span>utils

#### [module amazon-product-api.utils](#apidoc.module.amazon-product-api.utils)
1.  [function <span class="apidocSignatureSpan">amazon-product-api.utils.</span>formatQueryParams (query, method, credentials)](#apidoc.element.amazon-product-api.utils.formatQueryParams)
1.  [function <span class="apidocSignatureSpan">amazon-product-api.utils.</span>generateQueryString (query, method, credentials)](#apidoc.element.amazon-product-api.utils.generateQueryString)



# <a name="apidoc.module.amazon-product-api"></a>[module amazon-product-api](#apidoc.module.amazon-product-api)

#### <a name="apidoc.element.amazon-product-api.createClient"></a>[function <span class="apidocSignatureSpan">amazon-product-api.</span>createClient (credentials)](#apidoc.element.amazon-product-api.createClient)
- description and source-code
```javascript
createClient = function (credentials) {
  return {
    itemSearch: runQuery(credentials, 'ItemSearch'),
    itemLookup: runQuery(credentials, 'ItemLookup'),
    browseNodeLookup: runQuery(credentials, 'BrowseNodeLookup')
  };
}
```
- example usage
```shell
...
Require library
'''javascript
var amazon = require('amazon-product-api');
'''

Create client
'''javascript
var client = amazon.createClient({
  awsId: "aws ID",
  awsSecret: "aws Secret",
  awsTag: "aws Tag"
});
'''
Now you are ready to use the API!
...
```



# <a name="apidoc.module.amazon-product-api.utils"></a>[module amazon-product-api.utils](#apidoc.module.amazon-product-api.utils)

#### <a name="apidoc.element.amazon-product-api.utils.formatQueryParams"></a>[function <span class="apidocSignatureSpan">amazon-product-api.utils.</span>formatQueryParams (query, method, credentials)](#apidoc.element.amazon-product-api.utils.formatQueryParams)
- description and source-code
```javascript
formatQueryParams = function (query, method, credentials) {
  var params = {};

  // format query keys
  for (var param in query) {
    var capitalized = capitalize(param);
    params[capitalized] = query[param];
  }

  if (method === 'ItemSearch') {
    // Default
    params = setDefaultParams(params, {
      SearchIndex: 'All',
      Condition: 'All',
      ResponseGroup: 'ItemAttributes',
      Keywords: '',
      ItemPage: '1'
    });

  } else if (method === 'ItemLookup') {
    // Default
    params = setDefaultParams(params, {
      SearchIndex: 'All',
      Condition: 'All',
      ResponseGroup: 'ItemAttributes',
      IdType: 'ASIN',
      IncludeReviewsSummary: 'True',
      TruncateReviewsAt: '1000',
      VariationPage: 'All'
    });

    // Constraints
    // If ItemId is an ASIN (specified by IdType), a search index cannot be specified in the request.
    if (params['IdType'] === 'ASIN') {
      delete params['SearchIndex'];
    }

  } else if (method === 'BrowseNodeLookup') {
    // Default
    params = setDefaultParams(params, {
      BrowseNodeId: '',
      ResponseGroup: 'BrowseNodeInfo'
    });
  }


  // Constants
  params['Version'] = '2013-08-01';

  // Common params
  params['AWSAccessKeyId'] = credentials.awsId;
  // awsTag is associated with domain, so it ought to be defineable in query.
  params['AssociateTag'] = query.awsTag || credentials.awsTag;
  params['Timestamp'] = new Date().toISOString();
  params['Service'] = 'AWSECommerceService';
  params['Operation'] = method;

  // sort
  params = sort(params);

  return params;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.amazon-product-api.utils.generateQueryString"></a>[function <span class="apidocSignatureSpan">amazon-product-api.utils.</span>generateQueryString (query, method, credentials)](#apidoc.element.amazon-product-api.utils.generateQueryString)
- description and source-code
```javascript
generateQueryString = function (query, method, credentials) {
  var unsignedString = '';
  var domain = query.domain || 'webservices.amazon.com';
  var params = formatQueryParams(query, method, credentials);

  // generate query
  unsignedString = Object.keys(params).map(function (key) {
    return key + "=" + encodeURIComponent(params[key]).replace(/[!'()*]/g, function(c) {
      return '%' + c.charCodeAt(0).toString(16);
    });
  }).join("&")

  var signature = encodeURIComponent(generateSignature('GET\n' + domain + '\n/onca/xml\n' + unsignedString, credentials.awsSecret
)).replace(/\+/g, '%2B');
  var queryString = 'https://' + domain + '/onca/xml?' + unsignedString + '&Signature=' + signature;

  return queryString;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
