# Swift Style Guide

Make sure to read [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/).

Specifics from these guidelines + additional remarks are mentioned below.

This guide is mainly edited from [LinkedIn's Swift Style Guide](https://github.com/linkedin/swift-style-guide), after comparing and picking up some good stuffs from [Ray Wenderlich's Swift Style Guide](https://github.com/raywenderlich/swift-style-guide).

*Last updated for Swift 3.0 on January 16th, 2017.*

## Todo

- [ ] Rules for *Memory Management*
- [ ] Rules for *Xcode build time enhancement*
- [ ] Make `swiftlint.yml` containing rules from this guide


## Table Of Contents

- [Swift Style Guide](#swift-style-guide)
    - [0. Correctness](#0-correctness)
    - [1. Code Formatting](#1-code-formatting)
    - [2. Code Organization](#2-code-organization)
    - [3. Naming](#3-naming)
    - [4. General Coding Style](#4-general-coding-style)
    - [5. Access Modifiers](#5-access-modifiers)
    - [6. Custom Operators](#6-custom-operators)
    - [7. Switch Statements and `enum`s](#7-switch-statements-and-enums)
    - [8. Optionals](#8-optionals)
    - [9. Protocols](#9-protocols)
    - [10. Properties](#10-properties)
    - [11. Closures](#11-closures)
    - [12. Arrays](#12-arrays)
    - [13. Error Handling](#13-error-handling)
    - [14. Using `guard` Statements](#14-using-guard-statements)
    - [15. Documentation/Comments](#15-documentationcomments)
- [Other Guides](#other-guide)

## 0. Correctness

* **0.1.** Strive to make your code compile without warnings. This rule informs many style decisions such as using #selector types instead of string literals.
* **0.2.** Use a [static analyzer](http://stackoverflow.com/questions/49716/what-is-static-code-analysis) or [linter](http://stackoverflow.com/questions/8503559/what-is-linting) to enforce Swift style and conventions, such as [SwiftLint](https://github.com/realm/SwiftLint) *(recommended)* or [Tailor](https://github.com/sleekbyte/tailor). Please try to install SwiftLint via [CocoaPods](cocoapods.org) so that other team members don't need to install it on their computer.

## 1. Code Formatting

* **1.1.** 🔗 Use **4 spaces** for tabs.
* **1.2.** 🔗 Avoid uncomfortably long lines with a hard maximum of **160 characters per line** *(Xcode->Preferences->Text Editing->Page guide at column: 160 is helpful for this)*
* **1.3.** 🔗 Ensure that there is **a newline at the end of every file**
* **1.4.** 🔗 Ensure that there is **no trailing whitespace anywhere** *(Xcode->Preferences->Text Editing->Automatically trim trailing whitespace + Including whitespace-only lines)*
* **1.5.** 🔗 Do not place opening braces on new lines - we use the [1TBS style](https://en.m.wikipedia.org/wiki/Indent_style#Variant:_1TBS).

<details>
<summary>Examples</summary>

```swift
class SomeClass {
    func someMethod() {
        if x == y {
            /* ... */
        } else if x == z {
            /* ... */
        } else {
            /* ... */
        }
    }

    /* ... */
}
```
</details>

* **1.6.** 🔗 When writing a type for a property, constant, variable, a key for a dictionary, a function argument, a protocol conformance, or a superclass, **don't add a space before the colon**

<details>
<summary>Examples</summary>

```swift
// specifying type
let pirateViewController: PirateViewController

// dictionary syntax (note that we left-align as opposed to aligning colons)
let ninjaDictionary: [String: AnyObject] = [
    "fightLikeDairyFarmer": false,
    "disgusting": true
]

// declaring a function
func myFunction<T, U: SomeProtocol>(firstArgument: U, secondArgument: T) where T.RelatedType == U {
    /* ... */
}

// calling a function
someFunction(someArgument: "Kitten")

// superclasses
class PirateViewController: UIViewController {
    /* ... */
}

// protocols
extension PirateViewController: UITableViewDataSource {
    /* ... */
}
```
</details>

* **1.7.** 🔗 In general, there should be **a space following a comma**

<details>
<summary>Examples</summary>

```swift
let myArray = [1, 2, 3, 4, 5]
```
</details>

* **1.8.** 🔗 There should be **a space before and after a binary operator** such as `+`, `==`, or `->`. There should also not be a space after a `(` and before a `)`.

<details>
<summary>Examples</summary>

```swift
let myValue = 20 + (30 / 2) * 3
if 1 + 1 == 3 {
    fatalError("The universe is broken.")
}
func pancake(with syrup: Syrup) -> Pancake {
    /* ... */
}
```
</details>

* **1.9.** 🔗 We **follow Xcode's recommended indentation style**

* **1.10.** 🔗 When calling a function that has many parameters, put each argument on a separate line with a single extra indentation.

<details>
<summary>Examples</summary>

```swift
someFunctionWithManyArguments(
    firstArgument: "Hello, I am a string",
    secondArgument: resultFromSomeFunction(),
    thirdArgument: someOtherLocalProperty)
```
</details>

* **1.11.** ⚠️ When dealing with an implicit array or dictionary large enough to warrant splitting it into multiple lines, **treat the `[` and `]` as if they were braces in a method, `if` statement, etc.** Closures in a method should be treated similarly.

<details>
<summary>Examples</summary>

```swift
someFunctionWithABunchOfArguments(
    someStringArgument: "hello I am a string",
    someArrayArgument: [
        "dadada daaaa daaaa dadada daaaa daaaa dadada daaaa daaaa",
        "string one is crazy - what is it thinking?"
    ],
    someDictionaryArgument: [
        "dictionary key 1": "some value 1, but also some more text here",
        "dictionary key 2": "some value 2"
    ],
    someClosure: { parameter1 in
        print(parameter1)
    })
```
</details>

* **1.12.** ⚠️ Prefer using local constants or other mitigation techniques to **avoid multi-line predicates where possible**.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
let firstCondition = x == firstReallyReallyLongPredicateFunction()
let secondCondition = y == secondReallyReallyLongPredicateFunction()
let thirdCondition = z == thirdReallyReallyLongPredicateFunction()
if firstCondition && secondCondition && thirdCondition {
    // do something
}

// NOT PREFERRED
if x == firstReallyReallyLongPredicateFunction()
    && y == secondReallyReallyLongPredicateFunction()
    && z == thirdReallyReallyLongPredicateFunction() {
    // do something
}
```
</details>

## 2. Code Organization

* **2.1.** ⚠️ **Use extensions** to organize your code into logical blocks of functionality. Each extension should be set off with a `// MARK: - comment` to keep things well-organized.
* **2.2.** ⚠️ In particular, when adding protocol conformance to a model, **prefer adding a separate extension for the protocol methods**. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}

// NOT PREFERRED
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```
</details>

* **2.3.** ⚠️ **Unused (dead) code, including Xcode template code and placeholder comments should be removed**. An exception is when your tutorial or book instructs the user to use the commented code.

## 3. Naming

* **3.1.** ⚠️ Use the Swift naming conventions described in the [**API Design Guidelines**](https://swift.org/documentation/api-design-guidelines/).

<details>
<summary>Some key takeaways include</summary>

- striving for clarity at the call site
- prioritizing clarity over brevity
- using camel case (not snake case)
- using uppercase for types (and protocols), lowercase for everything else
- including all needed words while omitting needless words
- using names based on roles, not types
- sometimes compensating for weak type information
- striving for fluent usage
- beginning factory methods with `make`
- naming methods for their side effects
  - verb methods follow the -ed, -ing rule for the non-mutating version
  - noun methods follow the formX rule for the non-mutating version
  - boolean types should read like assertions
  - protocols that describe _what something is_ should read as nouns
  - protocols that describe _a capability_ should end in _-able_ or _-ible_
- using terms that don't surprise experts or confuse beginners
- generally avoiding abbreviations
- using precedent for names
- preferring methods and properties to free functions
- casing acronyms and initialisms uniformly up or down
- giving the same base name to methods that share the same meaning
- avoiding overloads on return type
- choosing good parameter names that serve as documentation
- labeling closure and tuple parameters
- taking advantage of default parameters

</details>

* **3.1.** ⚠️ There is **no need for Objective-C style prefixing in Swift** (e.g. use just `GuybrushThreepwood` instead of `LIGuybrushThreepwood`).

* **3.2.** ⚠️ **Type names:** Use [`PascalCase`](http://wiki.c2.com/?PascalCase) for type names (e.g. `struct`, `enum`, `class`, `protocol`, `typedef`, `associatedtype`, etc.).

* **3.3.** ⚠️ **Non-type names:** Use [`camelCase`](http://wiki.c2.com/?CamelCase) (initial lowercase letter) for non-type-names (function, method, property, constant, variable, argument names, enum cases, etc.).

* **3.4.** ⚠️ **Acronym:** When dealing with an acronym or other name that is usually written in all caps, actually use all caps in any names that use this in code. The exception is if this word is at the start of a name that needs to start with lowercase - in this case, use all lowercase for the acronym.

<details>
<summary>Examples</summary>

```swift
// "HTML" is at the start of a constant name, so we use lowercase "html"
let htmlBodyContent: String = "<p>Hello, World!</p>"
// Prefer using ID to Id
let profileID: Int = 1
// Prefer URLFinder to UrlFinder
class URLFinder {
    /* ... */
}
```
</details>

* **3.5.** ⚠️ **Constants:** All constants other than singletons that are instance-independent should be `static`. All such `static` constants should be placed in a container `enum` type as per [rule **4.16**](#4-coding-style). The naming of this container should be singular (e.g. `Constant` and not `Constants`) and it should be named such that it is relatively obvious that it is a constant container. If this is not obvious, you can add a `Constant` suffix to the name. You should use these containers to group constants that have similar or the same prefixes, suffixes and/or use cases.

<details>
<summary>Examples</summary>

```swift
class MyClassName {
    // PREFERRED
    enum AccessibilityIdentifier {
        static let pirateButton = "pirate_button"
    }
    enum SillyMathConstant {
        static let indianaPi = 3
    }
    static let shared = MyClassName()

    // NOT PREFERRED
    static let kPirateButtonAccessibilityIdentifier = "pirate_button"
    enum SillyMath {
        static let indianaPi = 3
    }
    enum Singleton {
        static let shared = MyClassName()
    }
}
```
</details>

* **3.6.** ⚠️ **Generics:** For generics and associated types, use either a single capital letter (`T`, `U`, `V`, `S`, etc.) or a `PascalCase` word that describes the generic. If this word clashes with a protocol that it conforms to or a superclass that it subclasses, you can append a `Type` suffix to the associated type or generic name.

<details>
<summary>Examples</summary>

```swift
class SomeClass<T> { /* ... */ }
class SomeClass<Model> { /* ... */ }
protocol Modelable {
    associatedtype Model
}
protocol Sequence {
    associatedtype IteratorType: Iterator
}
```
</details>

* **3.7.** ⚠️ Names should be descriptive and unambiguous.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
class RoundAnimatingButton: UIButton { /* ... */ }

// NOT PREFERRED
class CustomButton: UIButton { /* ... */ }
```
</details>

* **3.8.** ⚠️ Do not abbreviate, use shortened names, or single letter names.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
class RoundAnimatingButton: UIButton {
    let animationDuration: NSTimeInterval

    func startAnimating() {
        let firstSubview = subviews.first
    }

}

// NOT PREFERRED
class RoundAnimating: UIButton {
    let aniDur: NSTimeInterval

    func srtAnmating() {
        let v = subviews.first
    }
}
```
</details>

* **3.9.** ⚠️ Include type information in constant or variable names when it is not obvious otherwise.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
class ConnectionTableViewCell: UITableViewCell {
    let personImageView: UIImageView

    let animationDuration: TimeInterval

    // it is ok not to include string in the ivar name here because it's obvious
    // that it's a string from the property name
    let firstName: String

    // though not preferred, it is OK to use `Controller` instead of `ViewController`
    let popupController: UIViewController
    let popupViewController: UIViewController

    // when working with a subclass of `UIViewController` such as a table view
    // controller, collection view controller, split view controller, etc.,
    // fully indicate the type in the name.
    let popupTableViewController: UITableViewController

    // when working with outlets, make sure to specify the outlet type in the
    // property name.
    @IBOutlet weak var submitButton: UIButton!
    @IBOutlet weak var emailTextField: UITextField!
    @IBOutlet weak var nameLabel: UILabel!

}

// NOT PREFERRED
class ConnectionTableViewCell: UITableViewCell {
    // this isn't a `UIImage`, so shouldn't be called image
    // use personImageView instead
    let personImage: UIImageView

    // this isn't a `String`, so it should be `textLabel`
    let text: UILabel

    // `animation` is not clearly a time interval
    // use `animationDuration` or `animationTimeInterval` instead
    let animation: TimeInterval

    // this is not obviously a `String`
    // use `transitionText` or `transitionString` instead
    let transition: String

    // this is a view controller - not a view
    let popupView: UIViewController

    // as mentioned previously, we don't want to use abbreviations, so don't use
    // `VC` instead of `ViewController`
    let popupVC: UIViewController

    // even though this is still technically a `UIViewController`, this property
    // should indicate that we are working with a *Table* View Controller
    let popupViewController: UITableViewController

    // for the sake of consistency, we should put the type name at the end of the
    // property name and not at the start
    @IBOutlet weak var btnSubmit: UIButton!
    @IBOutlet weak var buttonSubmit: UIButton!

    // we should always have a type in the property name when dealing with outlets
    // for example, here, we should have `firstNameLabel` instead
    @IBOutlet weak var firstName: UILabel!
}
```
</details>

* **3.10.** ⚠️ When naming function arguments, make sure that the function can be read easily to understand the purpose of each argument.

* **3.11.** ⚠️ **Protocols:** As per [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/), a `protocol` should be named as nouns if they describe what something is doing (e.g. `Collection`) and using the suffixes `able`, `ible`, or `ing` if it describes a capability (e.g. `Equatable`, `ProgressReporting`). If neither of those options makes sense for your use case, you can add a `Protocol` suffix to the protocol's name as well. Some example `protocol`s are below.

<details>
<summary>Examples</summary>

```swift
// here, the name is a noun that describes what the protocol does
protocol TableViewSectionProvider {
    func rowHeight(at row: Int) -> CGFloat
    var numberOfRows: Int { get }
    /* ... */
}

// here, the protocol is a capability, and we name it appropriately
protocol Loggable {
    func logCurrentState()
    /* ... */
}

// suppose we have an `InputTextView` class, but we also want a protocol
// to generalize some of the functionality - it might be appropriate to
// use the `Protocol` suffix here
protocol InputTextViewProtocol {
    func sendTrackingEvent()
    func inputText() -> String
    /* ... */
}
```
</details>

* **3.12.** ⚠️ **Delegates:** When creating custom delegate methods, an unnamed first parameter should be the delegate source. (UIKit contains numerous examples of this.)

<details>
<summary>Examples</summary>

```swift
// PREFERRED
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool

// NOT PREFERRED
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```
</details>

## 4. General Coding Style

* **4.1.** ⚠️ Prefer `let` to `var` whenever possible.

* **4.2.** ⚠️ Prefer the composition of `map`, `filter`, `reduce`, etc. over iterating when transforming from one collection to another. Make sure to avoid using closures that have side effects when using these methods.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
let stringOfInts = [1, 2, 3].flatMap { String($0) }
// ["1", "2", "3"]

// NOT PREFERRED
var stringOfInts: [String] = []
for integer in [1, 2, 3] {
    stringOfInts.append(String(integer))
}

// PREFERRED
let evenNumbers = [4, 8, 15, 16, 23, 42].filter { $0 % 2 == 0 }
// [4, 8, 16, 42]

// NOT PREFERRED
var evenNumbers: [Int] = []
for integer in [4, 8, 15, 16, 23, 42] {
    if integer % 2 == 0 {
        evenNumbers.append(integer)
    }
}
```
</details>

* **4.3.** ⚠️ Prefer not declaring types for constants or variables if they can be inferred anyway.

* **4.4.** ⚠️ If a function returns multiple values, prefer returning a tuple to using `inout` arguments (it’s best to use labeled tuples for clarity on what you’re returning if it is not otherwise obvious). If you use a certain tuple more than once, consider using a `typealias`. If you’re returning 3 or more items in a tuple, consider using a `struct` or `class` instead.

<details>
<summary>Examples</summary>

```swift
func pirateName() -> (firstName: String, lastName: String) {
    return ("Guybrush", "Threepwood")
}

let name = pirateName()
let firstName = name.firstName
let lastName = name.lastName
```

</details>

* **4.5.** Be wary of retain cycles when creating delegates/protocols for your classes; typically, these properties should be declared `weak`.

* **4.6.** Be careful when calling `self` directly from an escaping closure as this can cause a retain cycle - use a [capture list](https://developer.apple.com/library/ios/documentation/swift/conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-XID_163) when this might be the case:

<details>
<summary>Examples</summary>

```swift
myFunctionWithEscapingClosure() { [weak self] (error) -> Void in
    // you can do this

    self?.doSomething()

    // or you can do this

    guard let strongSelf = self else {
        return
    }

    strongSelf.doSomething()
}
```

</details>

* **4.7.** Don't use labeled breaks.

* **4.8.** Don't place parentheses around control flow predicates.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
if x == y {
    /* ... */
}

// NOT PREFERRED
if (x == y) {
    /* ... */
}
```

</details>

* **4.9.** Avoid writing out an `enum` type where possible - use shorthand.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
imageView.setImageWithURL(url, type: .person)

// NOT PREFERRED
imageView.setImageWithURL(url, type: AsyncImageView.Type.person)
```

</details>

* **4.10.** Don’t use shorthand for class methods since it is generally more difficult to infer the context from class methods as opposed to `enum`s.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
imageView.backgroundColor = UIColor.white

// NOT PREFERRED
imageView.backgroundColor = .white
```

</details>

* **4.11.** Prefer not writing `self.` unless it is required.

* **4.12.** When writing methods, keep in mind whether the method is intended to be overridden or not. If not, mark it as `final`, though keep in mind that this will prevent the method from being overwritten for testing purposes. In general, `final` methods result in improved compilation times, so it is good to use this when applicable. Be particularly careful, however, when applying the `final` keyword in a library since it is non-trivial to change something to be non-`final` in a library as opposed to have changing something to be non-`final` in your local project.

* **4.13.** When using a statement such as `else`, `catch`, etc. that follows a block, put this keyword on the same line as the block. Again, we are following the [1TBS style](https://en.m.wikipedia.org/wiki/Indent_style#Variant:_1TBS) here. Example `if`/`else` and `do`/`catch` code is below.

<details>
<summary>Examples</summary>

```swift
if someBoolean {
    // do something
} else {
    // do something else
}

do {
    let fileContents = try readFile("filename.txt")
} catch {
    print(error)
}
```

</details>

* **4.14.** Prefer `static` to `class` when declaring a function or property that is associated with a class as opposed to an instance of that class. Only use `class` if you specifically need the functionality of overriding that function or property in a subclass, though consider using a `protocol` to achieve this instead.

* **4.15.** If you have a function that takes no arguments, has no side effects, and returns some object or value, prefer using a computed property instead.

* **4.16.** For the purpose of namespacing a set of `static` functions and/or `static` properties, prefer using a caseless `enum` over a `class` or a `struct`. The advantage of using a case-less enumeration is that it can't accidentally be instantiated and works as a pure namespace.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
enum Math {
    static let e = 2.718281828459045235360287
    static let root2 = 1.41421356237309504880168872
}

// NOT PREFERRED
let e = 2.718281828459045235360287  // pollutes global namespace
let root2 = 1.41421356237309504880168872

struct Math {
    static let e = 2.718281828459045235360287
    static let root2 = 1.41421356237309504880168872
}
```

</details>

## 5. Access Modifiers

* **5.1.** Write the access modifier keyword first if it is needed.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
private static let myPrivateNumber: Int

// NOT PREFERRED
static private let myPrivateNumber: Int
```

</details>

* **5.2.** The access modifier keyword should not be on a line by itself - keep it inline with what it is describing.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
open class Pirate {
    /* ... */
}

// NOT PREFERRED
open
class Pirate {
    /* ... */
}
```

</details>

* **5.3.** In general, do not write the `internal` access modifier keyword since it is the default.

* **5.4.** If a property needs to be accessed by unit tests, you will have to make it `internal` to use `@testable import ModuleName`. If a property *should* be private, but you declare it to be `internal` for the purposes of unit testing, make sure you add an appropriate bit of documentation commenting that explains this. You can make use of the `- warning:` markup syntax for clarity as shown below.

<details>
<summary>Examples</summary>

```swift
/**
 This property defines the pirate's name.
 - warning: Not `private` for `@testable`.
 */
let pirateName = "LeChuck"
```

</details>

* **5.5.** Prefer `private` to `fileprivate` where possible.

* **5.6.** When choosing between `public` and `open`, prefer `open` if you intend for something to be subclassable outside of a given module and `public` otherwise. Note that anything `internal` and above can be subclassed in tests by using `@testable import`, so this shouldn't be a reason to use `open`. In general, lean towards being a bit more liberal with using `open` when it comes to libraries, but a bit more conservative when it comes to modules in a codebase such as an app where it is easy to change things in multiple modules simultaneously.

## 6. Custom Operators

* **6.1.** **Prefer creating named functions to custom operators.**

If you want to introduce a custom operator, make sure that you have a *very* good reason why you want to introduce a new operator into global scope as opposed to using some other construct.

You can override existing operators to support new types (especially `==`). However, your new definitions must preserve the semantics of the operator. For example, `==` must always test equality and return a boolean.

## 7. Switch Statements and `enum`s

* **7.1.** When using a switch statement that has a finite set of possibilities (`enum`), do *NOT* include a `default` case. Instead, place unused cases at the bottom and use the `break` keyword to prevent execution.

* **7.2.** Since `switch` cases in Swift break by default, do not include the `break` keyword if it is not needed.

* **7.3.** The `case` statements should line up with the `switch` statement itself as per default Swift standards.

* **7.4.** When defining a case that has an associated value, make sure that this value is appropriately labeled as opposed to just types (e.g. `case hunger(hungerLevel: Int)` instead of `case hunger(Int)`).

<details>
<summary>Examples</summary>

```swift
enum Problem {
    case attitude
    case hair
    case hunger(hungerLevel: Int)
}

func handleProblem(problem: Problem) {
    switch problem {
    case .attitude:
        print("At least I don't have a hair problem.")
    case .hair:
        print("Your barber didn't know when to stop.")
    case .hunger(let hungerLevel):
        print("The hunger level is \(hungerLevel).")
    }
}
```

</details>

* **7.5.** Prefer lists of possibilities (e.g. `case 1, 2, 3:`) to using the `fallthrough` keyword where possible).

* **7.6.** If you have a default case that shouldn't be reached, preferably throw an error (or handle it some other similar way such as asserting).

<details>
<summary>Examples</summary>

```swift
func handleDigit(_ digit: Int) throws {
    switch digit {
    case 0, 1, 2, 3, 4, 5, 6, 7, 8, 9:
        print("Yes, \(digit) is a digit!")
    default:
        throw Error(message: "The given number was not a digit.")
    }
}
```

</details>

## 8. Optionals

* **8.1.** The only time you should be using implicitly unwrapped optionals is with `@IBOutlet`s. In every other case, it is better to use a non-optional or regular optional property. Yes, there are cases in which you can probably "guarantee" that the property will never be `nil` when used, but it is better to be safe and consistent. Similarly, don't use force unwraps.

* **8.2.** Don't use `as!` or `try!`.

* **8.3.** If you don't plan on actually using the value stored in an optional, but need to determine whether or not this value is `nil`, explicitly check this value against `nil` as opposed to using `if let` syntax.

<details>
<summary>Examples</summary>

```swift
// PREFERERED
if someOptional != nil {
    // do something
}

// NOT PREFERRED
if let _ = someOptional {
    // do something
}
```

</details>

* **8.4.** Don't use `unowned`. You can think of `unowned` as somewhat of an equivalent of a `weak` property that is implicitly unwrapped (though `unowned` has slight performance improvements on account of completely ignoring reference counting). Since we don't ever want to have implicit unwraps, we similarly don't want `unowned` properties.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
weak var parentViewController: UIViewController?

// NOT PREFERRED
weak var parentViewController: UIViewController!
unowned var parentViewController: UIViewController
```

</details>

* **8.5.** When unwrapping optionals, use the same name for the unwrapped constant or variable where appropriate.

<details>
<summary>Examples</summary>

```swift
guard let myValue = myValue else {
    return
}
```

</details>

## 9. Protocols

When implementing protocols, there are two ways of organizing your code:

1. Using `// MARK:` comments to separate your protocol implementation from the rest of your code
2. Using an extension outside your `class`/`struct` implementation code, but in the same source file

Keep in mind that when using an extension, however, the methods in the extension can't be overridden by a subclass, which can make testing difficult. If this is a common use case, it might be better to stick with method #1 for consistency. Otherwise, method #2 allows for cleaner separation of concerns.

Even when using method #2, add `// MARK:` statements anyway for easier readability in Xcode's method/property/class/etc. list UI.

## 10. Properties

* **10.1.** If making a read-only, computed property, provide the getter without the `get {}` around it.

<details>
<summary>Examples</summary>

```swift
var computedProperty: String {
    if someBool {
        return "I'm a mighty pirate!"
    }
    return "I'm selling these fine leather jackets."
}
```

</details>

* **10.2.** When using `get {}`, `set {}`, `willSet`, and `didSet`, indent these blocks.
* **10.3.** Though you can create a custom name for the new or old value for `willSet`/`didSet` and `set`, use the standard `newValue`/`oldValue` identifiers that are provided by default.

<details>
<summary>Examples</summary>

```swift
var storedProperty: String = "I'm selling these fine leather jackets." {
    willSet {
        print("will set to \(newValue)")
    }
    didSet {
        print("did set from \(oldValue) to \(storedProperty)")
    }
}

var computedProperty: String  {
    get {
        if someBool {
            return "I'm a mighty pirate!"
        }
        return storedProperty
    }
    set {
        storedProperty = newValue
    }
}
```

</details>

* **10.4.** You can declare a singleton property as follows:

<details>
<summary>Examples</summary>

```swift
class PirateManager {
    static let shared = PirateManager()

    /* ... */
}
```

</details>

## 11. Closures

* **11.1.** If the types of the parameters are obvious, it is OK to omit the type name, but being explicit is also OK. Sometimes readability is enhanced by adding clarifying detail and sometimes by taking repetitive parts away - use your best judgment and be consistent.

<details>
<summary>Examples</summary>

```swift
// omitting the type
doSomethingWithClosure() { response in
    print(response)
}

// explicit type
doSomethingWithClosure() { response: NSURLResponse in
    print(response)
}

// using shorthand in a map statement
[1, 2, 3].flatMap { String($0) }
```

</details>

* **11.2.** If specifying a closure as a type, you don’t need to wrap it in parentheses unless it is required (e.g. if the type is optional or the closure is within another closure). Always wrap the arguments in the closure in a set of parentheses - use `()` to indicate no arguments and use `Void` to indicate that nothing is returned.

<details>
<summary>Examples</summary>

```swift
let completionBlock: (Bool) -> Void = { (success) in
    print("Success? \(success)")
}

let completionBlock: () -> Void = {
    print("Completed!")
}

let completionBlock: (() -> Void)? = nil
```

</details>

* **11.3.** Keep parameter names on same line as the opening brace for closures when possible without too much horizontal overflow (i.e. ensure lines are less than 160 characters).

* **11.4.** Use trailing closure syntax unless the meaning of the closure is not obvious without the parameter name (an example of this could be if a method has parameters for success and failure closures).

<details>
<summary>Examples</summary>

```swift
// trailing closure
doSomething(1.0) { (parameter1) in
    print("Parameter 1 is \(parameter1)")
}

// no trailing closure
doSomething(1.0, success: { (parameter1) in
    print("Success with \(parameter1)")
}, failure: { (parameter1) in
    print("Failure with \(parameter1)")
})
```

</details>

## 12. Arrays

* **12.1.** In general, avoid accessing an array directly with subscripts. When possible, use accessors such as `.first` or `.last`, which are optional and won’t crash. Prefer using a `for item in items` syntax when possible as opposed to something like `for i in 0 ..< items.count`. If you need to access an array subscript directly, make sure to do proper bounds checking. You can use `for (index, value) in items.enumerated()` to get both the index and the value.

* **12.2.** Never use the `+=` or `+` operator to append/concatenate to arrays. Instead, use `.append()` or `.append(contentsOf:)` as these are far more performant (at least with respect to compilation) in Swift's current state. If you are declaring an array that is based on other arrays and want to keep it immutable, instead of `let myNewArray = arr1 + arr2`, use `let myNewArray = [arr1, arr2].flatten()`.

## 13. Error Handling

Suppose a function `myFunction` is supposed to return a `String`, however, at some point it can run into an error. A common approach is to have this function return an optional `String?` where we return `nil` if something went wrong.

<details>
<summary>Examples</summary>

```swift
func readFile(named filename: String) -> String? {
    guard let file = openFile(named: filename) else {
        return nil
    }

    let fileContents = file.read()
    file.close()
    return fileContents
}

func printSomeFile() {
    let filename = "somefile.txt"
    guard let fileContents = readFile(named: filename) else {
        print("Unable to open file \(filename).")
        return
    }
    print(fileContents)
}
```

</details>

Instead, we should be using Swift's `try`/`catch` behavior when it is appropriate to know the reason for the failure.

You can use a `struct` such as the following:

<details>
<summary>Show codes</summary>

```swift
struct Error: Swift.Error {
    public let file: StaticString
    public let function: StaticString
    public let line: UInt
    public let message: String

    public init(message: String, file: StaticString = #file, function: StaticString = #function, line: UInt = #line) {
        self.file = file
        self.function = function
        self.line = line
        self.message = message
    }
}
```

</details>

<details>
<summary>Example usage</summary>

```swift
func readFile(named filename: String) throws -> String {
    guard let file = openFile(named: filename) else {
        throw Error(message: "Unable to open file named \(filename).")
    }

    let fileContents = file.read()
    file.close()
    return fileContents
}

func printSomeFile() {
    do {
        let fileContents = try readFile(named: filename)
        print(fileContents)
    } catch {
        print(error)
    }
}
```

</details>

There are some exceptions in which it does make sense to use an optional as opposed to error handling. When the result should *semantically* potentially be `nil` as opposed to something going wrong while retrieving the result, it makes sense to return an optional instead of using error handling.

In general, if a method can "fail", and the reason for the failure is not immediately obvious if using an optional return type, it probably makes sense for the method to throw an error.

## 14. Using `guard` Statements

* **14.1.** In general, we prefer to use an "early return" strategy where applicable as opposed to nesting code in `if` statements. Using `guard` statements for this use-case is often helpful and can improve the readability of the code.
<details>
<summary>Examples</summary>

```swift
// PREFERRED
func eatDoughnut(at index: Int) {
    guard index >= 0 && index < doughnuts.count else {
        // return early because the index is out of bounds
        return
    }

    let doughnut = doughnuts[index]
    eat(doughnut)
}

// NOT PREFERRED
func eatDoughnut(at index: Int) {
    if index >= 0 && index < doughnuts.count {
        let doughnut = doughnuts[index]
        eat(doughnut)
    }
}
```

</details>

* **14.2.** When unwrapping optionals, prefer `guard` statements as opposed to `if` statements to decrease the amount of nested indentation in your code.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
guard let monkeyIsland = monkeyIsland else {
    return
}
bookVacation(on: monkeyIsland)
bragAboutVacation(at: monkeyIsland)

// NOT PREFERRED
if let monkeyIsland = monkeyIsland {
    bookVacation(on: monkeyIsland)
    bragAboutVacation(at: monkeyIsland)
}

// EVEN LESS PREFERRED
if monkeyIsland == nil {
    return
}
bookVacation(on: monkeyIsland!)
bragAboutVacation(at: monkeyIsland!)
```

</details>

* **14.3.** When deciding between using an `if` statement or a `guard` statement when unwrapping optionals is *not* involved, the most important thing to keep in mind is the readability of the code. There are many possible cases here, such as depending on two different booleans, a complicated logical statement involving multiple comparisons, etc., so in general, use your best judgement to write code that is readable and consistent. If you are unsure whether `guard` or `if` is more readable or they seem equally readable, prefer using `guard`.

<details>
<summary>Examples</summary>

```swift
// an `if` statement is readable here
if operationFailed {
    return
}

// a `guard` statement is readable here
guard isSuccessful else {
    return
}

// double negative logic like this can get hard to read - i.e. don't do this
guard !operationFailed else {
    return
}
```

</details>

* **14.4.** If choosing between two different states, it makes more sense to use an `if` statement as opposed to a `guard` statement.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
if isFriendly {
    print("Hello, nice to meet you!")
} else {
    print("You have the manners of a beggar.")
}

// NOT PREFERRED
guard isFriendly else {
    print("You have the manners of a beggar.")
    return
}

print("Hello, nice to meet you!")
```

</details>

* **14.5.** You should also use `guard` only if a failure should result in exiting the current context. Below is an example in which it makes more sense to use two `if` statements instead of using two `guard`s - we have two unrelated conditions that should not block one another.

<details>
<summary>Examples</summary>

```swift
if let monkeyIsland = monkeyIsland {
    bookVacation(onIsland: monkeyIsland)
}

if let woodchuck = woodchuck, canChuckWood(woodchuck) {
    woodchuck.chuckWood()
}
```

</details>


* **14.6.** Often, we can run into a situation in which we need to unwrap multiple optionals using `guard` statements. In general, combine unwraps into a single `guard` statement if handling the failure of each unwrap is identical (e.g. just a `return`, `break`, `continue`, `throw`, or some other `@noescape`).

<details>
<summary>Examples</summary>

```swift
// combined because we just return
guard let thingOne = thingOne,
    let thingTwo = thingTwo,
    let thingThree = thingThree else {
    return
}

// separate statements because we handle a specific error in each case
guard let thingOne = thingOne else {
    throw Error(message: "Unwrapping thingOne failed.")
}

guard let thingTwo = thingTwo else {
    throw Error(message: "Unwrapping thingTwo failed.")
}

guard let thingThree = thingThree else {
    throw Error(message: "Unwrapping thingThree failed.")
}
```

</details>

* **14.7.** Don’t use one-liners for `guard` statements.

<details>
<summary>Examples</summary>

```swift
// PREFERRED
guard let thingOne = thingOne else {
    return
}

// NOT PREFERRED
guard let thingOne = thingOne else { return }
```

</details>

## 15. Documentation/Comments

If a function is more complicated than a simple O(1) operation, you should generally consider adding a doc comment for the function since there could be some information that the method signature does not make immediately obvious. If there are any quirks to the way that something was implemented, whether technically interesting, tricky, not obvious, etc., this should be documented. Documentation should be added for complex classes/structs/enums/protocols and properties. All `public` functions/classes/properties/constants/structs/enums/protocols/etc. should be documented as well (provided, again, that their signature/name does not make their meaning/functionality immediately obvious).

After writing a doc comment, you should option click the function/property/class/etc. to make sure that everything is formatted correctly.

Be sure to check out the full set of features available in Swift's comment markup [described in Apple's Documentation](https://developer.apple.com/library/tvos/documentation/Xcode/Reference/xcode_markup_formatting_ref/Attention.html#//apple_ref/doc/uid/TP40016497-CH29-SW1).

Guidelines:

* **15.1.** 160 character column limit (like the rest of the code).

* **15.2.** Even if the doc comment takes up one line, use block (`/** */`).

* **15.3.** Do not prefix each additional line with a `*`.

* **15.4.** Use the new `- parameter` syntax as opposed to the old `:param:` syntax (make sure to use lower case `parameter` and not `Parameter`). See [the documentation on Swift Markup](https://developer.apple.com/library/watchos/documentation/Xcode/Reference/xcode_markup_formatting_ref/) for more details on how this is formatted.

* **15.5.** If you’re going to be documenting the parameters/returns/throws of a method, document all of them, even if some of the documentation ends up being somewhat repetitive (this is preferable to having the documentation look incomplete). Sometimes, if only a single parameter warrants documentation, it might be better to just mention it in the description instead.

* **15.6.** For complicated classes, describe the usage of the class with some potential examples as seems appropriate. Remember that markdown syntax is valid in Swift's comment docs. Newlines, lists, etc. are therefore appropriate.

<details>
<summary>Examples</summary>

```swift
/**
 ## Feature Support

 This class does some awesome things. It supports:

 - Feature 1
 - Feature 2
 - Feature 3

 ## Examples

 Here is an example use case indented by four spaces because that indicates a
 code block:

     let myAwesomeThing = MyAwesomeClass()
     myAwesomeThing.makeMoney()

 ## Warnings

 There are some things you should be careful of:

 1. Thing one
 2. Thing two
 3. Thing three
 */
class MyAwesomeClass {
    /* ... */
}
```

</details>

* **15.7.** When mentioning code, use code ticks - \`

<details>
<summary>Examples</summary>

```swift
/**
 This does something with a `UIViewController`, perchance.
 - warning: Make sure that `someValue` is `true` before running this function.
 */
func myFunction() {
    /* ... */
}
```

</details>

* **15.8.** When writing doc comments, prefer brevity where possible.

* **15.9.** Always leave a space after `//`.
* **15.10.** Always leave comments on their own line.
* **15.11.** When using `// MARK: - whatever`, leave a newline after the comment.

<details>
<summary>Examples</summary>

```swift
class Pirate {

    // MARK: - instance properties

    private let pirateName: String

    // MARK: - initialization

    init() {
        /* ... */
    }

}
```

</details>

# Other Guides

* [LinkedIn's Swift Style Guide](https://github.com/linkedin/swift-style-guide)
* [GitHub's Swift Style Guide](https://github.com/github/swift-style-guide)
* [Ray Wenderlich's Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)
