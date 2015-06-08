# angular-notifications
Add notification popups to any element http://jemonjam.com/angular-notifications

[![Build Status](https://travis-ci.org/jacob-meacham/angular-notifications.svg?branch=develop)](https://travis-ci.org/jacob-meacham/angular-notifications)
[![Coverage Status](https://coveralls.io/repos/jacob-meacham/angular-notifications/badge.svg?branch=develop)](https://coveralls.io/r/jacob-meacham/angular-notifications?branch=develop)
[![Code Climate](https://codeclimate.com/github/jacob-meacham/grunt-lcov-merge/badges/gpa.svg)](https://codeclimate.com/github/jacob-meacham/grunt-lcov-merge)

![demo](http://i.imgur.com/F9qc7Uw.gif)

```js
<notification-icon count='scopeVariable'>
  <i class="fa fa-envelope-o fa-3x"></i>
</notification-icon>
```

## Getting Started
### 1. Install bower components
```shell
bower install angular-notifications --save
```

### 2. Add css and scripts
```html
<link rel="stylesheet" href="bower_components/angular-notifications/angular-notifications.min.css">

<script src="bower_components/angular/angular.js"></script>
<script src="bower_components/angular-animate/angular-animate.js"></script>
<script src="bower_components/angular-notifications/angular-notifications.min.js"></script>
```

### 3. Add a dependency to your app
```
angular.module('MyApp', ['angular-notifications']);
```

### 4. Add a notification-icon element around any other element
```html
<notification-icon count='scopeVariable'>
  ...
</notification-icon>
```

angular-notifications is an angular directive that adds a notification popup on top of any element. The counter is tied to a scope variable and updating the count is as easy as updating the scope variable. angular-notifications comes with a number of canned animations and a default style, but it is easy to add your own styles or custom animations. angular-notifications can also optionally listen for DOM events and clear the count on a DOM event.

## Basic Usage

### Counter
The only required attribute for angular-notifications is 'count'. This uses two-way binding to bind a scope variable to the isolate scope. This makes updating the count very simple, since your controller has full control over how it's set.
```html
<notification-icon count="myScopeVariable">
    ...
</notification-icon>
```
When myScopeVariable is <= 0, the notification icon will not be visible. Once myScopeVariable > 0, the notification will show up.

[Live Demo](http://jemonjam.com/angular-notifications#basic)

### Built-in Animations
angular-notifications comes with a few prebuilt animations for your use.

* bounce
* fade
* grow
* shake

There are three separate animation events: appear, update, and disappear. Appear is triggered when the counter goes from 0 to non-zero. Update is trigger when the counter increments or decrements but does not go to or from zero. Disapper is triggered when the counter goes from non-zero to zero. The default animation for appear and update is grow, and none for disappear. The most common case is setting the same animation for appear and update, and you can use the 'animation' attribute for this case.

```html
<notification-icon count="myCount" animation='bounce'>
    ...
</notification-icon>
```
This will create a notification that bounces when appearing and when the counter is updated. All three animation events can also be set explicitly:

```html
<notification-icon count="myCount" appear-animation='bounce' update-animation='shake' disappear-animation='fade'>
    ...
</notification-icon>
```
This will create a notification that bounces when appearing, shakes when updates, and fades away when disappearing. Because all of these attributes do not use two-way binding, if you're using a variable for the animation, you'll want to use {{myVariable}} when binding.

[Live Demo](http://jemonjam.com/angular-notifications#animations)

### DOM Events
angular-notifications can respond to DOM events to clear the counter. This clears the scope variable and runs an $apply. Your controller can $watch the variable if you want to react to clearing the counter.

```html
<notification-icon count="myCount" clear-trigger='click'>
    ...
</notification-icon>
```
Will cause the count to be cleared upon click. Any DOM event name is valid as a clear-trigger. Because clear-trigger does not use two-way binding, if you're using a variable as the trigger, you'll want to use {{myVariable}} when binding.

[Live Demo](http://jemonjam.com/angular-notifications#dom-events)

## Customizing
angular-notifications was designed to be very simple to customize so that it fits the feel of your app.

### Adding Custom Style
Adding custom style is done via CSS. When the directive is created, it adds a few elements to the DOM
```html
<notification-icon>
    <div class="angular-notifications-container">
        <div class="angular-notifications-icon overlay">
            <div class="notification-inner">
                <!-- Your transcluded element here -->
            </div>
        </div>
    </div>
</notification-icon>
```

You can add styling at any level. For instance, if you just want to change the look of the notifaction icon, you can add to your app's css:

```css
.angular-notifications-icon {
  left: -10px;
  background: yellow;
  color: black;
  width: 30px;
  height: 30px;
  font-weight: bolder;
  font-size: 1.2em;
}
```
Which will make the notification icon appear on the left with a yellow background and bold, larger text.
[Live Demo](http://jemonjam.com/angular-notifications#custom-style)

### Adding Custom Animations
Adding a custom animation is as simple as adding custom styles. angular-notifications uses the standard [angular-animate](https://docs.angularjs.org/guide/animations) module for providing animations. This means that you can use either CSS keyframes or CSS transitions to build animations.

```css
.angular-notifications-icon.my-custom-animation {
  transition:0.5s linear all;
}

.angular-notifications-icon.my-custom-animation-add {
  background: black;
  color: white;
}

.angular-notifications-icon.my-custom-animation-add-active {
  background: yellow;
  color: black;
}

.angular-notifications-icon.my-custom-keyframe-animation {
    animation: custom_keyframe_animation 0.5s;
}
@keyframes custom_keyframe_animation {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
```

Adding your animation is as simple as specifying it by name on the directive
```js
<notification-icon count='myCount' animation='my-custom-animation' disappear-animation='my-custom-keyframe-animation'>
    ...
</notification>
```
[Live Demo](http://jemonjam.com/angular-notifications#custom-style)

## Advanced Usage

### hideCount
If you don't want the count number appear, you can hide the count using the 'hide-count' attribute
```html
<notification-icon count='myCount' hide-count='true'>
    ...
</notification>
```
When myCount > 0, the notification icon will be visible, but the number will be hidden. When myCount <= 0, the icon will be hidden as normal.

[Live Demo](http://jemonjam.com/angular-notifications#hide-count)

### Pill shape
When the number of notifications grows large enough, the icon changes to a pill shape. This is achieved by adding the css class wide-icon to the icon's div. By default, the shape transitions to a pill once the count is greater than or equal to 100, but is configurable via the attribute 'wide-threshold'.
```html
<notification-icon count='myCount' wide-threshold='10'>
    ...
</notification-icon>
```
This will change the shape to a pill once myCount >= 10.

[Live Demo](http://jemonjam.com/angular-notifications#pill)

###