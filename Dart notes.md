# Dart notes

* Dart is null-safe, which means it does not let you access members that could potentially be `null`. You can override this behavior by suffixing a property with the bang operator to indicate that you're sure the property is not `null` at the time of access
```dart
obj.prop!.copyWith(...);
```
where `obj.prop` is potentially `null`.

* `List`s are created with a generics-like syntax:
```dart
var favorites = <String>[];
```

* `Set`s are created with the {} syntax, like in Python

* An underscore at the beginning of a class name marks the class as private; this is enforced by the compiler

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

* Dart constructors have several convenience forms:
	* Initializing formal parameters
```dart
	class Point {
		final double x;
		final double y;

		Point(this.x, this.y);
	}
```

* fsdf 