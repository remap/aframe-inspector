# A-Frame Inspector Custom Modifications

This modification of the A-Frame inspector is compared with the v1.5.0 changes.

## propertyNotify()

Here we add an entire custom function, which notifies the application about property updates in response to user interactions with relevant details such as entity id, component name, property name, and value.

The following can be found in `./src/components/components/PropertyRow.js` towards the very end before `render()` function.

```javascript
{
    key: "propertyNotify",
    value: function propertyNotify() {
    // borrowed from widget above
    var value =
        this.props.schema.type === "selector"
        ? this.props.entity.getDOMAttribute(
            this.props.componentname
            )[this.props.name]
        : this.props.data;
    window.dispatchEvent(
        new CustomEvent("inspectorPropertyUpdate", {
        detail: {
            id: this.props.entity.id,
            component: this.props.componentname,
            property: this.props.name,
            value: value,
        },
        })
    );
    },
},
```

### render()

These modifications to the existing `render()` in `./src/components/components/PropertyRow.js` are done in conjunction with our previous addition of `propertyNotify()` to enable triggering of the method upon clicking the property name.

```javascript
react_jsx_runtime__WEBPACK_IMPORTED_MODULE_14__.jsx)(
    "a",
    {
    onClick: function onClick(event) {
        _this2.propertyNotify();
        event.preventDefault();
        event.stopPropagation();
    },
    children: props.name,
    }
),
```

We wrapped an anchor tag around `props.name` and added an `onClick` event handler so that clicking on the property name in the rendered component triggers `propertyNotify()`.

The use of `_this2` is to preserve the context of the `this` within the event handler.

```javascript
value: function render() {
    var _this2 = this;
    var props = this.props;
```

## SceneGraph

### rebuildEntityOptions

In `./src/components/scenegraph/SceneGraph.js`, we add a simple `console.log` to indicate that the inspector received the event `rebuildEntityOptions`.

The method updates the list of entities available in the inspector interface.

```javascript
_defineProperty(
  _assertThisInitialized(_this),
  "rebuildEntityOptions",
  function () {
    console.log("AFRAME Inspector received event rebuildEntityOptions");
    // Method implementation
  }
);
```

#### Additional modifications (SceneGraph)

This indicates that the event listener is listening for the `rebuildInspectorSceneGraph` event, which then initiates a rebuild of the entity options in the spector scene graph and triggers a refresh of the sidebar object in the 3D scene (ensures that the UI reflects any changes made to the scene graph).

```javascript
// JB
console.log("Adding rebuildInspectorSceneGraph");
window.addEventListener("rebuildInspectorSceneGraph", function (event) {
  _this.rebuildEntityOptions();
  _lib_Events__WEBPACK_IMPORTED_MODULE_4__["default"].emit(
    "refreshsidebarobject3d"
  );
});
```

## Toolbar.js

### render()

This addition enchances the functionality of the toolbar by providing a convenient way for users to copy entity HTML to the clipboard, which can be useful for various debugging or development purposes.

```javascript
/*#__PURE__*/ (0,
react_jsx_runtime__WEBPACK_IMPORTED_MODULE_5__.jsx)("a", {
    title: "Copy entity HTML to clipboard",
    "data-action": "copy-entity-to-clipboard",
    className: "button fa fa-clipboard",
    style: {
        float: "right",
        marginRight: "5px",
    },
}),
```

## Updates from v1.5.0

Aside from symbol representation changes in the code comments, we only have the following minor update in `./src/lib/viewport.js` under the `objectChange` event listener in `Viewport()`:

```javascript
// We need to call setAttribute for component attrValue to be up to date,
// so that entity.flushToDOM() works correctly when duplicating an entity.
transformControls.object.el.setAttribute(component, value);
```
