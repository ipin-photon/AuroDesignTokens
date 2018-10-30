# Orion Design Tokens (ODT)

Design Tokens are abstract UI sub-atomic values that make up the greater Orion Design System (ODT).

The goal of this project is to maintain these core values in such a way as to feed the UI of other engineering efforts rather than be looked upon as a manually communicated design dependency.

## Contained within this repository

This repository serves two purposes:

1. To maintain the single source of record of the distributed token files
1. Provide example pipelines of how to use Design Tokens in prod apps

### The src/ dir

Within the project's `src/` dir are the various token values stored in `.json` format.

Only these source files are included in the npm build for project reference. Example pipelines and documentation are not.

### The example/ dir

Contained within the `example/` directory are example Sass and `config.json` files that illustrate how the Orion Design Tokens can be included with a production project.

### The gulpfile

The `gulpfile.js` file is an example build pipeline that will consume the Orion Design Tokens and create the necessary resources for a production project.

### Webpack

Webpack is supported, **example pipeline is WIP ...**

### Run example locally

To run locally, clone the resources and run the following commands:

```
$ npm i   // install all dependencies
$ gulp    // run example gulp build pipeline
```

Once all the dependencies are installed, the pipeline should output all the necessary build resources from the repo's example and output them within the `example/` dir.

## Install with your production project

To install ODT into your production project requires two steps:

1. npm install the [ODT package](https://itsals.visualstudio.com/Orion%20Design%20System/_packaging?feed=as.com-npm&package=%40alaskaair%2Falaskaair-design-tokens&version=0.1.1384816&protocolType=Npm&_a=package)
1. Build processing pipeline

## Build ODT pipeline

The example pipeline contains all the steps you should consider when building your integrated ODT pipeline.

The example pipeline currently supports Sass and CSS examples. Native mobile platforms are supported, but not yet documented in this project.

### Style Dictionary (dependency)

For processing of `.json` files to a usable Sass/CSS resources, the ODT project uses [Style Dictionary](https://www.npmjs.com/package/style-dictionary). Data formating and build process are engineered to Style Dictionary's opinions.

For more information, see Style Dictionary's [documentation](https://amzn.github.io/style-dictionary/#/).

### config.json

Within the `example/` dir is an example `config.json` file that is required for Style Dictionary. Engineers will be required to edit this document in their prod project to fit their project's specifications.

`"source":` will point to where the tokens are installed within the project

### Config within pipeline

If preferred, you can bypass the `config.json` dependency and extend the configuration directly within your build pipeline reference.

```js
const StyleDictionary = require('style-dictionary').extend({
  source: ['./src/**/*.json'],
  platforms: {
    scss: {
      transformGroup: 'scss',
      buildPath: './example/tokensBuild/',
      files: [{
        destination: '_TokenVariables.scss',
        format: 'scss/variables'
      }]
    }
  }
});

StyleDictionary.buildAllPlatforms();
```

### Sass or CSS Custom Properties?

Style Dictionary is able to output variable files in either Sass or CSS Custom Properties (variables) format. The example pipeline and the `style.scss` file has references to both Sass and CSS variables.

**Important:** CSS variables need to have their references available to them in the final output CSS. Whereas Sass will convert these values to static values in the output CSS.

The example build pipeline addresses this by concatenating the CSS variables with the final CSS output file.

### Native output support

Style Dictionary fully supports native platforms and is able to output resources that are usable in both iOS and Android native development.

### CSS Custom Properties browser support

CSS Custom Properties are new to CSS and thus do not have good legacy browser support. The term polyfill is used loosely in this scenario in that legacy browser support is best addressed in a PostCSS build pipeline.

In the example, the processed Sass is put through a PostCSS process that take the variable value and create a static property along side the dynamic one. You have the option to preserve the custom property or remove it from the final output CSS. It is recommended that you **preserve** the dynamic value for browsers that support this convention.
