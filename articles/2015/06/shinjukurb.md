# shinjukurb

- 2015/06/24 19:30-21:00

## タイムゾーン

- 二宮さん
- スペースマーケット
    - 予約の時間
    - 支払い周りの時間
- 問題
    - システムのタイムゾーン: 未設定 (UTC)
        - 画面からJSTのつもりでDB保存、DB上ではUTCとして保存されている
            - JSTとUTCが混在
- 理想
    - DB上ではUTC
    - ユーザーに関してはユーザーのタイムゾーン
    - スペースについてはスペースが有る場所のタイムゾーン
        - アメリカの会議室ならアメリカのタイムゾーン
    - ApplicationController内でaround_actionでタイムゾーン設定をやった
        - ブログあり
- 移行
    - 深夜メンテナンス
    - メンテナンス中に該当の箇所(JSTと思って保存しているがUTCが使われている箇所)から9時間引く

## RSpec高速化

## APIのテストどうしてる？

## RSpecの網羅性

- Shared exampleのデメリット
    - 読みにくくなる
    - メンテナンスしにくくなる
    - 部分実行しづらくなる
    - letによるパラメータが爆発する
        - shared contextを使えば抑えられる

## Rails engine のRSpec

- Test::UnitはRails Engineのテストに対応
- RSpecは別Gem (ammeterなど) を使う必要あり
