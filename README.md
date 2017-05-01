# MapD Charting

Dimensional charting built to work natively with crossfilter rendered using d3.js.

### Table of Contents
- [Quick Start](#quick-start)
- [Screenshots](#screenshots)
- [Examples](#examples)
- [Synopsis](#synopsis)
- [Development Guidelines](#development-guidelines)
- [Testing](#testing)
- [Scripts](#scripts)
- [Documentation](#documentation)
- [Contributing](.github/CONTRIBUTING.md)
- [License](LICENSE.md)

# Quick Start

##### Step 1: Install Dependencies

```bash
npm install #downloads all dependencies and devDependencies
npm install mapbox-gl@https://github.com/mapd/mapbox-gl-js/tarball/9c04de6949fe498c8c79f5c0627dfd6d6321f307 #downloads mapbox peer dependency
```

##### Step 2: Run Start Script
```bash
npm run start
```

# Screenshots

#### Flights Dataset: Brushing on timeline with Bubble Chart and Row Chart

![example1](https://cloud.githubusercontent.com/assets/2932405/25641647/1acce1f2-2f4a-11e7-87d4-a4e80cb262f5.gif)

#### Tweets Dataset: Brushing on timeline and hovering on Pointmap datapoint which displays row information

![example2](https://cloud.githubusercontent.com/assets/2932405/25641746/acd9cdd0-2f4a-11e7-9821-99e3152075cd.gif)

#### Tweets Dataset: Using MapD-Draw tool on pointmap to select specific areas on a map

![example5](https://cloud.githubusercontent.com/assets/2932405/25641955/ab7015ac-2f4b-11e7-848a-c749fe2081bf.gif)

# Synopsis

MapD-Charting is a superfast charting library that works natively with [crossfilter](https://github.com/square/crossfilter) that is based off [dc.js](https://github.com/dc-js/dc.js).  It is designed to work with MapD-Connector and MapD-Crossfilter to create charts instantly with our MapD-Core SQL Database.  Please see [examples](#examples) for further understanding to quickly create interactive charts.

Our [Tweetmap Demo](https://www.mapd.com/demos/tweetmap/) was made only using MapD-Charting.

# Examples

##### To do -- insert links to examples

# Development Guidelines

### Use Asynchronous Methods

Asynchronous methods must be used. Synchronous methods are depreacted and cause a bad user experience.

For instance, use the asynchronous versions of the `render` and `redraw` methods:

```js
// bad
chart.render()
chart.redraw()

// good
chart.renderAsync()
chart.redrawAsync()
```

Since our version of DC must make asynchronous requests to get data, use the `dataAsync()` method for this request and only use `data()` as the cached result of `dataAsync()`

```js
// bad
chart.data((group) => {
  return group.top()
})

// good
chart.dataAsync((group, callback) => {
  group.topAsync()
    .then(result => {
      chart.dataCache = result
      callback(null, result)
    })
    .catch(e => callback(e))
})

chart.data(() => chart.dataCache)
```

# Testing

New components in MapD-Charting should be unit-tested and linted.  All tests will be in the same folder as the new component.

```
+-- src
|   +-- /mixins/new-mixin-component.js
|   +-- /mixins/new-mixin-component.unit.spec.js
```

The linter and all tests run on
```bash
npm run test
```

To check only unit-tests run:
```bash
npm run test:unit
```

### Linting

Please lint all your code in `mapd-charting/`. The lint config file can be found in `.eslintrc.json`.  For new new components, please fix all lint warnings and errors.

# Scripts

| Command        | Description  |
--- | ---
`npm run start` | Copies files for examples and then serves the example
`npm run build` | Runs webpack and builds js and css in `/dist`
`npm run docs` | Creates and opens docs
`npm run nyc` | Runs coverage reporting for unit tests
`npm run test` | Runs both linting and unit tests
`npm run clean` | Removes node modules, dist, docs, and example files 
