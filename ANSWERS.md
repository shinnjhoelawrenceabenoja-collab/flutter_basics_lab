Github link:
https://github.com/shinnjhoelawrenceabenoja-collab/flutter_basics_lab.git

The Last three build() called lines from Activity 2
build() called: 6 glasses
build() called: 7 glasses
build() called: 8 glasses

Your observations from the experiments in Activities 2, 3 and 5

## 3. Observations from the experiments

### Activity 2: `_glasses++` moved outside `setState()`
When I moved `_glasses++` out of `setState()` and tapped +, the screen stayed
at "0 / 6 glasses" and the progress bar did not move. No new "build() called"
line appeared in the Debug Console. The value of `_glasses` did change in
memory, but without `setState()` Flutter was never told to call `build()`
again, so the screen kept showing the old number. After I put `_glasses++`
back inside `setState()`, the number, progress bar and Debug Console updated
on every tap again.

### Activity 3: `Expanded` removed from one StatBox
When I removed `Expanded` from one StatBox, that box shrank to only the width
of its own content (its icon and text), while the other two boxes shared the
remaining space. The three boxes were no longer equal in width. This shows
that `Expanded` makes a child take an equal share of the free space in a Row.
After I put `Expanded` back, the three boxes were equal again.

### Activity 5: one setting changed in `buildTheme()`
I changed the `ElevatedButton` corner radius from 8 to 20. The "Elevated"
button's corners became much rounder. Only that button changed, because the
setting is inside `elevatedButtonTheme`. The Outlined and Filled buttons
looked the same because they use their own default styles. This shows that
one change in `ThemeData` updates every ElevatedButton in the app at once.

## 4. Review questions

1. **What is the difference between a StatelessWidget and a StatefulWidget? Give one example of each from this lab.**

   A StatelessWidget never changes after it is drawn. It only has a `build()` method and shows the same thing every time. A StatefulWidget keeps data that can change while the app runs, and the screen redraws when that data changes. Example of stateless: `ProfileCard` in Activity 1. Example of stateful: `WaterTrackerPage` in Activity 2, because the number of glasses changes when I tap.

2. **Why is a StatefulWidget written as two classes? What does each class do?**

   The widget class (for example `WaterTrackerPage`) is fixed and only creates the State object through `createState()`. The State class (`_WaterTrackerPageState`) keeps the changing data, like `_glasses`, and has the `build()` method that draws the screen. Flutter can throw away and recreate the widget often, but the State object stays, so the data is not lost.

3. **In Activity 2, what happened when you changed `_glasses` outside `setState()`? Why?**

   The screen did not update when I tapped +, and no new `build() called` line appeared. The value changed in memory, but `setState()` is what tells Flutter that the data changed and `build()` must run again. Without it, Flutter did not redraw the screen.

4. **Looking at the Debug Console in Activity 2, when is `build()` called?**

   `build()` is called when the page is first drawn and again every time `setState()` is called. In the console, a new `build() called` line appeared after every tap on +, −, or Reset.

5. **Explain the main axis and the cross axis of a Row and of a Column.**

   In a Row, the main axis is horizontal (left to right) and the cross axis is vertical. In a Column, the main axis is vertical (top to bottom) and the cross axis is horizontal. `mainAxisAlignment` places children along the main axis, and `crossAxisAlignment` places them along the cross axis.

6. **In Activity 3, what happened when you removed `Expanded` from a StatBox? What does `Expanded` do?**

   The StatBox without `Expanded` became only as wide as its icon and text, so the three boxes were no longer equal. `Expanded` makes a child fill the free space in a Row or Column. When several children are wrapped in `Expanded`, they share that space equally.

7. **Why do we use `GridView.builder` for the menu instead of a Column with many children?**

   `GridView.builder` builds only the cards that are visible on the screen, so long lists stay fast. It also scrolls and arranges the items in columns automatically. A Column builds all its children at once, does not scroll by itself (a long one overflows), and has no grid layout.

8. **How do `Stack` and `Positioned` create the Best seller badge in Activity 4?**

   A `Stack` draws its children on top of each other. In `MenuCard`, the first child is the centered icon, name and price. The "Best seller" label is a second child placed on top. `Positioned(top: 8, left: 8)` pins it to the top-left corner of the card. The `if (item.bestSeller)` check adds the badge only for best-selling items.

9. **What is the difference between customizing with `ThemeData` and with a style on one widget? Give one example of each from Activity 5.**

   `ThemeData` sets the look for the whole app in one place, so every widget of that type follows it. A style on one widget changes only that widget. Example of ThemeData: `elevatedButtonTheme` in `buildTheme()` gives every ElevatedButton rounded corners and padding. Example of one widget: `TextStyle` or `Theme.of(context).textTheme.headlineSmall` on the "My skills" text, which affects only that text.

10. **When you flip the dark-mode switch in Activity 5, which widget holds the state, and how does the change reach `MaterialApp`?**

    `ThemeDemoApp` holds the state, in `_ThemeDemoAppState`, as the `_isDark` variable. The `SwitchListTile` in `SettingsPage` calls `onDarkChanged`, which is `_setDark`. That runs `setState()` and changes `_isDark`, and `build()` runs again. `MaterialApp` then uses `themeMode: _isDark ? ThemeMode.dark : ThemeMode.light`, so the whole app switches theme. This pattern is called lifting state up.
