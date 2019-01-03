# Predicate
### Simple comparisons
The basic form of a predicate is:
```swift
NSPredicate(format: "attribute = %@", someValue)
```
This is expressing an equality condition between an attribute of an object  ``` attribute = ```, and some value ```someValue```, which will be substituted in place of the ``` %@ ```. For example we could filter an array of ```NSString```s by conditioning on their length attribute:
```swift

let names: [NSString] = [ "Bola", "Mohan", "Josh" ]
let predicate = NSPredicate(format: "length = %d", 4)
let shortNames = (names as NSArray).filtered(using: predicate) // "Bola", "Josh"

```
For testing equality, you can use ```=``` (like SQL) or ```==``` (like Swift) interchangeably.

You can also compare values using ```>```, ```>=```, ```<``` or ```<=```:
```swift

NSPredicate(format: "distance > 500")
```
This example looks for an attribute named ```distance``` on the items being filtered, and narrows the scope to those items where it's strictly greater than ```500```.

You can express inequality using ```<>``` or ```!=```:
```swift

NSPredicate(format: "name != %@", "notMyName")

```
You can even pass the attribute name or key path into the string as a variable, using ```%K```:
```swift

NSPredicate(format: "%K = %@", someAttributeName, someValue)
```
### String comparisons
There are a number of built-in comparison operators for working with strings.

You can compare string values including wildcard characters (```?```, ```*```) using ```LIKE```:
```swift

// A Person!
class Person: NSObject {
  @objc var name: String

  init(_ name: String) {
    self.name = name
    super.init()
  }
}

let people = [ Person("Tray"), Person("May"), Person("Teresa") ]

let predicate1 = NSPredicate(format: "name LIKE %@", "?ay") // wildcard query for a word with “a” and “y” as the 2nd and 3rd letters
let predicate2 = NSPredicate(format: "name LIKE %@", "T*") // wildcard query for names starting with “T”

let result1 = (people as NSArray).filtered(using: predicate1) // May
let result2 = (people as NSArray).filtered(using: predicate2) // Tray, Teresa
```
The wildcard ```?``` will match any one character. Using ```*``` will match any number of characters. (Including zero!)

You can also compare strings with the ```BEGINSWITH```, ```ENDSWITH``` and ```CONTAINS``` operators.
Appending ```[c]``` makes operators (e.g. ```LIKE```) case insensitive:
```swift

NSPredicate(format: "name LIKE[c] %@", "t*") // matches: Tray, Teresa
```
And you can append ```[d]``` to make the comparison ignore any diacritics (for example, accents on characters).

### Collection relationships
You can also express logical relationships related to collections. For example, you can use ```IN``` to check if an object's ```name``` attribute is included in a collection of names.
```swift
let names = [ "Humphrey", "Celestina", "Ulric" ]
let predicate = NSPredicate(format: "name IN %@", names)
```
And you can use ```BETWEEN``` to see if an attribute for an object lies between two bounds (inclusive of the bounds):
```swift
NSPredicate(format: "age BETWEEN {53, 55}") // Matches anything >= 53 and <= 55
```
This really only scratches the surface. There are built in aggregating operators (e.g. ```@avg```, ```@count``` and ```@min```), operators that flatten arrays of arrays to make them easier to search (e.g. ```@unionOfArrays```), and many other operators.

### Creating predicates directly
You can use format strings to express complicated logical conditions. However, it's also possible to construct predicates programmatically without format strings.

This is particularly useful when you're building compound predicates. It’s possible to include compound statements in a format string using ```||``` or ```&&``` – but it can become unwieldy to build a string this way if you have multiple parts that need to be conditionally included. Instead, using an ```NSCompoundPredicate``` rather than a format string can be simpler. You can even combine the two approaches, by creating subpredicates with format strings and combining them programmatically:
```swift
// Two subpredicates created using format strings
let agePredicate = NSPredicate(format: "age > 45")
let retiredPredicate = NSPredicate(format: "retired = true”)

// A compound predicate created directly
let subpredicates: [NSPredicate]
if retireesOnly {
    subpredicates = [ agePredicate, retiredPredicate ]
} else {
    subpredicates = [ agePredicate ]
}

let compoundPredicate = NSCompoundPredicate(andPredicateWithSubpredicates: subpredicates)
```
### Additional resources
* [Predicate programming guide](https://developer.apple.com/documentation/foundation/nspredicate)
* [NSPredicate at NSHipster](https://nshipster.com/nspredicate/)
* [Realm's NSPredicate cheat-sheet](https://academy.realm.io/posts/nspredicate-cheatsheet/)


