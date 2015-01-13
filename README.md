Chintz
======

## What is Chintz

Aside from being a (sometimes derogatory) name for flowery patterns, Chintz is a spec for storing and rendering static front end elements in atomic component-oriented way.

### Work in progress

This is work in progress unversioned spec. Will solidify soon :-)

## Chintz Library

### 1. What is a Chintz Library?

A Chintz Library is any repository or directory tree which conforms to this specification. It can be consumed by any valid Chintz Parser as specified (see below).

### 2. Chintz Library: directory structure

Chintz libraries have the following base directory structure:

```
.
├── elements
│   ├── atoms
│   ├── molecules
│   └── organisms
└── fixtures
```

### 3. Elements

An Element is any grouping of templates, static dependencies, and/or other elements.

Taking cues from [Pattern Lab](http://patternlab.io/), each Element can be classified as either an Atom, Molecule, or Organism. The boundaries between these can sometimes be blurry, but we treat each as a generalisation of an Element. As far as any parser is concerned they are all the same thing. It adds value in other parts of the organisation (UX stakeholders, product stakeholders) to be able to think about the front end elements in terms of these classifications however.

### 4. Element Directory Structure

Each element has its own directory within either `/atoms`, `/molecules`, or `/organisms`. Within each directory there is
 - A manifest in YAML format
 - Zero or more template files
 - Zero or more CSS, SCSS, JS, or other static dependencies supported by your parser of choice

#### An example Chintz directory tree

```
.
├── elements
│   ├── atoms
│   │   └── exampleAtom
│   │       ├── exampleAtom.yaml
│   │       ├── exampleAtom.mustache
│   │       └── someJs.js
│   ├── molecules
│   │   └── exampleMolecule
│   │       ├── exampleMolecule.yaml
│   │       ├── exampleMolecule.mustache
│   │       └── someCss.css
│   └── organisms
│       └── exampleOrganism
│           ├── exampleOrganism.yaml
│           ├── moreCss.css
│           ├── anotherJs.js
│           └── exampleOrganism.mustache
└── fixtures
```

### 5. The manifest

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


## FAQ

### How do you use this to make web pages?

You need a tool written in your language of choice which can parse Chintz repos. That parser should be able to resolve each element's dependencies recursively, and do whatever is necessary to enable you to render data in your templates. The good news is that there is already [`chintz-parser-php`](https://github.com/pgchamberlin/chintz-parser-php) in development, and there are JavaScript and Ruby ones in the pipeline too.

`chintz-parser-php` has instructions on [getting started with the demo](https://github.com/pgchamberlin/chintz-parser-php).

### Is there a demo?

Yes, this Chintz library example is demoed using `chintz-parser-php` [here](http://peterchamberlin.com/experiments/chintz-parser-php/index.php).

### Why "element" rather than "component"

The word "component" suggests a complete abstraction which maps to a templateable front end view, whereas within this spec there is no assumption that any given element will have a view. An element may just represent a set of CSS dependencies, or a set of fonts. So a component may be made up of many elements, and an element may represent a component.
