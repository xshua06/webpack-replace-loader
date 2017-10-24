# webpack-replace-loader
> A Webpack loader for replacing strings 。

## Examples of use scenarios
1 . When the webpack project is packaged, it replaces the request URL of the development environment to the URL of the production environment.

2 . Replace All color from  `#00ff00` to  `#ff0700` in HTML page.

3 . In large projects, different contents are written in the relevant files according to the packing strategy.

## Install

Install `webpack-replace-loader` as a dependency to a project:
```shell
 npm install webpack-replace-loader --save-dev
```
## Configuration
Configuring webpack packaging policy:
```js
module: {
  loaders: [
    ...
    {
      test: /test\.js$/,
      loader: 'replace-webpack-loader',
      options: {
        arr: [
          {search: '$BaseUrl', replace: 'https://test.googles.com', attr: 'g'},
          {search: '$Title', replace: '社会主义核心价值观', attr: 'g'}
        ]
      }
    }
    ...
  ]
}
```

## Examples
 test.js :
 ```js
  var title = '$Title';
  function showTitle () {
    document.title = title;
  }
 ```
 After this `webpack` configuration package, generate test.js:

```js
var title = 'The core values of Chinese socialism';
function showTitle() {
  document.title = title;
}
```
In this case, `$Title` is simply to provide a search string anchor, and no practical significance.

## Webpack 的其他配置方法

1. replace the first "BaseUrl" with "https://www.googles.com/api/" in a.js ; "Title" instead of "google open API".

2. replace all the "Location" in B.js with "BeiJing"".

```js
module: {
  loaders: [
    ...
    {
      test: /a\.js$/,
      loader: 'replace-webpack-loader',
      options: {
        arr: [
          {search: 'BaseUrl', replace: 'https://www.googles.com/api/'},
          {search: 'Title', replace: 'google open API', attr: 'g'}
        ]
      }
    },
    {
      test: /b\.js$/,
      loader: 'replace-webpack-loader',
      options: {
        search: 'Location',
        replace: 'BeiJing',
        attr: 'g'
      }
    }
    ...
  ]
}
```
As long as your replacement anchor is not the same, you can also merge:

```js
module: {
  loaders: [
    ...
    {
      test: /(a\.js|b.js|c\.js)$/,
      loader: 'replace-webpack-loader',
      options: {
        arr: [
          {search: 'BaseUrl', replace: 'https://www.googles.com/api/'},
          {search: 'Title', replace: 'google open API', attr: 'g'}
          {search: 'Location', replace: 'BeiJing', attr: 'g'}
        ]
      }
    }
    ...
  ]
}
```
Including .css files, .less files. replace `color: red;` with `color: #0cff00; `.
```css
.test {
  color: red;
}
```
config：
```js
options: {
  search: 'color: red;',
  replace: 'color: #0cff00;',
  attr: 'g'
}
```
replaced：
```css
.test {
  color: #0cff00;
}
```

Change the `div` tag of the a.hml file to the `span` tag. Change class `text` to `box`:

```html
<span>$DOM</span>
```
config：
```js
options: {
  arr: [
    {search: 'span', replace: 'div', attr: 'g'},
    {search: '$DOM', replace: `
      <span class="box">
        <span class="text">The core values of Chinese socialism</span>
      </span>
    `}
  ]
}
```

replaced：
```html
<div>
  <span class="box">
    <span class="text">The core values of Chinese socialism</span>
  </span>
</div>
```

## 说明
After 1.2 version, all character escape has been included, but not limited to the following circumstances can be replaced.
```js
search: '<a class__';
search: '.a /bcc .g';
search: '[.a]';
search: '--{a-x}';
search: '({[list]})';
search: '/$/abb^';
search: '<c><d></>';
search: '?+^$@><-';
```