# TiDraggable - Native Draggable Views

Create draggable views.

## History
The original module was done by [Pedro Enrique](https://github.com/pec1985/TiDraggable), but is not maintained anymore. Then [Seth Benjamin](https://github.com/animecyc/TiDraggable) enhanced it, but also seems to have abandoned it. This new fork was sponsored by [Fokke Zandbergen](https://github.com/fokkezb/) and adds new features to the iOS version and updated it to 64-bit.

See [Changelog](#changelog) for details.

## Usage
See the [example](example/app.js) for a demo.

```javascript
var Draggable = require('ti.draggable');
var draggableView = Draggable.createView({
    width : 100,
    height : 100,
    backgroundColor : 'black',
    draggableConfig: {
        enabled: false,
        enabledOnLongpress: true,
        showShadowOnMove: true
    }
});
```

## Creating draggable views

To create a draggable view use the modules' `createView()` method. All of Titanium's properties for a view are supported along the additional `draggableConfig` property containing any options that should be set upon creation. See [Options](#options)

When the draggable proxy is created a new property is set called `draggable` which stores all the configuration properties and allows for options to be updated after creation.

### Other types of views

**iOS ONLY:** You can call almost all of the Ti.UI factory methods on the module, such as `createWindow( ... )`. Only `createView()` and `createWindow()` are officially supported. Other APIs haven't been fully tested.

## Options

Options can be set on view creation using `draggableConfig` or after creation using `DraggableView.draggable.setConfig( ... )`

The `setConfig` method can set options two different ways. You can pass an `object` containing the parameters you with to set or you can pass a key-value pair.

**Setting Options With An Object**
```javascript
DraggableView.draggable.setConfig('enabled', false);
```

**Setting Options With An Object**
```javascript
DraggableView.draggable.setConfig({
  enabled : false
});
```

### `Boolean` - enabled
Flag to enable or disable dragging.

### `Boolean` - enableOnLongpress
**iOS ONLY:** Flag to enable or disable dragging after a longpress on the view.

### `Boolean` - showShadowOnMove
**iOS ONLY:** Flag to enable or disable showing a shadow while the view is being moved.

### `Number` - minLeft
The left-most boundary of the view being dragged. Can be set to `null` to disable property.

### `Number` - maxLeft
The right-most boundary of the view being dragged. Can be set to `null` to disable property.

### `Number` - minTop
The top-most boundary of the view being dragged. Can be set to `null` to disable property.

### `Number` - maxTop
The bottom-most boundary of the view being dragged. Can be set to `null` to disable property.

### `Boolean` - ensureRight
Ensure that that the `right` edge of the view being dragged keeps its integrity. Can be set to `null` to disable property.

### `Boolean` - ensureBottom
Ensure that that the `bottom` edge of the view being dragged keeps its integrity. Can be set to `null` to disable property.

### `Array` - maps
An array of views that should be translated along with the view being dragged. See [View Mapping](#view-mapping).

## Events

Draggable views emit the following events.

**iOS ONLY:** Only on iOS, you can also listen to these same events on the module instead of the views created. Look at the event's `source` property to see what view is being dragged.

### start
Fires when the view starts dragging, passing:

- `center.x`, `center.y` - The coordinates of the center of the view.
- `left`, `top` - The coordinates of the left top of the view.
- `source` - The view being dragged

### move
Fires when the view changes position, passing:

- `center.x`, `center.y` - The coordinates of the center of the view.
- `left`, `top` - The coordinates of the left top of the view.
- `source` - The view being dragged
- `velocity.x`, `velocity.y` - The velocity of the view.

### end
Fires when the drag ends, passing:

- `center.x`, `center.y` - The coordinates of the center of the view.
- `left`, `top` - The coordinates of the left top of the view.
- `source` - The view being dragged
- `distance.x`, `distance.y` - The distance the view was dragged over.

### cancel
Fires when the drag is interupted by an incoming call for example.

## View Mapping

In the case where you want multiple views to be translated at the same time you can pass the `maps` property to the draggable config. This functionality is useful for creating parallax or 1:1 movements.

The `maps` property accepts an array of objects containing any of the following. The `view` property is required.

### Map Options

### `Ti.UI.View` - view
The view to translate.

### `Number` - parallaxAmount
A positive or negative number. Numbers less than `|1|` such as `0.1`, `0.2`, or `0.3` will cause the translation to move *faster* then the translation. A `parallaxAmount` of 1 will translate mapped views 1:1. A parallaxAmount `> 1` will result in a slower translation.

### `Object` - constrain
An object containing the boundaries of the mapped view. Can have the following:

* **x**
  * **start** The start position for the mapped view.
  * **end** The end position for the mapped view.
  * **callback** A function that will receive the completed percentage of the mapped translation. . Android does not support this option.
  * **fromCenter** Translate the view from its center. Android does not support this option.
* **y**
  * **start** The start position for the mapped view.
  * **end** The end position for the mapped view.
  * **callback** A function that will receive the completed percentage of the mapped translation. . Android does not support this option.
  * **fromCenter** Translate the view from its center. Android does not support this option.

## Changelog

- Additions in this fork:
  - iOS: Added listening to events of all draggable views via the module.
  - iOS: Added `enableOnLongpress` and `showShadowOnMove`.
  - iOS: Updated to 64-bit.
- Enhancements by [Seth Benjamin](https://github.com/animecyc/TiDraggable):
  - Improved drag performance for iOS and Android.
  - Updated public APIs for more seamless integration.
  - Removed the InfiniteScroll class as it doesn't really have much to do with the overall module.
  - Removed unnecessary APIs to reduce overall module footprint.
  - Removed unused variables and organized imports.
  - Added ability to unset boundaries.
  - Mapped the missing cancel gesture to the end gesture (firing the respective event).
  - Added ensureRight and ensureBottom, this allows for stable dragging of views where the dimensions are not known.
  - Added enabled boolean property for toggeling drag
  - Views can be mapped and translated with a draggable view.
  - Draggable implementation now has its own configurable property called draggable.
  - iOS: Supports all Ti.UI.View subclasses and Ti.UI.View wrapped views (View, Window, Label)
  - Android: Fixed a bug where touch events were not correctly passed to children or bubbled to the parent.
  - Android: Fixed a bug where min and max bounds were being incorrectly reported after being set.
  - Android: Improved drag tracking. It plays nice with child views now.
  - Android: Added a touch threshold to ensure all child views have a chance to have their respective events fired.
- Original version by [Pedro Enrique](https://github.com/pec1985/TiDraggable)

## Building the module yourself
If you are building the Android module, make sure you update the .classpath and build.properties files to match your setup.
