# Intelygenz Swift Style Guide

This style guide outlines the coding conventions of the iOS team at Intelygenz. This document is based on a list of documents with a few modifications:

* [The Official raywenderlich.com Swift Style Guide](<https://github.com/raywenderlich/swift-style-guide>)
* [Linkedin Swift Style Guide](https://github.com/linkedin/swift-style-guide)
* [NYTimes Objective-C Style Guide](https://github.com/NYTimes/objective-c-style-guide)
* [Github Swift Style Guide](https://github.com/github/swift-style-guide)

This is a living document and because of that it will be changing over the time with whichever thing we agree as a team in the interest of a unified, practical and simple coding style.


## Table of Contents##

* [Correctness](#correctness)
* [Naming](#naming)
 * [Class Prefixes](#class-prefixes)
 * [Class and Variable Names](#class-and-variable-names)
 * [Capitalization](#capitalization)
 * [Methods](#methods)
 * [Function Prose](#function-prose)
 * [Protocols](#protocols)
 * [Delegates](#delegates)
 * [Type Information](#type-information)
 * [Type Inferred Context](#type-inferred-context)
 * [Constants](#constants)
 * [Booleans](#booleans)
 * [Generics](#generics)
 * [Free Functions](#free-functions)
 * [Language](#language)
 * [Others](#others)
* [Code Organization](#code-organization)
 * [Protocol Conformance](#protocol-conformance)
 * [Unused Code](#unused-code)
 * [Minimal Imports](#minimal-imports)
* [Spacing](#spacing)
* [Comments](#comments)
* [Classes and Structures](#classes-and-structures)
 * [Which one to use?](#which-one-to-use)
 * [Use of self](#use-of-self)
 * [Make classes finalby default](#make-classes-finalby-default)
* [Properties](#properties)
 * [Computed Properties](#computed-properties)
 * [Property Observers](#property-observers)
 * [Singleton Properties](#singleton-properties)
 * [Lazy Initialization](#lazy-initialization)
* [Optionals](#optionals)
* [Access Modifiers](#access-modifiers)
* [Custom Operators](#custom-operators)
* [Switch Statements and Enums](#switch-statements-and-enums)
* [Closure Expressions](#closure-expressions)
* [Arrays](#arrays)
* [Error Handling](#error-handling)
* [Golden Path](#golden-path)
* [Image Naming](#image-naming)
* [References](#references)

## Correctness
Strive to make your code compile without warnings. This rule informs many style decisions such as using `#selector` types instead of string literals.


## Naming
Descriptive and consistent naming makes software easier to read and understand. Use the Swift naming conventions described in the [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/).

### Class Prefixes
Swift types are automatically namespaced by the module that contains them and you should not add a class prefix such as `MyModule`. 

**Prefered**

```swift
class MyClass {

}
```

**Not Prefered**

```swift
class MyModule.MyClass {

}
```

If two names from different modules collide you can disambiguate by prefixing the type name with the module name. However, only specify the module name when there is possibility for confusion which should be rare.

```swift
import SomeModule

let myInstance = MyModule.UsefulClass()
```

### Class and Variable Names

Names should be descriptive and unambiguous.

**Preferred**

```swift
class RoundAnimatingButton: UIButton {
    /* ... */ 
}
```

 **Not Preferred**

 ```swift
 class CustomButton: UIButton { 
     /* ... */ 
 }
 ```

When naming a variable prioritize clarity over brevity. Include all needed words while omitting needless words. Do not abbreviate, use shortened names, or single letter names.

**Preferred**

```swift
class RoundAnimatingButton: UIButton {
    let animationDuration: NSTimeInterval

    func startAnimating() {
        let firstSubview = subviews.first
    }
}
```

 **Not Preferred**

 ```swift
 class RoundAnimating: UIButton {
     let aniDur: NSTimeInterval

     func srtAnmating() {
         let v = subviews.first
     }
 }
 ```

* Use terms that don't surprise experts or confuse beginners
* Use names based on roles, not types. Types are not necessary to be included as postfix name if it is implicit on variable type. 

> **Example 1:** Use `now: Date` instead of `nowDate: Date`.

> **Example 2:** If you have a variable that count the number of seconds of an animation the variable animation does not include all the information. Use `let animationDuration: NSTimeInterval` instead of `let animationTimeInterval: NSTimeInterval`

### Capitalization
Naming capitalizing must follow the following convention:

* `lowercase`  
Used for language reserved words that specify the kind of the type (e.g. `struct`, `enum`, `class`, `typedef`, `associatedtype`, etc.)

* `PascalCase`  
First letter of every word in uppercase. Used for type names (class, protocols, enums...).

* `camelCase`  
Initial lowercase letter. Then first letter of every word in uppercase. Used when naming a function, method, property, constant, variable, argument names, enum cases, etc.

When dealing with an acronym or other name that is usually written in all caps, actually use all caps in any names that use this in code. The exception is if this word is at the start of a name that needs to start with lowercase - in this case, use all lowercase for the acronym.

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

### Methods
* Begin factory methods with make
* Name methods for their side effects
* Verb methods follow the -ed, -ing rule for the non-mutating version

**Example**

| Mutating | Non-mutating |
| -------- | ------------ |
| `x.sort()` | `z = x.sorted()` |
| `x.append(y)` | `z = x.appending(y)` |
 
* Noun methods follow the formX rule for the mutating version

**Example**

| Non-mutating | Mutating |
| ------------ | -------- |
| `x = y.union(z)`	| `y.formUnion(z)` |
| `j = c.successor(i)` |	`c.formSuccessor(&i)` |

* Give the same base name to methods that share the same meaning
* Choose good parameter names that serve as documentation

### Function Prose
When naming function arguments, make sure that the function can be read easily to understand the purpose of each argument.

When referring to methods in prose, being unambiguous is critical. To refer to a method name, use the simplest form possible.

1. Write the method name with no parameters. Example: Next, you need to call the method `addTarget`.
2. Write the method name with argument labels. Example: Next, you need to call the method `addTarget(_:action:)`.
3. Write the full method name with argument labels and types. Example: Next, you need to call the method `addTarget(_: Any?, action: Selector?)`.

For the above example, when referring to `UIGestureRecognizer`, option 1 is unambiguous and preferred.

### Protocols
* Protocols that describe what something is should read as nouns
* Protocols that describe a capability should end in `-able` or `-ible`

### Delegates
When creating custom delegate methods, an unnamed first parameter should be the delegate source. (UIKit contains numerous examples of this.)

**Preferred**

```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

**Not Preferred**

```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### Type Information
Include type information in constant or variable names when it is not obvious otherwise.

**Preferred**

```swift
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
    @IBOutlet weak private var submitButton: UIButton!
    @IBOutlet weak private var emailTextField: UITextField!
    @IBOutlet weak private var nameLabel: UILabel!
}
```

**Not Preferred**

```swift
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
    @IBOutlet weak private var btnSubmit: UIButton!
    @IBOutlet weak private var buttonSubmit: UIButton!

    // we should always have a type in the property name when dealing with outlets
    // for example, here, we should have `firstNameLabel` instead
    @IBOutlet weak private var firstName: UILabel!
}
```

### Type Inferred Context
Use compiler inferred context to write shorter, clear code. (Also see Type Inference.)

**Preferred**

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

**Not Preferred**

```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### Constants
Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

> **Tip:** A good technique is to define everything using let and only change it to var if the compiler complains!

All constants other than singletons that are instance-independent should be `static`. All such `static` constants should be placed in a container `struct` type. The naming of this container should be singular (e.g. `Constant` and not `Constants`) and it should be named such that it is relatively obvious that it is a constant container. If this is not obvious, you can add a `Constant` suffix to the name. You should use these containers to group constants that have similar or the same prefixes, suffixes and/or use cases. Constants should be agrupped by meaning using recursive structs.

**Preferred**

```swift
class MyCustomView {
	struct AccessibilityIdentifier {
        static let sendButton = "send_button"
    }
    struct Color {
        static let sky = UIColor.blue
        static let dark = UIColor.black
    }
    struct Animation {
        struct Time {
            static let fadeIn = 4.0
            static let fadeOut = 2.5
        }
        struct Position {
            struct Start {
                static let x = 0.0
                static let y = 0.0
            }
            struct End {
                static let x = 20.0
                static let y = 10.0
            }
        }
    }
}
```

**Not Preferred**

```swift
static let kSendButtonAccessibilityIdentifier = "send_button"

struct Colors {
    static let kSkyColor = UIColor.blue
    static let kDarkColor = UIColor.black
}

struct AnimationTimes {
    static let fadeInAnimationTime = 4.0
    static let fadeOutAnimationTime = 2.5
}

struct PositionStart {
    static let x = 0.0
    static let y = 0.0
}

struct PositionEnd {
    static let x = 20.0
    static let y = 10.0
}
```



### Booleans
Boolean types should read like assertions. Boolean variable name should be an adjective when possible and should omit the `is` prefix.

**Preferred**

```swift
var editable = false
```

**Not Preferred**

```swift
var isEditable = false
```

### Generics
Generic type parameters should be descriptive, upper camel case names. If generic type conforms a protocol use the first letter of the protocol name. For example `C` for `Codable` or `D` for `Decodable`. When a type name doesn't have a meaningful relationship or role, use a traditional single uppercase letter such as `T`, `U`, or `V`. 

**Preferred**

```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

**Not Preferred**

```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### Free Functions
Free functions, which aren't attached to a class or type, should be used sparingly. When possible, prefer to use a method instead of a free function. This aids in readability and discoverability.

Free functions are most appropriate when they aren't associated with any particular type or instance.

**Preferred**

```swift
let sorted = items.mergeSorted()  // easily discoverable
rocket.launch()  // acts on the model
```

**Not Preferred**

```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

**Free Function Exceptions**

```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x, y, z)  // another free function that feels natural
```

### Language
Use US English spelling to match Apple's API.

**Preferred**

```swift
let color = "red"
```

**Not Preferred**

```swift
let colour = "red"
```

### Others
* Using precedent for names
* Avoiding overloads on return type
* Labeling closure and tuple parameters
* Taking advantage of default parameters

## Code Organization
Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a `// MARK: - comment` to keep things well-organized.

### Protocol Conformance
In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

**Preferred**

```swift
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
```

**Not Preferred**

```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

Since the compiler does not allow you to re-declare protocol conformance in a derived class, it is not always required to replicate the extension groups of the base class. This is especially true if the derived class is a terminal class and a small number of methods are being overridden. When to preserve the extension groups is left to the discretion of the author.

For UIKit view controllers, consider grouping lifecycle, custom accessors, and IBAction in separate class extensions within the same file.

> **Exception:** Objective-C protocols cannot be implemented inside extensions for generic classes.

### Unused Code
Unused (dead) code, including Xcode template code and placeholder comments should be removed. An exception is when your tutorial or book instructs the user to use the commented code.

Aspirational methods not directly associated with the tutorial whose implementation simply calls the superclass should also be removed. This includes any empty/unused UIApplicationDelegate methods.

**Preferred**

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

**Not Preferred**

```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}
```

### Minimal Imports
Keep imports minimal. For example, don't import `UIKit` when importing `Foundation` will suffice.


## Spacing
* Identation MUST use 4 spaces. Never indent with tabs. Be sure to set this preferences in Xcode

 > **Tip:** You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or _Editor > Structure > Re-Indent_ in the menu).

* Long lines should be wrapped at around 140 characters.

 > **Tip** You can set a Page Guide on Xcode (_Preferences > Text Editing > Page guide at column: 140_)

* Avoid trailing whitespaces at the end of lines.
* Add a single newline character at the end of each line.
* Add a single newline at the end of every file.
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a methods often means you should refactor in several methods.
* By convention, Swift embraces the **"One True Brace Style"** or [**1TBS**](https://en.m.wikipedia.org/wiki/Indentation_style#1TBS) as its standard for laying out code. 1TBS adopts the following features
 * Opening braces appear at the end of the statement that establishes a clause
 * Else statements appear between paired close and open braces
 * All clauses are braced. Single-line clauses use mandatory bracing, just like multi-line clauses. Bracing ensures that all code line insertions will be safely added, and cannot break the intended flow. 

 **Preferred:**

 ```swift
 if user.isHappy {
       // Do something
 } else {
       // Do something else
 }
 ```

 **Not Preferred**

 ```swift
 if user.isHappy
 {
       // Do something
 }
 else {
       // Do something else
 }
 ```

* Colons always have no space on the left and one space on the right. Exceptions are the ternary operator `? :`, empty dictionary `[:]` and `#selector` syntax for unnamed parameters `(_:)`.

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
 
* Don't use labeled breaks.

* Don't place parentheses around control flow predicates.

 **Preferred**
 
 ```swift
 if x == y {
        /* ... */
 }
 ```

 **Not Preferred**
 
 ```swift
 if (x == y) {
        /* ... */
 }
 ```

* In general, there should be a spacing following a comma. 

 ```swift
 let myArray = [1, 2, 3, 4, 5]
 ```

* There should be a space before and after a binary operator such as `+`, `==`, or `->`. There should also not be a space after a `(` and before a `)`.

 ```swift
 let myValue = 20 + (30 / 2) * 3
 if 1 + 1 == 3 {
        fatalError("The universe is broken.")
 }
 func pancake(with syrup: Syrup) -> Pancake {
        /* ... */
 }
 ```

* When declaring a function that spans multiple lines, prefer using that syntax to which Xcode, as of version 7.3, defaults.

 ```swift
 // Xcode indentation for a function declaration that spans multiple lines
 func myFunctionWithManyParameters(parameterOne: String,
                                   parameterTwo: String,
                                   parameterThree: String) {
     // Xcode indents to here for this kind of statement
     print("\(parameterOne) \(parameterTwo) \(parameterThree)")
 }

 // Xcode indentation for a multi-line `if` statement
 if myFirstValue > (mySecondValue + myThirdValue)
     && myFourthValue == .someEnumValue {

     // Xcode indents to here for this kind of statement
     print("Hello, World!")
 }
  ```

* When calling a function that has many parameters, put each argument on a separate line with a single extra indentation.

```swift
someFunctionWithManyArguments(
     firstArgument: "Hello, I am a string",
     secondArgument: resultFromSomeFunction(),
     thirdArgument: someOtherLocalProperty)
```

* When dealing with an implicit array or dictionary large enough to warrant splitting it into multiple lines, treat the [ and ] as if they were braces in a method, if statement, etc. Closures in a method should be treated similarly.

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

* Prefer using local constants or other mitigation techniques to avoid multi-line predicates where possible.

**Preferred**

```swift
let firstCondition = x == firstReallyReallyLongPredicateFunction()
let secondCondition = y == secondReallyReallyLongPredicateFunction()
let thirdCondition = z == thirdReallyReallyLongPredicateFunction()
if firstCondition && secondCondition && thirdCondition {
    // do something
}
```

**Not Preferred**

```swift
if x == firstReallyReallyLongPredicateFunction()
    && y == secondReallyReallyLongPredicateFunction()
    && z == thirdReallyReallyLongPredicateFunction() {
    // do something
}
```

> Note that in this example the meaning of the code changes, in the first example the three conditions are always evaluated while in the second one they are lazily evaluated. This is specially important when dealing with optionals, so many times it is not possible to refactor to local constants and it is preferred to refactor to an early exit pattern splitting a long if into multiple `guard` clauses, as shown in [Golden Path](#golden-path)


## Comments
* Code should be self-explanatory therefore commenting code should be avoided as much as possible.
* A comment should explain **why** shomething is done instead of **how** is done.
* Comments must be kept up-to-date or deleted.
* If there are any quirks to the way that something was implemented, whether technically interesting, tricky, not obvious, etc., this should be documented. 
* Documentation should be added for complex classes/structs/enums/protocols and proterties.
* All `public` functions/classes/properties/constants/structs/enums/protocols/etc. should be documented as well (provided that their signature/name does not make their meaning/funtionality inmediately obvious).

> **Tip 1:** After writing a doc comment, you should option click the function/property/class/etc. to make sure that everything is formatted correctly.

> **Tip 2:** Be sure to check out the full set of features available in Swift's comment markup described in Apple's Documentation.

* Set 140 characters as column limit for coments (like the rest of the code).
* Use simple single line comments as far as possible. Avoid block comments inline with code. _Exception: This does not apply to those commentes used to generate documentation_
* Even if the doc comment takes up one line, use block (`/** */`).
* Do not prefix each additional line with a `*`.
* Use the new `- parameter` syntax as opposed to the old `:param:` syntax (make sure to use lower case `parameter` and not `Parameter`). Option-click on a method you wrote to make sure the quick help looks correct.

```swift
class Human {
    /**
     This method feeds a certain food to a person.

     - parameter food: The food you want to be eaten.
     - parameter person: The person who should eat the food.
     - returns: True if the food was eaten by the person; false otherwise.
    */
    func feed(_ food: Food, to person: Human) -> Bool {
        // ...
    }
}
```

* If you’re going to be documenting the parameters/returns/throws of a method, document all of them, even if some of the documentation ends up being somewhat repetitive (this is preferable to having the documentation look incomplete). Sometimes, if only a single parameter warrants documentation, it might be better to just mention it in the description instead.
* For complicated classes, describe the usage of the class with some potential examples as seems appropriate. Remember that markdown syntax is valid in Swift's comment docs. Newlines, lists, etc. are therefore appropriate.

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

* When writing doc comments, prefer brevity where possible
* Always leave a space after `//`.
* Always leave comments on their own line.
* When using `// MARK: - whatever`, leave a newline after the comment.

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

## Classes and Structures

### Which one to use?
Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Generally use structs for the model and classes for the rest.

Sometimes, things should be structs but need to conform to `AnyObject` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

Unless you require functionality that can only be provided by a class (like identity or deinitializers), implement a struct instead.

Note that inheritance is (by itself) usually not a good reason to use classes, because polymorphism can be provided by protocols, and implementation reuse can be provided through composition.

For example, this class hierarchy:

```swift
class Vehicle {
    let numberOfWheels: Int

    init(numberOfWheels: Int) {
        self.numberOfWheels = numberOfWheels
    }

    func maximumTotalTirePressure(pressurePerWheel: Float) -> Float {
        return pressurePerWheel * Float(numberOfWheels)
    }
}

class Bicycle: Vehicle {
    init() {
        super.init(numberOfWheels: 2)
    }
}

class Car: Vehicle {
    init() {
        super.init(numberOfWheels: 4)
    }
}
```

could be refactored into these definitions:

```swift
protocol Vehicle {
    var numberOfWheels: Int { get }
}

extension Vehicle {
    func maximumTotalTirePressure(vehicle: Vehicle, pressurePerWheel: Float) -> Float {
        return pressurePerWheel * Float(vehicle.numberOfWheels)
    }
}

struct Bicycle: Vehicle {
    let numberOfWheels = 2
}

struct Car: Vehicle {
    let numberOfWheels = 4
}
```

Value types are simpler, easier to reason about, and behave as expected with the let keyword.

### Use of `self`
Avoid writing `.self` unless it is requiered. Use self only when required by the compiler (in `@escaping` closures, or in initializers to disambiguate properties from arguments). In other words, if it compiles without `self` then omit it.


### Make classes `final`by default
Classes should start as final, and only be changed to allow subclassing if a valid need for inheritance has been identified. Even in that case, as many definitions as possible within the class should be final as well, following the same rules. Marking classes as final also improve compilation times.

## Properties

### Computed Properties
If making a read-only, computed property, omit the get clause. The get clause is required only when a set clause is provided.

**Preferred**

```swift
var diameter: Double {
    return radius * 2
}
```

**Not Preferred**

```swift
var diameter: Double {
    get {
        return radius * 2
    }
}
```

### Property Observers
* Use `willSet` and `didSet` very carefully. Property observers related code should be simple and do not execute complex code that could have unexpected side-effects.

```swift
var model: MyModel {
    didSet {
        table.reloadData()
    }
}
```

* Though you can create a custom name for the new or old value for `willSet`/`didSet` and set, use the standard newValue/oldValue identifiers that are provided by default.

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

* When using `get {}`, `set {}`, `willSet`, and `didSet`, always indent these blocks.

### Singleton Properties
You can declare a singleton property as follows:

```swift
class PirateManager {
    static let shared = PirateManager()

    /* ... */
}
```

### Lazy Initialization
Consider using lazy initialization for finer grain control over object lifetime. This is especially true for `UIViewController` that loads views lazily. You can either use a closure that is immediately called `{ }()` or call a private factory method.

```swift
lazy var locationManager = self.makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
  let manager = CLLocationManager()
  manager.desiredAccuracy = kCLLocationAccuracyBest
  manager.delegate = self
  manager.requestAlwaysAuthorization()
  return manager
}
```

> `[unowned self]` is not required here.  
> 
> A retain cycle is not created.
Location manager has a side-effect for popping up UI to ask the user for permission so fine grain control makes sense here.

## Optionals
* Declare variables and funcion return types as optional with `?` where a nil value is acceptable.

* When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

* The only time you should be using implicitly unwrapped optionals is with `@IBOutlets`. In every other case, it is better to use a non-optional or regular optional property. Yes, there are cases in which you can probably "guarantee" that the property will never be `nil` when used, but it is better to be safe and consistent. Similarly, don't use force unwraps.

* Don't use `as!` or `try!`.

* When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

 ```swift
 textContainer?.textLabel?.setNeedsDisplay()
 ```
 
 Use optional binding when it's more convenient to unwrap once and perform multiple operations:

 ```swift
 if let textContainer = self.textContainer {
       // do many things with textContainer
 }
 ```
 
* When unwrapping optionals use the same name for the unwrapped constant or variable where appropriate instead using new variable names.

 **Preferred**

 ```swift
 var subview: UIView?
 var volume: Double?

 // later on...
 if let subview = subview, let volume = volume {
        // do something with unwrapped subview and volume
 }
 ```

 **Not Preferred**

 ```swift
 var optionalSubview: UIView?
 var volume: Double?

 if let unwrappedSubview = optionalSubview {
        if let realVolume = volume {
            // do something with unwrappedSubview and realVolume
        }
 } 
 ```

## Access Modifiers
* Write the access modifier keyword first if it is needed.  

 **Preferred**  

 ```swift
 private static let myPrivateNumber: Int
 ```  

 **Not Preferred**  

 ```swift
 static private let myPrivateNumber: Int
 ```

* The access modifier keyword should not be on a line by itself - keep it inline with what it is describing.

 **Preferred**

 ```swift
 open class Pirate {
     /* ... */
 }
 ```

 **Not Preferred**

 ```swift
 open
 class Pirate {
     /* ... */
 }
 ```

* In general, do not write the `internal` access modifier keyword since it is the default.

* If a property needs to be accessed by unit tests, you will have to make it `internal` to use `@testable import ModuleName`. If a property *should* be private, but you declare it to be `internal` for the purposes of unit testing, make sure you add an appropriate bit of documentation commenting that explains this. You can make use of the `- warning:` markup syntax for clarity as shown below.

 ```swift
 /**
  This property defines the pirate's name.
  - warning: Not `private` for `@testable`.
  */
 let pirateName = "LeChuck"
 ```

* Prefer private to fileprivate where possible.
* When choosing between `public` and `open`, prefer `open` if you intend for something to be subclassable outside of a given module and `public` otherwise. Note that anything `internal` and above can be subclassed in tests by using `@testable import`, so this shouldn't be a reason to use open. In general, lean towards being a bit more liberal with using open when it comes to libraries, but a bit more conservative when it comes to modules in a codebase such as an app where it is easy to change things in multiple modules simultaneously. 

## Custom Operators
Prefer creating named functions to custom operators.

If you want to introduce a custom operator, make sure that you have a very good reason why you want to introduce a new operator into global scope as opposed to using some other construct.

You can override existing operators to support new types (especially `==`). However, your new definitions must preserve the semantics of the operator. For example, `==` must always test equality and return a boolean.

## Switch Statements and Enums
* When using a switch statement that has a finite set of possibilities (`enum`), do NOT include a `default` case. Instead, place unused cases at the bottom and use the `break` keyword to prevent execution.

* Since `switch` cases in Swift break by default, do not include the `break` keyword if it is not needed.

* The `case` statements should line up with the `switch` statement itself as per default Swift standards.

* When defining a case that has an associated value, make sure that this value is appropriately labeled as opposed to just types (e.g. `case hunger(hungerLevel: Int`) instead of `case hunger(Int)`).

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

* Prefer lists of possibilities (e.g. `case 1, 2, 3:`) to using the `fallthrough` keyword where possible).

* If you have a `default` case that shouldn't be reached, preferably throw an error (or handle it some other similar way such as asserting).

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

## Closure Expressions
* Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.

 **Preferred**

 ```swift
 UIView.animate(withDuration: 1.0) {
         self.myView.alpha = 0
 }

 UIView.animate(withDuration: 1.0, animations: {
         self.myView.alpha = 0
 }, completion: { finished in
         self.myView.removeFromSuperview()
 })
 ```

 **Not Preferred**

 ```swift
 UIView.animate(withDuration: 1.0, animations: {
       self.myView.alpha = 0
 })

 UIView.animate(withDuration: 1.0, animations: {
       self.myView.alpha = 0
 }) { f in
       self.myView.removeFromSuperview()
 }
 ```

* For single-expression closures where the context is clear, use implicit returns. Don't use parentheses unlesss is required.

 ```swift
 attendeeList.sorted { $0 > $1 }
 ```

* Chained methods using trailing closures should be clear and easy to read in context. If this is not possible split in two lines.

 ```swift
 let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

 let value = numbers
     .map {$0 * 2}
     .filter {$0 > 50}
     .map {$0 + 10}
 ```

* Use trailing closure syntax unless the meaning of the closure is not obvious without the parameter name (an example of this could be if a method has parameters for success and failure closures).

 ```swift
 // trailing closure
 doSomething(1.0) { parameter1 in
        print("Parameter 1 is \(parameter1)")
 }

 // no trailing closure
 doSomething(1.0, success: { parameter1 in
        print("Success with \(parameter1)")
 }, failure: { (parameter1) in
        print("Failure with \(parameter1)")
 })
 ```
 
## Arrays

* In general, avoid accessing an array directly with subscripts. When possible, use accessors such as `.first` or `.last`, which are optional and won’t crash. 
* Prefer using a `for item in items` syntax when possible as opposed to something like `for i in 0 ..< items.count`. If you need to access an array subscript directly, make sure to do proper bounds checking. You can use for `(index, value) in items.enumerated()` to get both the index and the value.
* Never use the `+=` or `+` operator to append/concatenate to arrays. Instead, use `.append()` or `.append(contentsOf:)` as these are far more performant (at least with respect to compilation) in Swift's current state. If you are declaring an array that is based on other arrays and want to keep it immutable, instead of `let myNewArray = arr1 + arr2`, use `let myNewArray = [arr1, arr2].joined()`.

## Error Handling

* We must use `try/catch` to handle functions that could have error instead of returning optional values.
 
 **Preferred**

 ```swift
 func printSomeFile() {
        let filename = "somefile.txt"
        do {
            let fileContents = try readFile(named: filename)
            print(fileContents)
        } catch {
            print("Unable to open file \(filename): \(error)")
        }
 }

 
 func readFile(named filename: String) throws -> String {
        guard let file = openFile(named: filename) else {
            throw FileError.openFile
        }

        let fileContents = file.read()
        file.close()
        return fileContents
 }
 ```
 **Not Preferred**

 ```swift
 func printSomeFile() {
        let filename = "somefile.txt"
        guard let fileContents = readFile(named: filename) else {
            print("Unable to open file \(filename).")
            return
        }
        print(fileContents)
 }
 
 func readFile(named filename: String) -> String? {
        guard let file = openFile(named: filename) else {
            return nil
        }

        let fileContents = file.read()
        file.close()
        return fileContents
 }
 ```
* We can use `struct` to define different types of errors and use them accordly.

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

* When calling an error prone function never use `try!`. You can use a `try/catch` code block or `try?` and ignore the error but never `try!`, it could crash your app.


## Golden Path
* When coding with conditionals, the left-hand margin of the code should be the "golden" or "happy" path. That is, don't nest `if` statements. Multiple return statements are OK. The `guard` statement is built for this.

 **Preferred**

 ```swift
 func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  guard let context = context else {
      throw FFTError.noContext
  }
  
  guard let inputData = inputData else {
      throw FFTError.noInputData
  }

  // use context and input to compute the frequencies
  return frequencies
}
```

 **Not Preferred**

 ```swift
 func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
   if let context = context {
        if let inputData = inputData {
            // use context and input to compute the frequencies
        return frequencies
        } else {
            throw FFTError.noInputData
        }
   } else {
        throw FFTError.noContext
   }
 }
 ```

* When multiple optionals are unwrapped either with guard or if let, minimize nesting by using the compound version when possible. Example:

 **Preferred**

 ```swift
 guard let number1 = number1,
          let number2 = number2,
          let number3 = number3 else {
        fatalError("impossible")
 }
 // do something with numbers
 ```

 **Not Preferred**

 ```swift
 if let number1 = number1 {
        if let number2 = number2 {
             if let number3 = number3 {
                 // do something with numbers
             } else {
                 fatalError("impossible")
             }
        } else {
             fatalError("impossible")
        }
 } else {
        fatalError("impossible")
 }
 ```

* Guard statements are required to exit in some way. Generally, this should be simple one line statement such as `return`, `throw`, `break`, `continue`, and `fatalError()`. Large code blocks should be avoided. If cleanup code is required for multiple exit points, consider using a `defer` block to avoid cleanup code duplication. 

* When deciding between using an `if` statement or a `guard` statement when unwrapping optionals is not involved, the most important thing to keep in mind is the readability of the code. There are many possible cases here, such as depending on two different booleans, a complicated logical statement involving multiple comparisons, etc., so in general, use your best judgement to write code that is readable and consistent. If you are unsure whether `guard` or `if` is more readable or they seem equally readable, prefer using `guard`.

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

* If choosing between two different states, it makes more sense to use an `if` statement as opposed to a `guard` statement.

 **Preferred**

 ```swift
 if isFriendly {
       print("Hello, nice to meet you!")
 } else {
       print("You have the manners of a beggar.")
 }
 ```

 **Not Preferred**
 
 ```swift
 guard isFriendly else {
      print("You have the manners of a beggar.")
      return
 }

 print("Hello, nice to meet you!")
 ```
 
* You should also use `guard` only if a failure should result in exiting the current context. Below is an example in which it makes more sense to use two `if` statements instead of using two `guard`s - we have two unrelated conditions that should not block one another.

 ```swift
 if let monkeyIsland = monkeyIsland {
       bookVacation(onIsland: monkeyIsland)
 }

 if let woodchuck = woodchuck, canChuckWood(woodchuck) {
       woodchuck.chuckWood()
 }
 ```

* Often, we can run into a situation in which we need to unwrap multiple optionals using guard statements. In general, combine unwraps into a single guard statement if handling the failure of each unwrap is identical (e.g. just a `return`, `break`, `continue`, `throw`, or some other `@noescape`).

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
 
* Don’t use one-liners for guard statements.

 **Preferred**
 
 ```swift
 guard let thingOne = thingOne else {
        return
 }
 ```

 **Not Preferred**

 ```swift
 guard let thingOne = thingOne else { return }
 ````
 
## Image Naming
Image names should be named consistently to preserve organization and developer sanity. Images SHOULD be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose SHOULD be grouped in respective groups in an Asset Catalog.

## References
[Swift Bracing](http://ericasadun.com/2015/12/28/swift-bracing/)


## License

[MIT](/LICENSE)
