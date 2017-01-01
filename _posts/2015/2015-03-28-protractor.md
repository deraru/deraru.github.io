# protractorハンズオン＆ハッカソン

- 2015/03/28 12:00 - 16:00
- [MSakamaki/protractor-handson](https://github.com/MSakamaki/protractor-handson)

# 概要

- E2Eテストツール
- 

## karmaとの比較

- 単体テスト
- テストランナーツール
    - テスト環境を持ち運べる
    - テスト環境とテストコードを分離する
- コンポーネント化する
- 中でJSコードを実行するだけ
- ブラウザの中でJSを動かしてテストする

## Selenium

- protractorでも利用している
- karmaと同程度のコード量で、E2Eが可能になった


## スタック

- テスト対象アプリケーション
    - Webアプリ
    - App Server
    - Node
- protractor
    - Webdriver, Selenium
    - WebdriverServer
    - Node
- テストコード

テストコード->Node->Selenium->Webdriver->App Server

# チュートリアル

- [AngularJS用テストフレームワーク「Protractor」チュートリアル日本語訳 - Qiita](http://qiita.com/weed/items/30098f7be2f753580f63)



