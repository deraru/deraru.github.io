---
layout: post
title:  Objective-C でのオブジェクトの作成方法
date:   2011-01-22 20:16:00 +09:00
categories:
    - tech
tags:
    - iOS
    - Objective-C
    - iPhone
---

2パターンある。
コンストラクタは便利だけどiPhoneはメモリが少ないから、出来る限り自分でメモリを割り当てて開放する方がいいみたい。（Appleが推奨してる）

- 自分でメモリを割り当てて（alloc）、初期化（init）する方法
    - 割り当てたメモリは、あとで自分で開放（release）する必要がある
    - `NSString *string = [[NSString alloc] init];`
        - メモリを割り当てて、初期化しただけ
    - `NSString *string = [[NSString alloc] initWithString:@”This is a string”];`
        - メモリを割り当てて、文字列で初期化
- コンストラクタを使う方法
    - 自分でメモリを割り当てる必要も、自分で開放する必要もない（自動的に開放（autorelease）される）
        - 自分で開放することもできるが、するべきではない
        - オブジェクトは後で自動的に開放される
            - 明示的に保持（retain）してある場合は保持される
    - `NSString *string = [NSString stringWithString:@”This is a string”];`
        - NSString クラスの stringWithString クラスメソッドは、NSStringオブジェクトを返す

参考資料

- Learning iPhone Programming
    - O’Reilly Media
