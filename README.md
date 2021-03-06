# ember-content-editable

[![Latest NPM release][npm-badge]][npm-badge-url]
[![TravisCI Build Status][travis-badge]][travis-badge-url]
[![Code Climate][codeclimate-badge]][codeclimate-badge-url]
[![Ember Observer Score][ember-observer-badge]][ember-observer-badge-url]
[![Dependencies][dependencies-badge]][dependencies-badge-url]
[![Dev Dependencies][devDependencies-badge]][devDependencies-badge-url]

Ember cli content-editable component, with placeholder and value binding. Use it just like an `input` or `textarea`, but it will autoresize for you. It also works in [almost all browsers](http://caniuse.com/contenteditable).

## Installation

`ember install ember-content-editable`

### Versions

As this addon has recently changed ownership, this section gives a rough overview over the latest releases and plans for the future.

**0.10.0**: last release by [AddJAm](https://github.com/AddJAm) 

**0.11.x**: updates dependencies and keeps the previous implementation wherever possible

**1.0.0**: will be an upcoming release, which makes use of the updated ember component api. Any features deprecated in 0.11.x or earlier will be removed. Please contribute to the [1.0.0-rewrite](https://github.com/st-h/ember-content-editable/tree/1.0.0-rewrite) branch. Release date is not set yet.

## Usage

Use it just like `input` or `textarea`.

```javascript
{{content-editable value=name
                   placeholder="Your name"
                   type="text"}}
```

You can also pass in an extra CSS class if required, and of course specify the tag.

```javascript
{{content-editable value=name
                   placeholder="Your name"
                   class="name-field"
                   tagName="h3"}}
```

### Options

Option Name          | Description                                    | Default
---------------------|------------------------------------------------|---------
value                | The value to be edited                         | `""`
placeholder          | Placeholder displayed when value is blank      | `""`
stringInterpolator   | Function which processes / intercepts any updated value. Takes a string and returns the string to be used instead.           | none
class                | String with any extra css class               | none
type                 | `number`, `text`, or `html`. `text` strips out any html tags, `html` doesn't.                    | `html`
spellcheck           | Uses browsers spellcheck, same as with `<input>` | none
readonly             | If true, element can't be edited but is focusable | false
disabled             | If true, element can't be edited, focused or tabbed to | false
maxlength            | Maximum length of the input, in characters     | none
allowNewlines        | If false, linebreaks can't be entered          | true
autofocus            | If true, the element will be focused once inserted into the document | false
clearPlaceholderOnFocus | If true, the placeholder will be cleared as soon as the element gains focus (even if no content is present yet) | false

##### isText Deprecation
isText has been deprecated. You should replace `isText=true` with `type="text"`, and `isText=false` with `type="html"`.

##### editable Deprecation
`editable` has been deprecated in favour of `disabled` to be more consistent with
standard input tags.

### Events
You can provide actions to handle the following list of events. Arguments passed to your action are consistent with Ember implementations in places like the `{{input}}` helper. `value` is the current value of the content-editable field, `component` is the component instance itself, and `event` is the corresponding raw event object.

| Event Name     | Arguments
|----------------|----------------
| key-up         | value, event
| key-down       | value, event
| key-press      | component, event
| escape-press   | component, event
| enter          | component, event
| insert-newline | component, event
| focus-in       | component, event
| focus-out      | component, event
| mouse-enter    | component, event
| mouse-leave    | component, event

For example:
```javascript
{{content-editable value=name
                   placeholder="Your name"
                   enter="save"}}
```

### Customizing Placeholder Color
```
.ember-content-editable:empty {
  color: rgba(0,0,0,0.6);
}
```

## Common Problems
These are some solutions to common problems browsers have with contenteditable elements.

### Filtering Input
If you want to filter the input, you can achieve this using the `key-press` event.

The following example filters the input to only allow numerical values.

```
{{content-editable value=age key-press=filter}}
```

```
  filter(currentValue, event) {
    const keyCode = event.which;
    if (keyCode <= 48 || keyCode >= 58) {
      event.preventDefault();
    }
  },
```

### Extra Tags
Some browsers have a bug where extra tags including `<div>`s get inserted into contenteditable fields, usually when newlines are entered.

1) If you don't care about any tags, use `type="text"` to strip all of them.
2) If you do care about tags, either use `display: inline-block` on the content-editable component (simplest solution) or pass a function as `stringInterpolator=myInterpolator` to remove extra text.

```
myInterpolator(inputString) {
  /* Remove extra tags */
  return stringWithNoDivs;
}
```

### Tab Index
The `tabindex` attribute is bound to the element in the DOM, but only [certain tags support it](http://www.w3.org/TR/html4/interact/forms.html#adef-tabindex).

>The following elements support the tabindex attribute: A, AREA, BUTTON, INPUT, OBJECT, SELECT, and TEXTAREA.

So to use `tabindex`, you'll also need to set `tagName` to one of those.

### Newlines aren't showing
Try using `whitespace: pre-line;` or `whitespace: pre-wrap;` in your CSS.

### I can't blur the element
A solution to this is to call `window.getSelection().removeAllRanges()` after you call `blur()` on the element.

For example, if you have `enter='endEditing'` on your content-editable, the following action would prevent the newline and blur the element.

```
endEditing(contentEditable, event) {
  event.preventDefault();
  contentEditable.element.blur();
  window.getSelection().removeAllRanges();
}
```

### Cursor appears too big when element is empty
Setting `display: block;` in CSS seems to solve this.

## Acknowledements

[AddJAm](https://github.com/AddJAm) has done a great job writing and maintaining this addon for a long time. Thanks guys.

## Contributions

The current maintainer (st-h) tries to do his best to maintain this addon in the ember communities interest by keeping dependencies up to date and keeping current features working.

If you want to report a bug, please open a new issue. Any bugs that are not totally obvious should include a way to reproduce the issue (like ember-twiddle) or a failing test. Or even better, provide a PR which tests and fixes the issue.

In case you find there is a feature missing, please provide a PR with corresponding test coverage. Please keep in mind to keep addons lightweight. If in doubt, open an issue first and see what others think about it.

If you want to help in taking care of this addon, just let us know.

[npm-badge]: https://img.shields.io/npm/v/ember-content-editable.svg
[npm-badge-url]: https://www.npmjs.com/package/ember-content-editable
[travis-badge]: https://img.shields.io/travis/st-h/ember-content-editable/master.svg?label=TravisCI
[travis-badge-url]: https://travis-ci.org/st-h/ember-content-editable
[codeclimate-badge]: https://api.codeclimate.com/v1/badges/8688ab1cea89cb7cb918/maintainability
[codeclimate-badge-url]: https://codeclimate.com/github/st-h/ember-content-editable/maintainability
[ember-observer-badge]: http://emberobserver.com/badges/ember-content-editable.svg
[ember-observer-badge-url]: http://emberobserver.com/addons/ember-content-editable
[dependencies-badge]: https://img.shields.io/david/st-h/ember-content-editable.svg
[dependencies-badge-url]: https://david-dm.org/st-h/ember-content-editable
[devDependencies-badge]: https://img.shields.io/david/dev/st-h/ember-content-editable.svg
[devDependencies-badge-url]: https://david-dm.org/st-h/ember-content-editable#info=devDependencies
