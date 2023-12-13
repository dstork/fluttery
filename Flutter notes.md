# Flutter notes

## High-level basics

* `main` is always the entry function, and usually consists of a call passing an instance of a StatelessWidget to `runApp`
```dart
void main() {
	runApp(const MyApp());
}
```
or shorter
```dart
void main() => runApp(const MyApp());
```
This StatelessWidget consists of at least one method, `Widget build(BuildContext context)`. It returns the top-level component/widget, typically a `MaterialApp`.

A `StatefulWidget` contains a method 
```dart
State<Widget> createState() => _WidgetState();
```
`_WidgetState` extends `State<Widget>` and manages its own state. In such a scenario, the `build` method mentioned above is moved to the `_WidgetState` class.

* flutter makes use of trailing commas - use them
* flutter uses composition over inheritance where possible, hence stuff like `Padding` being a widget instead of an attribute of `Text`
* Cmd+Shift+Space (or Ctrl+Shift+Space) within a function or Widget and it will bring up a full list of properties
* Cmd+. (or Ctrl+.) brings up the refactoring menu

## Widgets

* `Column`, container that arranges `children` elements vertically. By default, a `Column` visually places its children at the top -- this can be changed with the `mainAxisAlignment` property (same as for the `Row`)
* `Row`, container that arranges its `children` horizontally
* `ElevatedButton`, button with `onPressed` and `child` properties - `child` is usually a `Text`
* `ElevatedButton.icon` is a convenience constructor that exposes `onPressed`, `icon`, and `label` constructors.
* `Text` takes a string argument to the constructor
* `Scaffold` implements the basic material design visual layout structure. It provides APIs for showing drawers and bottom sheets. See [here](https://api.flutter.dev/flutter/material/Scaffold-class.html)
* `Padding` wraps a `child` widget and provides a `padding`to it (e.g. `const EdgeInsets.all(8.0)`)
* `Card` is a panel with slightly rounded corners and an elevation shadow around its `child` content
* `Center` centers its `child` within itself horizontally and vertically
* `Align` lets you arbitrarily position a child within itself 
* `SizedBox` serves as margin in flutter (`height`/`width`)
* `SafeArea` defines an area that is not obscured by a hardware notch or a status bar. Has a single `child`
* `NavigationRail` defines a sidebar with multiple `destinations` (`NavigationRailDestination`) and can predefine a `selectedIndex`. Event handler `onDestinationSelected`
* `NavigationRailDestination` contains an `icon` and a `label`
* `Expanded`, "greedy" container that automatically uses all available space. Must be a descendant of a `Row`, `Column` or `Flex`. If you're using multiple  `Expanded` widgets, you can specify via the `flex` parameter how this particular widget is to be stretched in size, relative to the others. Specifies its content via the `child` property
* `Placeholder` draws a crossed rectangle to indicate that part of the UI as unfinished
* `LayoutBuilder` contains a `builder(context, constraints)` method which allows to retrieve information about e.g. screen size. It is called every time `constraints` change (e.g. when the user rotates their phone) and it returns a Widget-tree which can change depending on `constraints`-property: `NavigationRail(extended: constraints.maxWidth >= 600)`
* `ListView`, a column that scrolls
* `ListTile` with `title`, `leading` and `onTap`
* `MaterialApp`, convenience widget that wraps a number of widgets that are commonly required for Material Design applications
* `ColoredBox`, 
* `AnimatedSwitcher` defines a `child` and a `duration`
* `Spacer` creates an adjustable, empty spacer that can be used to tune the spacing between widgets in a Flex container, like `Row` or `Column`
* `AnimatedSize`, animated widget that automatically transitions its size over a given `duration` whenever the given `child`'s size changes
* `Dismissible`, a widget that can be dismissed by dragging in the indicated direction

## State

A `ChangeNotifier` (via a class that extends it) together with a `ChangeNotifierProvider` is an easy way to manage state in a flutter app:

```dart
class MyApp extends StatelessWidget {
	// ...
	@override
	Widget build(BuildContext context) {
		return ChangeNotifierProvider(
			create: (context) => MyAppState(),
			child: MaterialApp(
				// title,  theme, ...
			),
			home: MyHomePage(),
		);
	}
}

class MyAppState extends ChangeNotifier {
	var current = WordPair.random();

	void getNext() {
		current = WordPair.random();
		notifyListeners();		// <- push update to listeners
	}
}

class MyHomePage extends StatelessWidget {
	@override
	Widget build(BuildContext content) {
		var appState = context.watch<MyAppState>();
	}
}
```

Every property in a `ChangeNotifier`-derived class is part of the state and can be watched. 

## Theming

The theme in any context can be derived via 
```dart 
final theme = Theme.of(context)
```
and then be referred via 
```dart
color: theme.colorScheme.primary
```

Theming also exists for text. Derive the `theme` variable as above, then access its `textTheme` property to derive a certain `style`:
```dart
final style = theme.textTheme.dispayMedium!.copyWith(
	color: theme.colorSCheme.onPrimary,
);
```
This style can then be used to style individual text widgets:
```dart
Text("My Text", style: style)
```
