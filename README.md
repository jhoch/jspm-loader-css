# JSPM Loader: CSS

An extensible CSS loader for JSPM.

Install the plugin and name it `css` locally

```
jspm install css=npm:jspm-loader-css
```

Load the styles by referencing them in your JS:

```js
import from './styles.css!'
```

## :local mode

The default CSS loader supports opt-in CSS Modules syntax. So, importing the following CSS file:

```css
:local(.myComponent) {}
```

generates and loads the following CSS

```css
._path_to_file__myComponent {}
```

and returns the mapping to JS for you to use in templates:

```js
import styles from './styles.css!'
elem.innerHTML = `<div class="${styles.myComponent}"></div>`
```

For the full CSS Modules syntax, where everything is local by default, see the [JSPM CSS Modules Loader](https://github.com/jhoch/jspm-loader-css-modules) project.

## :export & :import

The loader also supports the CSS Modules Interchange Format. 

## Import path notation

The path you specify will be processed through SystemJS with your [configuration](https://github.com/systemjs/systemjs/blob/master/docs/config-api.md).  
For example, with the configuration below :

```js
// Your config.js
System.config({
  paths: {
    "github:*": "jspm_packages/github/*",
    "~/*": "somewhere/*"
  }
}
```

You can write various import paths:

```css
/* Your ike.icss */
.ike {
  composes: bounce animated from "https://github.jspm.io/daneden/animate.css@3.1.0/animate.css";
  composes: bounce animated from "github:daneden/animate.css@3.1.0/animate.css";
  composes: bounce animated from "~/animate.css";
}
```

## Customize your own loader

You can customize this loader to meet your needs.

1. Create a `css.js` file under your project folder next to `config.js` file.
2. In the `css.js` file, include whatever postcss plugins you need:

  ```js
	import { CSSLoader, Plugins } from 'jspm-loader-css'
	import vars from 'postcss-simple-vars' // you want to use this postcss plugin
	
  const {fetch, bundle} = new CSSLoader([
    vars,
    Plugins.localByDefault,
    Plugins.extractImports,
    Plugins.scope,
    Plugins.autoprefixer()
  ], __moduleName);
  
  export {fetch, bundle};
	``` 
	
	Just make sure you have `Plugins.autoprefixer` passed to `new CSSLoader`, it's required.
	
3. Since you have had `jspm-loader-css` installed with `jspm install css=npm:jspm-loader-css`, now open `config.js` and replace line

	```js
	"css": "npm:jspm-loader-css@x.x.x"
	```
	
	with:
	
	```js
	"jspm-loader-css": "npm:jspm-loader-css@x.x.x"
	```
	
 jspm will use what `css.js` exports as the default css loader.
	
You can also check [an example css.js file here](https://github.com/jhoch/glenmaddern.com/blob/master/src/css.js "Customize your own jspm css loader").
	
