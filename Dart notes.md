# Dart notes

## Collections

#### `List`s are created with a generics-like syntax:
```dart
var favorites = <String>[];
```

You can emulate Python's `enumerate(list)` with the `List`s `.indexed` method, which returns a tuple of (`index` and `item`):

```dart
for (final (index, card) in cards.indexed) {
	counter += card.bet * (index + 1);
}
```

*List comprehensions* can be done using `List.generate`:

```dart
var cards =
	List.generate(lines.length, (index) => Card.fromString(lines[index]));
```

is roughly 

```python
cards = [Card(lines[index]) for index in range(len(lines))]
```

#### `Set`s are created with the {} syntax, like in Python
```dart
var mySet = <int>{};
```

#### `Map`s can also be created with a shorthand:
```dart
var myMap = <String, int>{};
```

## Misc

#### Null safety

Dart is null-safe, which means it does not let you access members that could potentially be `null`. You can override this behavior by suffixing a property with the bang operator to indicate that you're sure the property is not `null` at the time of access
```dart
obj.prop!.copyWith(...);
```
where `obj.prop` is potentially `null`.

* `List`s are created with a generics-like syntax:
```dart
var favorites = <String>[];
```

* `Set`s are created with the {} syntax, like in Python
```dart
var mySet = <int>{};
```

* `Map`s can also be created with a shorthand:
```dart
var myMap = <String, int>{};
```

* An underscore at the beginning of a class name marks the class as private; this is enforced by the compiler. No specific modifier is needed.

* Dart allows `for` loops inside collection literals, e.g.
```dart
return Column(
	children: [
		Text("ABC"),
		for (var msg in messages)
			Text(msg),
	],
);
``` 
or
```dart
return Column(
	children: [
		messages.map((m) => Text(m)).toList()
	],
);
```

#### All strings are format strings

Accessing variables is done by prefixing a `$`.

### Operators

#### ??
Also called _null_ operator. This operator returns the expression on its left, except if it is null, and the right expression otherwise.

#### ??=
Also called null-aware assignment. This operator assigns value to the variable on its left, only if that variable is currently null.

#### ?.
Also called null-aware access(method invocation). This operator prevents you from crashing your app by trying to access a property or a method of an object that might be null.

[//]: # (See https://jelenaaa.medium.com/what-are-in-dart-df1f11706dd6)

### Streams

```dart
Stream<String> getName() {
	return Stream.value("Foo");
}

void test() async {
	await for (final value in getName()) {
		print(value);
	}
}
```

### Generators

Generators are defined by the `sync*` or `async*` keyword. Like in Python, the `yield` keyword replaces `return`.

```dart
Iterable<Int> getOneTwoThree() sync* {
	yield 1;
}

void test() {
	for (final value in getOneTwoThree()) {
		print(value);		// 1, 2, 3
	}
}
```

### Generics

```dart
class Pair<A, B> {
	final A value1;
	final B value2;
	Pair(this.value1, this.value2);
}

void test() {
	final names = Pair('foo', 20);
}
```

#### Max integer value

```dart
const int maxValue = -1 >>> 1;
```

## Classes and inheritance

### Subclassing

```dart
abstract class LivingThing {
	void breathe() {
		print("I am breathing");
	}

 void talk();			// no special syntax/keyword needed
}

class Cat extends LivingThing {
	void talk() {
		print("Meow");
	}
}

final fluffers = Cat();
```

### Custom Operators

```dart
class Cat {
	final String name;
	Cat(this.name);
	@override bool operator ==(covariant Cat other) => other.name == name;
	@override int get hashCode => name.hashCode;
}
```

### Extensions

```dart
class Cat {
	final String name;
	Cat(this.name);
}

extension Run on Cat {
	void run() {
		print("Cat $name is running");
	}
}

void test() {
	final meow = Cat("Fluffers");
	print(meow.name);
}
```

### Fat-arrow function syntax

```dart
class Math {
	int multipliedByTwo(int a) => a * 2;
}
```

### Constructors

Constructors are not inherited from superclasses.

Dart constructors have several (convenience) forms:

#### Initializing formal parameters
```dart
class Point {
	final double x;
	final double y;

	Point(this.x, this.y);
}
```

#### Dart has named constructors
```dart

```

#### Factory constructors

Using the `factory` keyword, you can define a factory constructor:

```dart
class Cat {
	final String name; 
	Cat(this.name);
	factory Cat.fluffBall() {					// factory constructor
		return Cat("Fluff ball");
	}
}

final fluffBall = Cat.fluffBall();	// using the factory constructor
```

A factory constructor does not have to return an instance of the class it was defined on (keyword "class clusters"). 

### Getters and setters

For `final` properties, define a getter for outside usage.

```dart
class Car {
	final int _doors = 4;
	int get doors => _doors;
	set doors(numDoors) {
		_doors = numDoors;
	}
}

Car car = Car();
print(car.doors);
```

## Asynchronous Programming

The equivalent to Promises in Dart is a `Future`. 

```dart
Future<int> futureMultipliedByTwo(int a) {
	return Future.delayed(const Duration(seconds: 3), () => a);
}
```

### async/await

```dart
void test() async {
	final result = await futureMultipliedByTwo(2);
}
```

## Parameters

### Optional parameters

Dart has two types of optional parameters, _positional_ and _named_. They are denoted in the parameter list / signature of a function by square brackets ([]) and curly brackets ({}) respectively. Positional:

```dart
getHttpUrl(String server, String path, [int port=80, int numRetries=3]) {
  // ...
}
```

### Named parameters 
```dart
getHttpUrl(String server, String path, {int port = 80}) {
  // ...
}
```

You are free to omit the _port_ parameter, but if you want to use it it has to be addressed by name: 

```dart
getHttpUrl('example.com', '/index.html', port: 8080);
```

Optional parameters have to be called in the specified order. Named parameters do not have this limitation. 

Positional and named parameters cannot be used in the same function declaration. 

[//]: # (See SO: https://stackoverflow.com/questions/13264230/what-is-the-difference-between-named-and-positional-parameters-in-dart)