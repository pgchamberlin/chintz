Chintz
======

## What is Chintz

Aside from being a (sometimes derogatory) name for flowery patterns, Chintz is a spec for storing static front end elements in a component-oriented way.

### Work in progress

This is work in progress, unversioned spec. Will solidify soon :-)

## Chintz spec

Each element has its own directory. Within each directory there is
 - A manifest in YAML format
 - Zero or more template files
 - Zero or more CSS, SCSS, JS, or other static dependencies

### The manifest

The manifest must specify
 - The name of the element

The manifest may also list
 - Dependencies
 - Supported data types

If the element has its own template
 - The template will be stored in the directory
 - The template will have the same name as the element

#### An example Chintz manifest

```
name: exampleElement
dependencies:
  css: [ base/base.css, base/typography.css ]
  js: [ exampleElement/someOtherFilename.js ]
  elements: [ anotherThing ]
```

#### An example Chintz directory tree

```
.
├── base
│   ├── base.yaml
│   ├── reset.css
│   ├── typography.css
├── exampleElement
│   ├── exampleElement.yaml
│   ├── exampleElement.mustache
│   └── someOtherFilename.js
└── anotherThing
    ├── anotherThing.yaml
    └── anotherThing.mustache
```

## FAQ

### How do you use this to make web pages?

You need a tool written in your language of choice which can parse Chintz repos. That parser should be able to resolve each element's dependencies recursively, and do whatever is necessary to enable you to render data in your templates. The good news is that there is already [`chintz-parser-php`](https://github.com/pgchamberlin/chintz-parser-php) in development, and there are JavaScript and Ruby ones in the pipeline too.

`chintz-parser-php` has instructions on [getting started with the demo](https://github.com/pgchamberlin/chintz-parser-php).

### Is there a demo?

Yes, this Chintz library example is demoed using `chintz-parser-php` [here](http://peterchamberlin.com/experiments/chintz-parser-php/index.php).

### Why "element" rather than "component"

The word "component" suggests a complete abstraction which maps to a templateable front end view, whereas within this spec there is no assumption that any given element will have a view. An element may just represent a set of CSS dependencies, or a set of fonts. So a component may be made up of many elements, and an element may represent a component.
