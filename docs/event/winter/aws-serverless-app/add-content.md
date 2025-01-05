---
title: "追加機能"
---
[講習](./../index.md)/[冬季イベント](../index.md)/[AWSでサーバーレスアプリを作ろう](./index.md)/追加機能


## CloudFrontにBasic認証をつける

```javascript
function handler(event) {
    var request = event.request;
    var headers = request.headers;

    // IDとPasswordを直接記載
    var username = "";
    var password = "";
    
    // ID:Password の文字列をBasic64でエンコードした文字列を設定する
    var authString = "Basic " + Buffer.from(username + ":" + password).toString('base64');

    if (
        typeof headers.authorization === "undefined" ||
        headers.authorization.value !== authString
    ) {
        return {
            statusCode: 401,
            statusDescription: "Unauthorized",
            headers: { "www-authenticate": { value: "Basic" } }
        };
    }
    return request;
}
```