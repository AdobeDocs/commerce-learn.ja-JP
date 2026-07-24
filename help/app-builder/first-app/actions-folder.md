---
title: アクションフォルダー
description: サンプルアプリケーションのOAuth認証とCommerce接続パターンなど、App Builder アクションフォルダー内のJavaScript ファイルとフォルダーについて説明します。
jira: KT-21680
doc-type: Tutorial
duration: 465
last-substantial-update: 2023-03-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: d13ed1e7-b18e-4bf5-af87-2a69e2588d65
TQID: https://experienceleague.adobe.com/v3hBL3cNfFO-xM8Dat2Be21-ihyORvvSrvZYZ-6wzf0
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 282072f1e29b836d19dee2e1b6498f75150fe3a5
workflow-type: tm+mt
source-wordcount: 199
ht-degree: 0%

---

# アクションフォルダーについて説明します {#actions-folder}

このサンプルアプリの`actions` フォルダーには、いくつかのJavaScript ファイルと`commerce`という1つのフォルダーが含まれています。 表示されているJavaScriptは、作業に関連するファイルであれば再利用できる便利なサンプルファイルです。 このフォルダーは、OAuthとRESTを使用してAdobe Commerce アプリケーションに接続する際の開発作業の時間を節約できます。

この例のフォルダーの実際の名前は任意ですが、名前を知ることで、サンプルコードを解釈できます。 意味のある命名規則を使用することで、アプリケーションがより複雑になる場合の混乱を避けることができます。

## この動画は誰のためのものでしょうか？

* Adobe Commerceを初めて使用する開発者で、Adobe App Builderの使用経験が限られており、サンプルアプリケーションのactions フォルダーについて学習しているユーザー。

## ビデオコンテンツ

* App Builderの概要と、`actions` フォルダーに焦点を当てたサンプルモジュール
* 「アクション」フォルダーの使用方法
* `actions`および`commerce` フォルダーにあるJavaScript ファイルの目的
* OAuth認証ファイルの概要

>[!VIDEO](https://video.tv.adobe.com/v/3421081?captions=jpn&learn=on)

## コードサンプル

actions/oauth1a.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, checkMissingRequestInputs} = require('../utils')
const { getCommerceOauthClient } = require('../oauth1a')

async function main(params) {
    const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

    try {
        const requiredParams = ['operation', 'COMMERCE_BASE_URL']
        const requiredHeaders = ['Authorization']
        const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
        if (errorMessage) {
            // return and log client errors
            return errorResponse(400, errorMessage, logger)
        }

        const { operation } = params

        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )

        const content = await oauth.get(operation)

        return {
            statusCode: 200,
            body: content
        }
    } catch (error) {
        // log any server errors
        logger.error(error)
        // return with 500
        return errorResponse(500, error, logger)
    }
}

exports.main = main
```

actions/utils.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */

/* This file exposes some common utilities for your actions */

/**
 *
 * Returns a log ready string of the action input parameters.
 * The `Authorization` header content will be replaced by '<hidden>'.
 *
 * @param {object} params action input parameters.
 *
 * @returns {string}
 *
 */
function stringParameters (params) {
  // hide authorization token without overriding params
  let headers = params.__ow_headers || {}
  if (headers.authorization) {
    headers = { ...headers, authorization: '<hidden>' }
  }
  return JSON.stringify({ ...params, __ow_headers: headers })
}

/**
 *
 * Returns the list of missing keys giving an object and its required keys.
 * A parameter is missing if its value is undefined or ''.
 * A value of 0 or null is not considered as missing.
 *
 * @param {object} obj object to check.
 * @param {array} required list of required keys.
 *        Each element can be multi level deep using a '.' separator e.g. 'myRequiredObj.myRequiredKey'
 *
 * @returns {array}
 * @private
 */
function getMissingKeys (obj, required) {
  return required.filter(r => {
    const splits = r.split('.')
    const last = splits[splits.length - 1]
    const traverse = splits.slice(0, -1).reduce((tObj, split) => { tObj = (tObj[split] || {}); return tObj }, obj)
    return traverse[last] === undefined || traverse[last] === '' // missing default params are empty string
  })
}

/**
 *
 * Returns the list of missing keys giving an object and its required keys.
 * A parameter is missing if its value is undefined or ''.
 * A value of 0 or null is not considered as missing.
 *
 * @param {object} params action input parameters.
 * @param {array} requiredHeaders list of required input headers.
 * @param {array} requiredParams list of required input parameters.
 *        Each element can be multi level deep using a '.' separator e.g. 'myRequiredObj.myRequiredKey'.
 *
 * @returns {string} if the return value is not null, then it holds an error message describing the missing inputs.
 *
 */
function checkMissingRequestInputs (params, requiredParams = [], requiredHeaders = []) {
  let errorMessage = null

  // input headers are always lowercase
  requiredHeaders = requiredHeaders.map(h => h.toLowerCase())
  // check for missing headers
  const missingHeaders = getMissingKeys(params.__ow_headers || {}, requiredHeaders)
  if (missingHeaders.length > 0) {
    errorMessage = `missing header(s) '${missingHeaders}'`
  }

  // check for missing parameters
  const missingParams = getMissingKeys(params, requiredParams)
  if (missingParams.length > 0) {
    if (errorMessage) {
      errorMessage += ' and '
    } else {
      errorMessage = ''
    }
    errorMessage += `missing parameter(s) '${missingParams}'`
  }

  return errorMessage
}

/**
 *
 * Extracts the bearer token string from the Authorization header in the request parameters.
 *
 * @param {object} params action input parameters.
 *
 * @returns {string|undefined} the token string or undefined if not set in request headers.
 *
 */
function getBearerToken (params) {
  if (params.__ow_headers &&
      params.__ow_headers.authorization &&
      params.__ow_headers.authorization.startsWith('Bearer ')) {
    return params.__ow_headers.authorization.substring('Bearer '.length)
  }
  return undefined
}
/**
 *
 * Returns an error response object and attempts to log.info the status code and error message
 *
 * @param {number} statusCode the error status code.
 *        e.g. 400
 * @param {string} message the error message.
 *        e.g. 'missing xyz parameter'
 * @param {*} [logger] an optional logger instance object with an `info` method
 *        e.g. `new require('@adobe/aio-sdk').Core.Logger('name')`
 *
 * @returns {object} the error object, ready to be returned from the action main's function.
 *
 */
function errorResponse (statusCode, message, logger) {
  if (logger && typeof logger.info === 'function') {
    logger.info(`${statusCode}: ${message}`)
  }
  return {
    error: {
      statusCode,
      body: {
        error: message
      }
    }
  }
}

module.exports = {
  errorResponse,
  getBearerToken,
  stringParameters,
  checkMissingRequestInputs
}
```

actions/commerce/index.js

```javascript
/*
 * Copyright 2023 Adobe
 * All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of Adobe and its suppliers, if any. The intellectual
 * and technical concepts contained herein are proprietary to Adobe
 * and its suppliers and are protected by all applicable intellectual
 * property laws, including trade secret and copyright laws.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Adobe.
 */
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, checkMissingRequestInputs} = require('../utils')
const { getCommerceOauthClient } = require('../oauth1a')

async function main(params) {
    const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

    try {
        const requiredParams = ['operation', 'COMMERCE_BASE_URL']
        const requiredHeaders = ['Authorization']
        const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
        if (errorMessage) {
            // return and log client errors
            return errorResponse(400, errorMessage, logger)
        }

        const { operation } = params

        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )

        const content = await oauth.get(operation)

        return {
            statusCode: 200,
            body: content
        }
    } catch (error) {
        // log any server errors
        logger.error(error)
        // return with 500
        return errorResponse(500, error, logger)
    }
}

exports.main = main
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}


