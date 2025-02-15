---
title: "ion-popover"
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

import Props from '@site/static/auto-generated/popover/props.md';
import Events from '@site/static/auto-generated/popover/events.md';
import Methods from '@site/static/auto-generated/popover/methods.md';
import Parts from '@site/static/auto-generated/popover/parts.md';
import CustomProps from '@site/static/auto-generated/popover/custom-props.md';
import Slots from '@site/static/auto-generated/popover/slots.md';

<head>
  <title>ion-popover: iOS / Android Popover UI Component & CSS Properties</title>
  <meta name="description" content="ion-popover is a dialog that appears on top of the current page. Learn about the popover UI component and CSS custom properties for iOS and Android devices." />
</head>

import EncapsulationPill from '@components/page/api/EncapsulationPill';
import APITOCInline from '@components/page/api/APITOCInline';

<EncapsulationPill type="shadow" />


A Popover is a dialog that appears on top of the current page. It can be used for anything, but generally it is used for overflow actions that don't fit in the navigation bar.

There are two ways to use `ion-popover`: inline or via the `popoverController`. Each method comes with different considerations, so be sure to use the approach that best fits your use case.

## Inline Popovers

`ion-popover` can be used by writing the component directly in your template. This reduces the number of handlers you need to wire up in order to present the popover.

When using `ion-popover` with Angular, React, or Vue, the component you pass in will be destroyed when the popover is dismissed. As this functionality is provided by the JavaScript framework, using `ion-popover` without a JavaScript framework will not destroy the component you passed in. If this is a needed functionality, we recommend using the `popoverController` instead.

### When to use

Using a popover inline is useful when you do not want to explicitly wire up click events to open the popover. For example, you can use the `trigger` property to designate a button that should present the popover when clicked. You can also use the `trigger-action` property to customize whether the popover should be presented when the trigger is left clicked, right clicked, or hovered over.

If you need fine grained control over when the popover is presented and dismissed, we recommend you use the `popoverController`.

### Angular 

Since the component you passed in needs to be created when the popover is presented and destroyed when the popover is dismissed, we are unable to project the content using `<ng-content>` internally. Instead, we use `<ng-container>` which expects an `<ng-template>` to be passed in. As a result, when passing in your component you will need to wrap it in an `<ng-template>`:

```html
<ion-popover [isOpen]="isPopoverOpen">
  <ng-template>
    <app-popover-content></app-popover-content>
  </ng-template>
</ion-popover>
```

### Triggers

A trigger for an inline `ion-popover` is the element that will open a popover when interacted with. The interaction behavior can be customized by setting the `trigger-action` property. Note that `trigger-action="context-menu"` will prevent your system's default context menu from opening.

:::note
 Triggers are not applicable when using the `popoverController` because the `ion-popover` is not created ahead of time.
:::

import InlineTrigger from '@site/static/usage/popover/presenting/inline-trigger/index.md';

<InlineTrigger />

### isOpen Property

Inline popovers can also be opened by setting the `isOpen` property to `true`. This method can be used if you need finer grained control over the popover than with a trigger.

`isOpen` uses a one-way data binding, meaning it will not automatically be set to `false` when the popover is dismissed. Developers should listen for the `ionPopoverDidDismiss` or `didDismiss` event and set `isOpen` to `false`. The reason for this is it prevents the internals of `ion-popover` from being tightly coupled with the state of the application. With a one way data binding, the popover only needs to concern itself with the boolean value that the reactive variable provides. With a two way data binding, the popover needs to concern itself with both the boolean value as well as the existence of the reactive variable itself. This can lead to non-deterministic behaviors and make applications harder to debug.


import IsOpenTrigger from '@site/static/usage/popover/presenting/inline-isopen/index.md';

<IsOpenTrigger />

## Controller Popovers

`ion-popover` can also be presented programmatically by using the `popoverController` imported from Ionic Framework. This allows you to have complete control over when a popover is presented above and beyond the customization that inline popovers give you.

### When to use

We typically recommend that you write your popovers inline as it streamlines the amount of code in your application. You should only use the `popoverController` for complex use cases where writing a popover inline is impractical. When using a controller, your popover is not created ahead of time, so properties such as `trigger` and `trigger-action` are not applicable here. In addition, nested popovers are not compatible with the controller approach because the popover is automatically added to the root of your application when the `create` method is called.

### React

Instead of a controller, React has a hook called `useIonPopover` which behaves in a similar fashion. Note that `useIonPopover` requires being a descendant of `<IonApp>`. If you need to use a popover outside of an `<IonApp>`, consider using an inline popover instead.

### Usage

import ControllerExample from '@site/static/usage/popover/presenting/controller/index.md';

<ControllerExample />


## Styling

Popovers are presented at the root of your application so they overlay your entire app. This behavior applies to both inline popovers and popovers presented from a controller. As a result, custom popover styles can not be scoped to a particular component as they will not apply to the popover. Instead, styles must be applied globally. For most developers, placing the custom styles in `global.css` is sufficient.

:::note
 If you are building an Ionic Angular app, the styles need to be added to a global stylesheet file.
:::

import Styling from '@site/static/usage/popover/customization/styling/index.md';

<Styling />


## Positioning

### Reference

When presenting a popover, Ionic Framework needs a reference point to present the popover relative to. With `reference="event"`, the popover will be presented relative to the x-y coordinates of the pointer event that was dispatched on your trigger element. With `reference="trigger"`, the popover will be presented relative to the bounding box of your trigger element.

### Side

Regardless of what you choose for your reference point, you can position a popover to the `top`, `right`, `left`, or `bottom` of your reference point by using the `side` property. You can also use the `start` or `end` values if you would like the side to switch based on LTR or RTL modes.

### Alignment

The `alignment` property allows you to line up an edge of your popover with a corresponding edge on your trigger element. The exact edge that is used depends on the value of the `side` property. 

### Side and Alignment Demo

import Positioning from '@site/static/usage/popover/customization/positioning/index.md';

<Positioning />

### Offsets

If you need finer grained control over the positioning of your popover you can use the `--offset-x` and `--offset-y` CSS Variables. For example, `--offset-x: 10px` will move your popover content to the right by `10px`.

## Sizing

When making dropdown menus, you may want to have the width of the popover match the width of the trigger element. Doing this without knowing the trigger width ahead of time is tricky. You can set the `size` property to `'cover'` and Ionic Framework will ensure that the width of the popover matches the width of your trigger element.

If you are using the `popoverController`, you must provide an event via the `event` option and Ionic Framework will use `event.target` as the reference element. See the [controller demo](#controller-popovers) for an example of this pattern.

import Sizing from '@site/static/usage/popover/customization/sizing/index.md';

<Sizing />

## Nested Popovers

When using `ion-popover` inline, you can nested popovers to create nested dropdown menus. When doing this, only the backdrop on the first popover will appear so that the screen does not get progressively darker as you open more popovers.

You can use the `dismissOnSelect` property to automatically close the popover when the popover content has been clicked. This behavior does not apply when clicking a trigger element for another popover.

:::note
 Nested popovers cannot be created when using the `popoverController` because the popover is automatically added to the root of your application when the `create` method is called.
:::

import NestedPopover from '@site/static/usage/popover/nested/index.md';

<NestedPopover />


## Interfaces

Below you will find all of the options available to you when using the `popoverController`. These options should be supplied when calling `popoverController.create()`.

```typescript
interface PopoverOptions {
  component: any;
  componentProps?: { [key: string]: any };
  showBackdrop?: boolean;
  backdropDismiss?: boolean;
  translucent?: boolean;
  cssClass?: string | string[];
  event?: Event;
  animated?: boolean;

  mode?: 'ios' | 'md';
  keyboardClose?: boolean;
  id?: string;
  htmlAttributes?: { [key: string]: any };

  enterAnimation?: AnimationBuilder;
  leaveAnimation?: AnimationBuilder;
  
  size?: PopoverSize;
  dismissOnSelect?: boolean;
  reference?: PositionReference;
  side?: PositionSide;
  alignment?: PositionAlign;
  arrow?: boolean;
}
```


## Types

Below you will find all of the custom types for `ion-popover`:

```typescript
type PopoverSize = 'cover' | 'auto';
type TriggerAction = 'click' | 'hover' | 'context-menu';
type PositionReference = 'trigger' | 'event';
type PositionSide = 'top' | 'right' | 'bottom' | 'left' | 'start' | 'end';
type PositionAlign = 'start' | 'center' | 'end';
```

## Accessibility

### Keyboard Navigation

`ion-popover` has basic keyboard support for navigating between focusable elements inside of the popover. The following table details what each key does:

| Key                | Function                                                     |
| ------------------ | ------------------------------------------------------------ |
| `Tab`              | Moves focus to the next focusable element.                   |
| `Shift` + `Tab`    | Moves focus to the previous focusable element.               |
| `Esc`              | Closes the popover. |
| `Space` or `Enter` | Clicks the focusable element. |


`ion-popover` has full arrow key support for navigating between `ion-item` elements with the `button` property. The most common use case for this is as a dropdown menu in a desktop-focused application. In addition to the basic keyboard support, the following table details arrow key support for dropdown menus:

| Key                | Function                                                       |
| ------------------ | -------------------------------------------------------------- |
| `ArrowUp`          | Moves focus to the previous focusable element.                 |
| `ArrowDown`        | Moves focus to the next focusable element.                     |
| `Home`             | Moves focus to the first focusable element.                    |
| `End`              | Moves focus to the last focusable element.                     |
| `ArrowLeft`        | When used in a child popover, closes the popover and returns focus to the parent popover. |
| `Space`, `Enter`, and `ArrowRight`       | When focusing a trigger element, opens the associated popover. |

## Performance

### Mounting Inner Contents

The content of an inline `ion-popover` is unmounted when closed. If this content is expensive to render, developers can use the `keepContentsMounted` property to mount the content as soon as the popover is mounted. This can help optimize the responsiveness of your application as the inner contents will have already been mounted when the popover opens.

import Mount from '@site/static/usage/popover/performance/mount/index.md';

<Mount />

Developers should keep the following in mind when using `keepContentsMounted`:

- This feature should be used as a last resort in order to deal with existing performance problems. Try to identify and resolve performance bottlenecks before using this feature. Additionally, do not use this to anticipate performance problems.

- This feature is only needed when using a JavaScript Framework. Developers not using a framework can  pass the contents to be rendered into the popover, and the contents will be rendered automatically.

- This feature only works with inline popovers. Popovers created with the `popoverController` are not created ahead of time, so the inner contents are not created either.

- Any JavaScript Framework lifecycle hooks on the inner component will run as soon as the popover is mounted, not when the popover is presented.

## Properties
<Props />

## Events
<Events />

## Methods
<Methods />

## CSS Shadow Parts
<Parts />

## CSS Custom Properties
<CustomProps />

## Slots
<Slots />
