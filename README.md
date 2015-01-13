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

## Chintz Parser

### 1. What is a Chintz Parser?

A Chintz Parser is any code library which can parse a Chintz Library and provide the functionality described below. This is intentionally a loose specification, but any implementation should satisfy all of the following goals in general terms.

### 2. Goals

#### 2.1 Make sense of Chintz manifests

The Parser should should be able to read a Chintz Manifest and determine the following information:

 - What is the name of the element
 - What dependencies does the element have

#### 2.2 Resolve dependencies

The Parser should be able to resolve

 - File locations for all static dependencies
 - Template locations and static dependencies for all element dependencies

#### 2.3 Render templates

The parser should be able to render templates, typically but not necessarily using an engine such as Mustache.

### 3. Proposed interface

#### 3.1 Initialisation

It should be possible to initialise the Parser with dependencies

 - An instance of a templating engine, such as Mustache
 - Zero or more instances of formatters for the resolution of specific dependencies, which should be registered in the Parser instance

#### 3.2 Public methods

The Chintz Parser should expose the following methods: `prepare`, `render`, and `getDependencies`.

##### Prepare

The `prepare` method should:

 - Take a single argument; either a single element name or an array/list of them
 - Read the Manifest for each element, and resolve its dependencies - recursively in the case of other elements
 - Register each element's template path where appropriate to the template renderer

##### Render

The `render` method should:

 - Take two arguments; an element name and a data parameter in a directly renderable format
 - Pass these params into the initialised template renderer

##### getDependencies

The `getDependencies` method should:

 - Take a single argument, the name of the dependency as keyed in the Manifests, for example `'js'`
 - By default return an array/list of the specified dependency as resolved during the `prepare` step
 - If there is a registered formatter for the dependency, call a `format` method on that instance, passing the default array/list and returning the result

### 4. State

A single instance of the Chintz Parser should be able to `prepare` any reasonable number of Elements, so that any of them can be rendered with any data, being called in any order.

An instance of a Chintz Parser should only ever maintain a single set of dependencies, which is the deduplicated sum of all the dependencies of the elements prepared by the instance.

## FAQ

### Is there a demo?

Yes, this Chintz library example is demoed using `chintz-parser-php` [here](http://peterchamberlin.com/experiments/chintz-parser-php/index.php).

### Why "element" rather than "component"

The word "component" suggests a complete abstraction which maps to a templateable front end view, whereas within this spec there is no assumption that any given element will have a view. An element may just represent a set of CSS dependencies, or a set of fonts. So a component may be made up of many elements, and an element may represent a component. It is unfortunate that this may create confusion with the concept of an element in HTML, but I haven't thought of a better name yet.
