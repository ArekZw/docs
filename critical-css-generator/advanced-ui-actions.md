# UI Actions

To discover critical CSS for multiple mobile or desktop viewports, for essential parts such as a mobile navigation menu or for javascript triggered DOM changes, it is possible to define UI actions that will modify the UI state during the Critical CSS generation process. 

The `ui_actions` parameter accepts an array with UI action objects that define UI state changes in chronological order.

# Available UI actions

## `run`

Run the critical CSS generator on the current UI state. This action needs to be repeated each time the UI state has changed in order to discover new above-the-fold CSS code.

```json
{
  "run": true
}
```

## `wait`

Wait for a number of milliseconds before continuing with the next action.

```json
{
  "wait": 1000
}
```

## `viewport`

Set the viewport size.

```json
{
  "viewport": "1300x900"
}
```

## `scroll`

Scroll the viewport. The option accepts a numeric value (pixels from top), a percentage string (`50%`) or an array with `[x,y]` positions in pixels.

```json
{
  "scroll": 400
}
```

## `event`

Trigger a browser event (`new Event()`) on an optional DOM selector.

```json
{
  "event": "click",
  "selector": "a.link"
}
```

## `mouseevent`

Trigger a mouse event (`new MouseEvent()`) on an optional DOM selector. The action accepts a `mouseEventInit` parameter with [MouseEvent options](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/MouseEvent) which includes the ability to set the `x,y` coordinates.

When `mouseEventInit` is omitted, the default options are `{"bubbles": true,"cancelable": true}`.

```json
{
  "mouseevent": "mouseover",
  "selector": "a.link",
  "mouseEventInit": {
    "bubbles": true,
    "cancelable": true
  }
}
```

## `script`

Evaluate javascript code. This action enables to execute javascript code on a page, e.g. to close popups before making further UI state changes.

```json
{
  "script": "Popups.close();"
}
```

## `fn`

Execute a javascript function. This action enables to execute a pre-configured javascript function. The extra option `"promise":true` enables to expect a promise and wait for the promise to resolve before continuing with the next action.

```json
{
  "fn": "action_to_perform",
  "promise": true
}
```

## Adding notes

Each action object accepts a `notes` parameter that can be used to add descriptive text.

```json
{
  "script": "add_to_cart();",
  "notes": "description of action"
}
```

# Example

```json
{
  "ui_actions": [
    {
      "viewport": "360x640",
      "notes": "Set viewport for above-the-fold CSS discovery."
    },
    {
      "wait": 1000,
      "notes": "Wait for 1000ms to enable the viewport to render."
    },
    {
      "run": true,
      "notes": "Run critical CSS generator (above-the-fold CSS calculation)"
    },
    {
      "mouseevent": "click",
      "selector": "a.nav-menu",
      "notes": "Fire a new MouseEvent on a.nav-menu DOM element."
    },
    {
      "wait": 2000
    },
    {
      "run": true
    },
    {
      "script": "close_nav_menu();",
      "notes": "Execute a script, in this case close the menu before continuing with the next viewport."
    },
    {
      "viewport": "1300x900"
    },
    {
      "wait": 1000
    },
    {
      "run": true
    }
  ]
}
```