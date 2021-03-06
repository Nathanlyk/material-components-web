<!--docs:
title: "Sliders"
layout: detail
section: components
iconId: slider
path: /catalog/input-controls/sliders/
-->

# Slider

<!--<div class="article__asset">
  <a class="article__asset-link"
     href="https://material-components-web.appspot.com/slider.html">
    <img src="{{ site.rootpath }}/images/mdc_web_screenshots/sliders.png" width="400" alt="Select screenshot">
  </a>
</div>-->

> **Status**:
> - [x] Continuous Sliders
> - [ ] Discrete Sliders

MDC Slider provides an implementation of the Material Design slider component. It is modeled after
the browser's `<input type="range">` element. Sliders are fully RTL-aware, and conform to the
WAI-ARIA [slider authoring practices](https://www.w3.org/TR/wai-aria-practices-1.1/#slider).

Note that **vertical sliders and range (multi-thumb) sliders are not supported, due to their absence
from the material design spec**.

Also note that we have taken certain deviations from the UX within the spec, e.g. nuances as to the
slider's motion across the track, as well as the style of the slider thumb when in the "off" state
in dark mode. Thus, there may be some treatments which deviate from the mocks. These deviations
arose out of design feedback from seeing sliders used on the web, and thus have been endorsed by
the Material Design team.

## Design and API Documentation

<ul class="icon-list">
  <li class="icon-list-item icon-list-item--spec">
    <a href="https://material.io/guidelines/components/sliders.html">Material Design guidelines: Sliders</a>
  </li>
</ul>

## Installation

```
npm i --save @material/slider
```

## Usage

```html
<div class="mdc-slider" tabindex="0" role="slider"
     aria-valuemin="0" aria-valuemax="100" aria-valuenow="0"
     aria-label="Select Value">
  <div class="mdc-slider__track-container">
    <div class="mdc-slider__track"></div>
  </div>
  <div class="mdc-slider__thumb-container">
    <svg class="mdc-slider__thumb" width="21" height="21">
      <circle cx="10.5" cy="10.5" r="7.875"></circle>
    </svg>
    <div class="mdc-slider__focus-ring"></div>
  </div>
</div>
```

Then in JS

```js
import {MDCSlider} from '@material/slider';

const slider = new MDCSlider(document.querySelector('.mdc-slider'));
slider.listen('MDCSlider:change', () => console.log(`Value changed to ${slider.value}`));
```

You can also include MDCSlider via its UMD version located at `dist/mdc.slider[.min].js`

```js
// CommonJS
const {MDCSlider} = require('@material/slider/dist/mdc.slider');

// AMD
require(['/path/to/@material/slider/dist/mdc.slider'], ({MDCSlider}) => {
  // Use MDCSlider
});

// Global
const {MDCSlider} = mdc.slider;
```

### Initializing the slider with custom ranges/values

When `MDCSlider` is initialized, it reads the element's `aria-valuemin`, `aria-valuemax`, and
`aria-valuenow` values if present and uses them to set the component's `min`, `max`, and `value`
properties. This means you can use these attributes to set these values for the slider within the
DOM.

```html
<div class="mdc-slider" tabindex="0" role="slider"
     aria-valuemin="-5" aria-valuemax="50" aria-valuenow="10"
     aria-label="Select Value">
  <!-- ... -->
</div>
```

### Using a step value

> **NOTE**: If a slider contains a step value it does _not_ mean that the slider is a "discrete"
> slider. "Discrete slider" is a UX treatment, while having a step value is behavioral.

`MDCSlider` supports quantization by allowing users to supply a floating-point `step` value via a
`data-step` attribute.

```html
<div class="mdc-slider" tabindex="0" role="slider"
     aria-valuemin="0" aria-valuemax="100" aria-valuenow="0"
     data-step="2" aria-label="Select Value">
  <!-- ... -->
</div>
```

When a step value is given, the slider will quantize all values to match that step value, _except_
for the minimum and maximum values, which can always be set. This is to ensure consistent behavior.

The step value can be any positive floating-point number, or `0`. When the step value is `0`, the
slider is considered to not have any step.

### Disabled sliders

Adding an `aria-disabled` attribute to a slider will initially disable it.

```html
<div class="mdc-slider" tabindex="0" role="slider"
     aria-valuemin="0" aria-valuemax="100" aria-valuenow="0"
     aria-label="Select Value" aria-disabled="true">
  <!-- ... -->
</div>
```

### MDC Slider Component API

The `MDCSlider` API is modeled after the `<input type="range">` element and supports a subset of the
properties that element supports. It also emits events equivalent to a range input's `input` and
`change` events.

#### Properties

| Property Name | Type | Description |
| --- | --- | --- |
| `value` | `number` | The current value of the slider. Changing this will update the slider's value. |
| `min` | `number` | The minimum value a slider can have. Values set programmatically will be clamped to this minimum value. Changing this property will update the slider's value if it is lower than the new minimum |
| `max` | `number` | The maximum value a slider can have. Values set programmatically will be clamped to this maximum value. Changing this property will update the slider's value if it is greater than the new maximum |
| `step` | `number` | Specifies the increments at which a slider value can be set. Can be any positive number, or `0` for no step. Changing this property will update the slider's value to be quantized along the new step increments |
| `disabled` | `boolean` | Whether or not the slider is disabled |

#### Methods

| Method Signature | Description |
| --- | --- |
| `layout() => void` | Recomputes the dimensions and re-lays out the component. This should be called if the dimensions of the slider itself or any of its parent elements change programmatically (it is called automatically on resize). |
| `stepUp(amount = 1) => void` | Increases the slider value by the given `amount`, or `1` if no amount is given |
| `stepDown(amount = 1) => void` | Increases the slider value by the given `amount`, or `1` if no amount is given |

#### Events

`MDCSlider` emits a `MDCSlider:input` custom event from its root element whenever the slider value
is changed by way of a user event, e.g. when a user is dragging the slider or changing the value
using the arrow keys. The `detail` property of the event is set to the slider instance that was
affected.

`MDCSlider` emits a `MDCSlider:change` custom event from its root element whenever the slider value
is changed _and committed_ by way of a user event, e.g. when a user stops dragging the slider or
changes the value using the arrow keys. The `detail` property of the event is set to the slider
instance that was affected.

### Using the foundation class

The `@material/slider` package ships with an `MDCSliderFoundation` class that framework authors can
use to build a custom MDCSlider component for their framework.

#### Adapter API

| Method Signature | Description |
| --- | --- |
| `addClass(className: string) => void` | Adds a class `className` to the root element |
| `removeClass(className: string) => void` | Removes a class `className` from the root element |
| `getAttribute(name: string) => string?` | Returns the value of the attribute `name` on the root element, or `null` if that attribute is not present on the root element. |
| `setAttribute(name: string, value: string) => void` | Sets an attribute `name` to the value `value` on the root element. |
| `removeAttribute(name: string) => void` | Removes an attribute `name` from the root element |
| `computeBoundingRect() => ClientRect` | Computes and returns the bounding client rect for the root element. Our implementations calls `getBoundingClientRect()`` for this. |
| `getTabIndex() => number` | Returns the value of the `tabIndex` property on the root element |
| `registerInteractionHandler(type: string, handler: EventListener) => void` | Adds an event listener `handler` for event type `type` to the slider's root element |
| `deregisterInteractionHandler(type: string, handler: EventListener) => void` | Removes an event listener `handler` for event type `type` from the slider's root element |
| `registerThumbContainerInteractionHandler(type: string, handler: EventListener) => void` | Adds an event listener `handler` for event type `type` to the slider's thumb container element |
| `deregisterThumbContainerInteractionHandler(type: string, handler: EventListener) => void` | Removes an event listener `handler` for event type `type` from the slider's thumb container element |
| `registerBodyInteractionHandler(type: string, handler: EventListener) => void` | Adds an event listener `handler` for event type `type` to the `<body>` element of the slider's document |
| `deregisterBodyInteractionHandler(type: string, handler: EventListener) => void` | Removes an event listener `handler` for event type `type` from the `<body>` element of the slider's document |
| `registerResizeHandler(handler: EventListener) => void` | Adds an event listener `handler` that is called when the component's viewport resizes, e.g. `window.onresize`. |
| `deregisterResizeHandler(handler: EventListener) => void` | Removes an event listener `handler` that was attached via `registerResizeHandler` |
| `notifyInput() => void` | Broadcasts an "input" event notifying clients that the slider's value is currently being changed. The implementation should choose to pass along any relevant information pertaining to this event. In our case we pass along the instance of the component for which the event is triggered for. |
| `notifyChange() => void` | Broadcasts a "change" event notifying clients that a change to the slider's value has been committed by the user. Similar guidance applies here as for `notifyInput()`. |
| `setThumbContainerStyleProperty(propertyName: string, value: string) => void` | Sets a dash-cased style property `propertyName` to the given `value` on the thumb container element. |
| `setTrackStyleProperty(propertyName: string, value: string) => void` | Sets a dash-cased style property `propertyName` to the given `value` on the track element |
| `isRTL() => boolean` | True if the slider is within an RTL context, false otherwise. |

#### MDCSliderFoundation API

| Method Signature | Description |
| --- | --- |
| `layout() => void` | Same as layout() detailed within the component methods table. Does the majority of the work; the component's layout method simply proxies to this. |
| `getValue() => number` | Returns the current value of the slider |
| `setValue(value: number) => void` | Sets the current value of the slider |
| `getMax() => number` | Returns the max value the slider can have |
| `setMax(max: number) => void` | Sets the max value the slider can have |
| `getMin() => number` | Returns the min value the slider can have |
| `setMin(min: number) => number` | Sets the min value the slider can have |
| `getStep() => number` | Returns the step value of the slider |
| `setStep(step: number) => void` | Sets the step value of the slider |
| `isDisabled() => boolean` | Returns whether or not the slider is disabled |
| `setDisabled(disabled: boolean) => void` | Disables the slider when given true, enables it otherwise. |

### Theming

All thematic elements of sliders make use of the **primary theme color**, including when the
component is used within a dark mode context.

#### Setting the correct background color for disabled/off slider thumbs

One tricky issue with sliders is how the thumb is supposed to look when in the disabled or
"off" states. In these cases, certain portions of the slider's thumb and track are supposed to
become "transparent" and reveal the background color behind it. However, this presents a problem as
there is no elegant way to derive what the background color behind the slider should be. We could
theoretically walk up the DOM until we found an ancestor with a set background, but that would break
the component's encapsulation model.

To solve this, you can supply a css custom property `--mdc-slider-bg-color-behind-component`. When
used, this will override the default colors used for the disabled/off state slider thumb and use
the color specified:

```css
.container {
  background: #fafafa;
}

.container > .mdc-slider {
  --mdc-slider-bg-color-behind-component: #fafafa;
}
```

If you need to accomplish this in a browser which does not support custom properties (IE11 and
older versions of Edge), you can override the Sass variables `$mdc-slider-default-assumed-bg-color`.

```scss
$mdc-slider-default-assumed-bg-color: #fafafa;

@import "@material/slider/mdc-slider";
```

If you're using the `.mdc-theme--dark` classes, you'll want to override `$mdc-slider-dark-theme-assumed-bg-color` instead.

Finally, if you'd prefer not to use Sass, you can use the following CSS snippet which should be
included _after_ the `@material/slider` styles have been loaded onto the page:

```css
.mdc-slider--off .mdc-slider__thumb {
  fill: YOUR_CUSTOM_COLOR;
}

.mdc-slider--disabled .mdc-slider__thumb {
  stroke: YOUR_CUSTOM_COLOR !important;
}
```

Note that the following snippet assumes you are not using `mdc-theme--dark`. If you are, you may
need to add `.mdc-theme--dark` as an additional ancestor selector before the `.mdc-slider--*`
classes.

### Tips/Tricks

#### Preventing [FOUC](https://en.wikipedia.org/wiki/Flash_of_unstyled_content)

Because `MDCSlider` updates its UI based on the values it reads in when it is instantiated, there is
potential for an incorrect first render before the script containing the `MDCSlider` initialization
logic executes. To avoid this, there are a few things you can attempt to do:

If the slider's `aria-valuenow` is set to `"0"`, you can manually add the class `mdc-slider--off`
to the root `mdc-slider` element.

If you know how wide the slider will be at the time of instantiation, you can add an inline style
to the `mdc-slider__thumb-container`/`mdc-slider__track` elements which will position it correctly
by using similar logic to that within our code:

1. Figure out the the percentage of length the thumb should have traveled across the track by
   computing `(value - min) / (max - min)`. We'll call this `pctComplete`.
1. Compute the amount the slider thumb container by multiplying the width of the slider element by
   `pctComplete`. We'll call this `translatePx`. Note that if you're using the slider in an RTL
   content, modify `translatePx` such that `translatePx = <width of the slider element> -
   translatePx`.
1. Set the `transform` style on `mdc-slider__thumb-container` to `translateX(${translatePx}px)
   translateX(-50%)`.
1. Set the `transform` style on `mdc-slider__track` to `scale(pctComplete)`.
