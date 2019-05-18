# Usage

The unused CSS remover provides two modes: `Simple` and `Advanced`.

# Simple mode

Simple mode applies the default configuration based on the viewport size and scroll position defined by the Critical CSS window.

# Advanced mode

Advanced mode provides many options for tuning the unused CSS remover and PostCSS parser. The configuration of the unused CSS remover is available in a JSON schema.

https://style.tools/json-schemas/unused-css-remover.json

## Configuration

By default, the unused CSS remover simply performs the unused CSS remover process on the basis of the active Critical CSS window.

```json
{
  "atRulesToKeep": [
    "charset",
    "keyframes",
    "import",
    "namespace",
    "page"
  ],
  "atRulesToRemove": [],
  "selectorsToKeep": [
    "*",
    "*:before",
    "*:after",
    "html",
    "body"
  ],
  "selectorsToRemove": [],
  "propertiesToKeep": [],
  "propertiesToRemove": [],
  "pseudoSelectorsToKeep": [
    "/:.*/"
  ],
  "pseudoSelectorsToRemove": [],
  "maxElementsToCheckPerSelector": false,
  "strictParser": false,
  "ui_actions": [
    {
      run: true
    }
  ]
}
```

The options enable to fine tune the PostCSS parser, what selectors and properties to preserve or to forcefully remove and the UI actions to perform during the unused CSS removal process.

## UI actions

On many websites, javascript may insert or modify HTML based on UI state changes or events. 

The `ui_actions` parameter enables to control the UI state. The parameter accepts an array with UI action objects that define UI state changes in chronological order.

### Available UI actions

#### `run`

Run the unused CSS remover on the current UI state. This action needs to be repeated each time the UI state has changed in order to discover new essential CSS code.

```json
{
  "run": true
}
```

#### `wait`

Wait for a number of milliseconds before continuing with the next action.

```json
{
  "wait": 1000
}
```

#### `viewport`

Set the viewport size.

```json
{
  "viewport": "1300x900"
}
```

#### `scroll`

Scroll the viewport. The option accepts a numeric value (pixels from top), a percentage string (`50%`) or an array with `[x,y]` positions in pixels.

```json
{
  "scroll": 400
}
```

#### `event`

Trigger a browser event (`new Event()`) on an optional DOM selector.

```json
{
  "event": "click",
  "selector": "a.link"
}
```

#### `mouseevent`

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

#### `script`

Evaluate javascript code. This action enables to execute javascript code on a page, e.g. to close popups before making further UI state changes.

```json
{
  "script": "Popups.close();"
}
```

#### `fn`

Execute a javascript function. This action enables to execute a pre-configured javascript function. The extra option `"promise":true` enables to expect a promise and wait for the promise to resolve before continuing with the next action.

```json
{
  "fn": "action_to_perform",
  "promise": true
}
```

### Example UI actions

```json
{
  "ui_actions": [
    {
      "viewport": "1300x900"
    },
    {
      "wait": 1000
    },
    {
      "run": true
    },
    {
      "mouseevent": "mousemove",
      "selector": "a.tooltip_menu"
    },
    {
      "wait": 1000
    },
    {
      "run": true
    },
    {
      "mouseevent": "mouseout",
      "selector": "a.tooltip_menu"
    },
    {
      "scroll": "50%"
    },
    {
      "wait": 1000
    },
    {
      "script": "add_to_cart();"
    },
    {
      "wait": 1000
    },
    {
      "run": true
    },
    {
      "script": "cart_popup_close();"
    },
    {
      "viewport": "360x600"
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