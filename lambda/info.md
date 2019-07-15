## 参考
### line messaging api
* https://dev.classmethod.jp/etc/lambda-line-bot-tutorial/

## 手順
### デプロイ
* zip -r lambda.zip index.js properties.json credentials.json node_modules/
* aws lambda update-function-code --function-name test --zip-file fileb://./lambda.zip
