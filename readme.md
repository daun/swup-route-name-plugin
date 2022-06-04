# Swup Route Name Plugin

**by [daun](https://github.com/daun)**

Use named routes to allow choosing between swup animations.

Given a list of URL patterns, this plugin will use
[path-to-regexp](https://www.npmjs.com/package/path-to-regexp) to identify
the previous and current routes and add additional classnames to allow switching
CSS animations based on the route name, e.g.: `from-route-home` and
`to-route-project`.

## Installation

This plugin can be installed with npm

```bash
npm install @swup/route-name-plugin
```

and included with import

```shell
import SwupRouteNamePlugin from '@swup/route-name-plugin';
```

or included from the dist folder

```html
<script src="./dist/SwupRouteNamePlugin.js"></script>
```

## Usage

To run this plugin, include an instance in the swup options.

```javascript
const swup = new Swup({
  plugins: [
    new SwupRouteNamePlugin({
      routes: [
        { name: 'home', path: '/:lang?' },
        { name: 'project', path: '/:lang/project/:slug' },
        { name: 'projects', path: '/:lang/projects' },
        { name: 'any', path: '(.*)' },
      ]
    })
  ]
});
```

## Classes

The plugin will add `from-route-*` and `to-route-*` classes to the `html` tag.

```html
<html class="is-animating from-route-home to-route-project">
```

You can then choose between animations based on the identified routes.

```css
.transition-default {
  transition: 300ms opacity ease-in-out, 300ms transform ease-in-out;
  opacity: 1;
}

/* Standard transition: fade out */
html.is-animating .transition-default {
  opacity: 0;
}

/* Transition from homepage: transform instead of fade */
html.is-animating.from-route-home .transition-default {
  opacity: 1;
  transform: translateX(100%);
}
```

## Options

All options with their default values:

```javascript
{
  routes: [
    { name: 'any', path: '(.*)' }
  ],
  unknownName: 'unknown',
  pathToRegexpOptions: {}
}
```

### routes

Array of patterns for identifying named routes. Both `name` and `path` are
required.

The `path` needs to be a valid route pattern that
[path-to-regexp](https://www.npmjs.com/package/path-to-regexp) will understand.

Order matters: the first found route name is used.

### unknownName

Default route name if no match was found among available patterns.

### pathToRegexpOptions

Options passed to [path-to-regexp](https://www.npmjs.com/package/path-to-regexp)
for matching. Useful if you want to change case sensitivity, delimiters, etc.
