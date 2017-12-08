---
layout: post
title:  Rails 5.1 からデフォルトになった Primary Key の Bigint に対応する
date:   2017-12-15 00:00:00 +09:00
categories:
    - tech
tags:
    - Rails
    - MySQL
author: deraru
---

Rails 5.1からデフォルトになった、 Primary Key の Bigint に対応する作業のログです。

DB は MySQL です。

# 新規テーブルの Primary Key を Bigint にせず、 Integer にする場合

新しくテーブルを作る際のマイグレーションファイルで、 `create_table` に `id: :integer` を渡すように編集します。

```ruby
class CreateDesks < ActiveRecord::Migration[5.1]
  def change
    create_table "desks", id: :integer do |t|
      t.string "name"
    end
  end
end
```

# 既存テーブルの Primary Key を Integer から Bigint にする場合

## 外部キーが無い場合

以下のようなマイグレーションを実行して、既存テーブルの Primary Key を Bigint に移行します。

```ruby
class MigrateChairsPrimaryKeyToBigint < ActiveRecord::Migration[5.1]
  def up
    change_column :chairs, :id, :bigint, null: false, auto_increment: true
  end

  def down
    change_column :chairs, :id, :integer, null: false, auto_increment: true
  end
end
```


## 外部キーが有る場合

外部キーが有る場合は、親テーブルの被参照カラムのデータ型と子テーブルの外部キー用のカラムのデータ型を揃える必要があるので、一旦外部キーを削除し、親テーブルの Primary Key を Bigint に移行した後、子テーブルの外部キー用のカラムも Bigint に移行し、その後に外部キーを作り直す必要があります。

```ruby
class MigrateUsersPrimaryKeyToBigint < ActiveRecord::Migration[5.1]
  def up
    # 一旦外部キーを削除する
    remove_foreign_key :chairs, :users

    # 親テーブルのPrimary KeyをBigintに変更する
    change_column :users, :id, :bigint, null: false, auto_increment: true
    # 子テーブルの外部キー用のカラムもBigintに変更する
    change_column :chairs, :user_id, :bigint, null: false

    # 外部キーを作り直す
    add_foreign_key :chairs, :users
  end

  def down
    remove_foreign_key :chairs, :users
    change_column :chairs, :user_id, :integer, null: false
    change_column :users, :id, :integer, null:false, auto_increment: true
    add_foreign_key :chairs, :users
  end
end
```

なかなか重いデータベースの処理が走るので、データ量が多い場合は一度に全テーブルを移行せず、何度かに分けて移行したほうがよさそうです。

1テーブルですら処理が重い場合は、外部キーと同様にインデックスを削除してからカラム変更を実行し、カラム変更後にインデックスを作り直す、などの対応をしてもよさそうです。
