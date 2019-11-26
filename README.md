# markdown-it-prism [![Build Status](https://travis-ci.org/jGleitz/markdown-it-prism.svg?branch=master)](https://travis-ci.org/jGleitz/markdown-it-prism) [![npm version](https://badge.fury.io/js/markdown-it-prism.svg)](https://badge.fury.io/js/markdown-it-prism) [![Bower version](https://badge.fury.io/bo/markdown-it-prism.svg)](https://badge.fury.io/bo/markdown-it-prism)
> [markdown-it](https://github.com/markdown-it/markdown-it) plugin to highlight code blocks using [Prism](http://prismjs.com/)

## Usage
```js
const md = require('markdown-it')();
const prism = require('markdown-it-prism');

md.use(prism, options);
```

The plugin will insert the necessary markup into all code blocks. [Include one of Prism’s stylesheets](http://prismjs.com/#basic-usage) in your HTML to get highlighted code.

### Options
The `options` object may contain:

Name   | Description | Default
-------|-------------|--------
`prism` | Prism class instance to override default one. This is so you can initialize your own instance (such as via a bundler like webpack) and clobber the blank generated one. If left blank you will use the pre generated one. | null
`plugins` | Array of [Prism Plugins](http://prismjs.com/#plugins) to load. The names to use can be found [here](https://github.com/PrismJS/prism/tree/master/plugins). Please note that some prism plugins (notably line-numbers) rely on the DOM being present and can thus not be used with this package (see [#1](https://github.com/jGleitz/markdown-it-prism/issues/1)). | `[]`
`init` | A function called after setting up prism. Will receive the prism instance as only argument. Useful for plugins needing further intialisation. | `() => {}`
`defaultLanguageForUnknown` | The language to use for code blocks that specify a language that Prism does not know. No default will be used if this option is `undefined`. | `undefined`
`defaultLanguageForUnspecified` | The language to use for code block that do not specify a language. No default will be used if this option is `undefined`. | `undefined`
`defaultLanguage` | Shorthand to set both `defaultLanguageForUnknown` and `defaultLanguageForUnspecified` to the same value. | `undefined`

## Usage with Webpack
If you want to use this plugin together with [Webpack](https://webpack.js.org/), you need to import all languages you intend to use:

```javascript
import MarkdownIt from 'markdown-it';
import prism from 'markdown-it-prism';

import "prismjs/components/prism-clike"
import "prismjs/components/prism-java"

function component() {
  const md = new MarkdownIt();
  md.use(prism);
  const element = document.createElement('div');
  element.innerHTML = md.render(`
Here is some *code*:
\`\`\`java
public class Test {
  public void foo() {}
}
\`\`\`
`);

  return element;
}

document.body.appendChild(component());
```

*Beware*: Prisms languages have dependencies onto each other. You need to import the languages together with their dependencies in the correct order.
