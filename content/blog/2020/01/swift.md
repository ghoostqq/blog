---
title: "Swift Quick start"
date: 2020-01-14T23:18:11+09:00
draft: false
categories:
tags:
- Swift
- iOS
keywords:
---

[100DaysOfSwiftUI](https://www.hackingwithswift.com/100/swiftui)の振り返り用Cheat sheetです．

## Day1

`var` 変数を宣言
`let` 定数を宣言（後から変更不可）（constatns）
`"Apple costs \(apple)"` String Interpolation　文字列の中に埋め込む

```swift
let album: String = "Reputation"
let year: Int = 1989
let height: Double = 1.78
let taylorRocks: Bool = true
```
型の指定。特別に指定しなくても初期化の時に自動判断されるが、特に指定する場合の文法は上記の通り。

また，変数の型を変更することはできない（type safe）．

## Day2

### collections

#### Array

```swift
let beatles = ["John", "Paul", "George", "Ringo"]
beatles[1]
```

#### Sets

```swift
let colors2 = Set(["red", "green", "blue", "red", "blue"])
```

#### Dictionaries

```swift
let heights = [
    "Taylor Swift": 1.78,
    "Ed Sheeran": 1.73
]
```

```swift
favoriteIceCream["Charlotte", default: "Unknown"]
```

存在しないキーを読んだら`nil`が帰るが、デフォルトを指定すればデフォルトが帰る。

#### Tuple

Tupleはどうやらそもそも型ではなさそう．collectionでもない．

```swift
var name = (first: "Taylor", last: "Swift")
```
tupleの使い方。あたいは変更できるが、型は変更できない。
`name.0`または`name.first`のように、ポジションまたは名前でアクセスできる。

### collections init and type annotation

```swift
// Array
var results = [Int]()
var results = Array<Int>()

// Dictionary
var teams = [String: String]()
var scores = Dictionary<String, Int>()

// Set
var numbers = Set<Int>()
```

ArrayとDictionaryにだけ，特別な記法がある．`Set<Int>()`のような記法の方がデフォルト．

### enum

```swift
enum Result {
    case success
    case failure
}
```
enumでは型を定義できる。この例ではResult型を定義し、successまたはfalureのみを選択できる
```swift
let result4 = Result.failure
```

caseにさらなる情報を付加できる
```swift
enum Activity {
    case bored
    case running(destination: String)
    case talking(topic: String)
    case singing(volume: Int)
}
```
使い方は
```swift
let talking = Activity.talking(topic: "football")
```

この付加情報は定義されている場合，初期化時に具体値を与えなければならない．

#### enumのIndex指定

```swift
enum Planet: Int {
    case mercury
    case venus
    case earth
    case mars
}
```
とすることで、caseにIntで0から自動で割り振られるので、earthは2が割り振られる。
```swift
let earth = Planet(rawValue: 2)
```
のように使うことができる．
```swift
enum Planet: Int {
    case mercury = 1
    case venus
    case earth
    case mars
}
```
このように個別に指定することができる。その後は自動でインクリメンタルするので、earthは3が割り振られる。

## Day3

```swift
12 + 4
12 - 4
12 * 4
12 / 4
12 / 5  // 2
12.0 / 5  // 2.4
12 / 5.0  // 2.4
12 % 5

"Hello " + "World"
["John", "Paul"] + ["George", "Ringo"]  // ["John", "Paul", "George", "Ringo"]

// compound assignment operators
var count = 0
count += 1

// comparison operators
6 == 4
6 != 4
6 < 4
6 >= 4
"abc" < "bbc"  // true
```

#### conditions

```swift
var sex = "man"
if sex == "man" {
    print("Hello, Mr.")
} else if sex == "woman" {
    print("Hello, Ms.")
} else {
    print("Hello, sir")
}

var age = 19
if age >= 18 && age < 20 {
    print("You can drive but not drink")
}
// And
true && false
// Or
true || false
```

##### 三項演算子

```swift
// the tenary operator
var sex2 = "woman"
print("Hello " + (sex2 == "man" ? "boy" : "girl"))
```

### switch文

```swift
// switch statements
let weather = "sunny"
switch weather {
case "rain":
    print("Bring an umbrella")
case "sunny":
    print("Wear sunglass")
    fallthrough
case "snow":
    print("Wrap up warn")
default:
    print("Enjoy your day!")
}
// "Wear sunglass\n"
// "Wrap up warn\n"
// ↑snowの中身がfallthroughにより実行された．
```

fallthroughは次のcaseまたはdefaultを実行するみたい．使い道限られそう．


```swift
// range operators
let score = 84
switch score {
case ..<0:
    print("Error")
case 0..<50:
    print("Failed")
case 50...84:
    print("Pass")
default:
    print("Great!")
}
```

range operatorは`a..<b`：aからbまででbを含まない；`a...b`：bを含む；の2種類．

