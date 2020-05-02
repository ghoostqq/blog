---
title: "Swift Cheat sheet (1-15/100)"
date: 2020-01-14T23:18:11+09:00
draft: false
categories:
tags:
- Swift
- iOS
keywords:
---

[100DaysOfSwiftUI](https://www.hackingwithswift.com/100/swiftui)の振り返り用Cheat sheetです．Day1-Day15のまとめです．

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

#### collections init and type annotation

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

### conditions

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

#### 三項演算子

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

## Day4

### Loop

#### for loop

```swift
for i in 1...10 {
    print(i)
}

for album in ["Red", "1989", "Reputation"] {
    print("\(album) is on Apple Music")
}

for _ in 1..<4 {
    print("Yes")
}
```

参照が必要なければ`_`を指定することで，Swiftは評価をスキップしてくれる？

#### while loop

```swift
var number = 1
while number < 100 {
    if number == 40 {
        print("I cannot wait. I'm coming!")
        break
    }
    print(number)
    number += 1
}
print("Here I come!")
```

#### repeat loop

```swift
repeat {
    print(number)
    number += 1
} while number < 40
```

#### name loop to exit

```swift
outerLoop: for i in 1...10 {
    innerLoop: for j in 1...10 {
        if i*j % 3 == 0 {
            break innerLoop
        }
        
        print("\(i*j)")
        
        if i*j == 20 {
            break outerLoop
        }
    }
}
```

#### continue

```swift
for i in 1...10 {
    if i % 2 == 0 {
        continue
    }
    print(i)
}
```

## Day5

### funcitons

```swift
func printHW(name: String) {
    print("Hello world!")
    print("Welcome \(name)")
}
printHW(name: "Qiushi")

func square(number: Int) -> Int {
    return number * number
}
print(square(number: 8))
```

#### parameter labels

```swift
func sayHello(to name: String) {
    print("Hello \(name)")
}
sayHello(to: "Qiushi")
```

パラメーターラベルは複数定義できる．可読性のためにここまでするのはたまげる．

#### omitting parameter labels; default parameters

```swift
func greet(_ name: String) {
    print("Hello \(name)")
}
greet("Qiushi")

func greetTwo(_ name1: String, _ name2: String = "John Doe") {
    print("Hello \(name1) and \(name2)")
}
greetTwo("Qiushi", "Peco")
greetTwo("Qiushi")
```

#### variadic funcitons

```swift
func squareMany(numbers: Int...) {
    for number in numbers {
        print("\(number) squared is \(number * number)")
    }
}
squareMany(numbers: 1, 2, 3)
```

可変長の引数を受けとる．

### throwing functions

```swift
enum PasswordError: Error {
    case obvious
}
func checkPassword(_ password: String) throws -> Bool {
    if password != "password" {
        throw PasswordError.obvious
    }
    return true
}
```

Errorを継承？するエラーenumを定義する必要あり．

エラーをthrowしうるfuncはthrowsで示す必要がある．

#### running throwing functions -> do, try, catch

```swift
do {
    try checkPassword("wrong pass")
    print("Correct password!")
} catch {
    print("Wrong password!")
}
```

エラーを返しうる処理をfuncにしなければならないのかな．

### inout parameters

```swift
func doubleInPlace(number: inout Int) {
    number *= 2
}
var myNum = 10
doubleInPlace(number: &myNum)
print(myNum)
```

C++での参照渡しに似ている．

## Day6

### closures

```swift
let drivin = {
    print("I'm driving")
}
drivin()
```

#### closure with parameters

```swift
let drivin2 = { (place: String) in
    print("I'm driving to \(place)")
}
drivin2("London")
```

closureは呼ぶときlabelをつけない（つけられない）．

#### closure return values

```swift
let drivin3 = { (place: String) -> String in
    return "I'm driving to \(place) in my car"
}
print(drivin3("London"))
```

### closure as parameters

```swift
func travel(by means: ()->Void) {
    print("I'm traveling", terminator: " ")
    means()
}
travel(by: {print("driving in my car")})
```

#### trailing closure syntax

```swift
travel() {
    print("flying on the plane")
}
travel {
    print("with my bike")
}
```

最後の引数がclosureの場合，このように省略して呼べる．

## Day7

### parameter closure that takes parameter

```swift
func travel2(by means: (String) -> Void) {
    print("I'm traveling", terminator: " ")
    means("London")
}
travel2 { (place: String) in
    print("to \(place) in my car")
}
```

```swift
func travel3(action: (String) -> String) {
    print("I'm traveling", terminator: " ")
    print(action("London"))
}
travel3 { (place: String) -> String in
    return "to \(place) in my car."
}
```

####  shorthand parameter names
```swift
travel3 { place in return "to \(place) in my car"}
travel3 { return "to \($0) in my car" }
```
closureの引数が自明なら省略できる．`$0`によってポジション番号で呼ぶこともできる．

```swift
func travel4 (action: (String, Int) -> String) {
    print("I'm traveling", terminator: " ")
    print(action("London", 50))
}
travel4 {
    return "to \($0) at \($1) kilo meters per hour"
}

func travel5 () -> (String)->Void {
    return {
        print("Traveling to \($0)")
    }
}
let travelClosure = travel5()
travelClosure("London")
// travel5()("London") ;works but is not recommended.
```

### capturing values

```swift
func travel6() -> (String) -> Void {
    var counter = 1
    return {
        print("\(counter). I'm going to \($0)")
        counter += 1
    }
}
let travelClosure2 = travel6()
travelClosure2("London")
travelClosure2("London")
travelClosure2("London")
travelClosure2("London")
```

この例ではcounterがcaptureされて，functionが呼ばれ終わった後も参照されている．ややこしい．

## Day8

### creating your own structs

```swift
struct Sport {
    var name: String
}

var tennis = Sport(name: "Tennis")
print(tennis.name)
```

#### computed properties

```swift
struct Sport2 {
    var name: String // stored property
    var isOlympicSport: Bool // stored property
    
    var olympicStatus: String {  // computed property
        if isOlympicSport {
            return "\(name) is an Olympic sport"
        } else {
            return "\(name) is not an Olympic sport"
        }
    }
}

let chessBoxing = Sport2(name: "Chessboxing", isOlympicSport: false)
print(chessBoxing.olympicStatus)
```

closureなのか？　評価してreturnされるものを値として返すプロパティ．

#### property observers

```swift
struct Progress {
    var task: String
    var amount: Int {
        didSet {
            print("\(task) is now \(amount)% complete")
        }
    }
}
var progress = Progress(task: "Loading data", amount: 0)
progress.amount = 20
progress.amount = 60
progress.amount = 100
```

- didSet; evaluated after property changes.
- willSet ; rarerly used. evaluated before property changes.

### Methods

```swift
struct City {
    var population: Int
    
    func collectTaxes() -> Int {
        return population * 1000
    }
}

let london = City(population: 9_000_000)
london.collectTaxes()
```

structの中のfuncはmethodと呼ばれる．

#### mutating methods

```swift
struct Person {
    var name: String
    
    mutating func makeAnonymous() {
        name = "Anonymous"
    }
}

var person = Person(name: "Qiushi")
person.makeAnonymous()
```

structの中で変数がvarとして定義されるかletとして定義されるかにかかわらず，structがletとして定義されれば中の変数も定数になる．

structがvarとletどちらで初期化されるか分からないので，struct中の変数はデフォルトではmethodによって変更できないようになっている．

変更するには，mutatingをつける必要がある．

### String is a struct too.

```swift
let string  = "Do or do not, there is no try."
print(string.count)
print(string.hasPrefix("Do"))
print(string.uppercased())
print(string.sorted())
```

### Array is a struct too.

```swift
var toys = ["Woody"]
print(toys.count)
toys.append("Buzz")
toys.firstIndex(of: "Buzz")
print(toys.sorted())
toys.remove(at: 0)
```

## Day9

### initializers

```swift
struct User {
    var username: String
    
    init() {
        username = "Anonymous"
        print("Creating a new user!")
    }
}

var user = User()
user.username = "Qiushi"
```

initにはfuncをつけない．

initを定義する場合，すべてのstruct変数に値が付与される必要がある．

#### self refer to the current instantce

```swift
struct Person {
    var name: String
    
    init(name: String) {
        print("\(name) was born!")
        self.name = name
    }
}

var person = Person(name: "Qiushi")
```

initの中でinitの引数とstructインスタンス変数名を区別するためにselfを用いる．

### lazy properties

```swift
struct FamilyTree {
    init() {
        print("Creating family tree!")
    }
}
struct Person2 {
    var name: String
    lazy var familyTree = FamilyTree()
    
    init (name: String) {
        self.name = name
    }
}
var qq = Person2(name: "Qiushi")
qq.familyTree
```

lazyをつければ，参照されて初めて評価される．

### static properties and methods

```swift
struct Student {
    static var classSize = 0
    var name: String
    
    init (name: String) {
        self.name = name
        Student.classSize += 1
    }
}

let ed = Student(name: "Ed")
let taylor = Student(name: "Taylor")
print(Student.classSize)
```

staticをつけることで，struct変数になる．参照するにはインスタンスではな`struct名.変数名`で呼ぶ．

### access control

```swift
struct Person3 {
    private var id: String
    
    init(id: String) {
        self.id = id
    }
    
    func identify() -> String {
        return "My social security number is \(id)"
    }
}
```

- private; 参照できる範囲をインスタンス内に制限する．
- public

## Day10

### class

classはstructと違い，memberwise initializerがデフォルトで付いてこないため，必ずinitを定義する必要がある．

```swift
class Dog {
    var name: String
    var breed: String
    
    init(name: String, breed: String) {
        self.name = name
        self.breed = breed
    }
}
let poppy = Dog(name: "Poppy", breed: "Poodle")
```

#### class inheritance

```swift
class Poodle: Dog {
    init(name: String) {
        super.init(name: name, breed: "Poodle")
    }
}
```

super.init()は必須．

引数が同じだとinitはoverrideをつける必要があるよう．

#### override methods

```swift
class Dog2 {
    func makeNoise() {
        print("Woof!")
    }
}
class Poodle2: Dog2 {
    override func makeNoise() {
        print("Yip!")
    }
}
let poppy2 = Poodle2()
poppy2.makeNoise()
```

#### final class

```swift
final class Dog3 {
}
//class Poodle3: Dog3 {
//} // fails
```

#### copying objects

`=`でコピーする時，classは同じポインタとなり，structは違うポインタとなるため（？），コピー先の変更がオリジナルにclassでは反映され，structでは反映されない．（structとclassの違いその3）

```swift
class SingerClass {
    var name = "Taylor Swift"
}
var singer = SingerClass()
print(singer.name)
var singerCopy = singer
singerCopy.name = "Justin Bieber"
print(singer.name)

struct SingerStruct {
    var name = "Taylor Swift"
}
var singer2 = SingerStruct()
print(singer2.name)
var singerCopy2 = singer2
singerCopy2.name = "Justin Bieber"
print(singer2.name)
```

#### deinitializers

classにはdeinitializerがある．（structとclassの違いその4）

```swift
class PersonBorn {
    var name = "John Doe"
    init () {
        print("\(name) is born!")
    }
    deinit {
        print("\(name) is no more!")
    }
}
for _ in 1...3 {
    var person = PersonBorn()
}
```

#### mutability

constantのclass instanceでも変数がvariableなら変更可能．（structとclassの違いその5）

```swift
class Singer4 {
    var firstName = "Taylor"
    let lastName = "Swift"
}
let taylor = Singer4()
taylor.firstName = "John"
//taylor.lastName = "Doe" // fails
print(taylor.name)
```

## Day11

### protocol

protocolは変数やメソッドの概要を定義するもので，structがどんなprotocolかを指定して定義するのに使う．

```swift
protocol Identifiable {
    var id: String { get set }
}
struct User: Identifiable {
    var id: String
}
func displayID(thing: Identifiable) {
    print("My ID is \(thing.id)")
}
var user = User(id: "abc")
displayID(thing: user)
```

#### protocol inheritance

```swift
protocol Payable {
    func calculateWages() -> Int
}
protocol NeedsTraining {
    func study()
}
protocol HasVacation {
    func takeVacation(days: Int)
}
protocol Employee: Payable, NeedsTraining, HasVacation {}
```

### extensions

```swift
extension Int {
    func squared() -> Int {
        return self * self
    }
    var isEven: Bool {
        return self % 2 == 0
    }
}
let number = 8
number.squared()
number.isEven
```

既存のtypeにmethodを追加できる．

stored propertyを追加することはできないが，computed propertyはできる．

### protocol extensions

```swift
let pythons = ["Eric", "Graham", "John", "Michael", "Terry", "Terry"]
let beatles = Set(["John", "Paul", "George", "Ringo"])

extension Collection {
    func summarize() {
        print("There are \(count) of us:")
        for name in self {
            print(name)
        }
    }
}
pythons.summarize()
beatles.summarize(
```

protocolのextensionを作ることで，それを継承する型で追加機能が使えるようになる．

（protocolは機能の中身までは定義しないのに，変な感覚だ．）

### protocol-oriented programming

```swift
protocol Identifiable {
    var id: String {set get}
    func identify ()
}

extension Identifiable {
    func identify() -> Void {
        print("My ID is \(id).")
    }
}

struct User : Identifiable {
    var id: String
}

let twostraws = User(id: "twostraws")
twostraws.identify()
```

protocolのメソッドに対して継承するstructで一つ一つ内容を定義することもできるが，protocol-oriented programmingではデフォルトをextensionで定義することを推奨（？）している．

## Day12

### optionals

```swift
var age : Int? = nil
age = 12
```

#### unwrapping optionals

```swift
var name: String? = nil
if let unwrapped = name {
    print("\(unwrapped.count)")
} else {
    print("Missing name.")
}
```

#### unwrapping with guard

```swift
func greet(_ name: String?) {
    guard let unwrapped = name else {
        print("You didn't provide a name!")
        return
    }
    print("Hello, \(unwrapped)!")
}
greet(nil)
greet("Qiushi")
```

#### force unwrapping

```swift
let str = "5"
let num = Int(str)
let num2 = Int(str)!
```

StringをInt()で整数型に変換する時，Int?型になる．必ず整数だと分かっている場合，Int()!とすることで強制的にInt型に変換できる．

#### implicitly unwrapped options

```swift
var num3: Int! = nil
num3 = 1
```

最初にnilをとるが必ず値が割り振られると分かっている時，implicitly unwrapped optionsを用いることでif letやguard letを書かずに済む．

だが，標準的なoptionalsを使う方が安全だろう．

### nil coalescing

```swift
func username(for id: Int) -> String? {
    if id == 1 {
        return "Taylor"
    } else {
        return nil
    }
}
let user = username(for: 2) ?? "Anonymous"
```

### optional chaining ??????

```swift
let names = ["John", "Paul", "George", "Ringo"]
let beatle = names.first?.uppercased()
let nilArray = Array<String>()
nilArray.first
let nobody = nilArray.first?.uppercased()
let nilTooArr: [String?] = ["A", nil]
nilTooArr.first
nilTooArr.last
nilTooArr.last??.uppercased()
// ????
```

#### optional try

```swift
enum PasswordError: Error {
    case obvious
}
func checkPass (_ password: String) throws -> Bool {
    if password == "password" {
        throw PasswordError.obvious
    }
    return true
}

if let result = try? checkPass("password") {
    print("Result is \(result)")
} else {
    print("D'oh.")
}

try! checkPass("Sekrit")
print("OK")
```

### failable initializers

```swift
let string = "8"
let number = Int(string)  // failable initializer

struct User {
    var name: String
    
    init? (name: String) {
        if name.count > 3 {
            self.name = name
        } else {
            return nil
        }
    }
}
let userA = User(name: "Qi")
```

#### typecasting

`as?`を用いたif let構文により，もしそのクラスであればそのまま代入され，そうでなければnilとなりifに通らない．

```swift
class Animal {}
class Fish: Animal {}
class Dog: Animal {
    func makeNoise() {
        print("Woof")
    }
}
let pets = [Fish(), Dog(), Fish(), Dog()]
for pet in pets {
    if let dog = pet as? Dog {
        dog.makeNoise()
    }
}
```

## Day13-15

```swift
let tuple = (1,2)
type(of: tuple)  // -> (Int, Int).Type
```

tupleは型ではない．

```swift
nil ?? 1  // -> 1
```

`??`はnilのデフォルトのために使うという認識で良さそう．

```swift
var str : String? = "abc"
type(of: str)  // -> Optional<String>.Type

if let string = str {
    type(of: string)
    print("str is \(str)")
    print("string is \(string)")
}

var nilll: String? = nil
if let string = nilll {
    type(of: string)
} else {
    // stringはこの場合そもそも参照できない
    type(of: nilll)
}

```

```swift
enum Weather {
    case sun
    case cloud
    case rain
}
func headache (weather: Weather) -> String? {
    switch weather {
    case .sun:
        return "like"
    case .rain:
        return "hate"
    case .cloud:
        return nil
    }
}
```

enumのswitch文はdefaultがなくても場合が全部列挙できていれば良い．

```swift
class C {
    @objc func blabla() {
    }
}
@objcMembers class Cs {
    func blabla() {
    }
}
```

Objective-Cのコードを動くようにするもの．既存コードを使う場合必要な場面がありそう．

```swift
struct Person {
    private var age: Int
    fileprivate var name: String
    
    var ageInDogYears: Int {
        get {  // can be abbregated.
            return age * 7
        }
    }
}
struct User {
    let person: Person
    var username: String {
        return person.name
    }
}
```

fileprivateは同じファイルの中でprivateに利用するというもの．パッケージとして公開するばあいなどに使うか．

```swift
class Album {
}
class StudioAlbum: Album {
    var studio: String
    init(studio: String) {
        self.studio = studio
    }
}
class LiveAlbum: Album {
    var location: String
    init(location: String) {
        self.location = location
    }
}
var taylorSwift = StudioAlbum(studio: "The Castles Studios")
var fearless = StudioAlbum(studio: "Aimeeland Studio")
var iTunesLive = LiveAlbum(location: "New York")
var allAlbums: [Album] = [taylorSwift, fearless, iTunesLive]
for album in allAlbums as? [LiveAlbum] ?? [LiveAlbum]() {
    print(album.location)
}
```

array全体に対して`as?`を使うことができる．その場合，変換が失敗してnilになった場合にもループを走らせるために空のarrayが必要．上のコードはStudioAlbumをLiveAlbumに変換できないためnilとなり結果何もしない．