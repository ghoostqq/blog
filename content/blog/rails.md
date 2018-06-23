---
title: "Rails"
date: 2018-06-04T12:38:36+09:00
draft: true
---

# Model
## モデルのデータ全てについて行う

```ruby
User.all.each { ... }
```
これは大きいデータだとメモリ的によろしくない。
そこで：

```ruby
User.find_each { ... }
```
こうするといい。
[ref](http://guides.rubyonrails.org/active_record_querying.html#find-each)


# その他

個別スクリプトを走らせる時にRailsのモデルとか使いたい場合
```terminal
$ rails runner your_script.rb
```

ページを戻る
```html.erb
<%= link_to 'Back', :back %>
```


