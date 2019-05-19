## Vue simple selectbox
[![License](https://img.shields.io/github/license/MannyMannyManny/vue-simple-selectbox.svg?style=popout)](https://github.com/MannyMannyManny/vue-simple-selectbox/blob/master/LICENSE)
[![License](https://img.shields.io/github/last-commit/MannyMannyManny/vue-simple-selectbox.svg)](https://github.com/MannyMannyManny/vue-simple-selectbox/commits/master)
[![License](https://img.shields.io/github/size/MannyMannyManny/vue-simple-selectbox/dist/vue-simple-selectbox.umd.js.svg)](https://github.com/MannyMannyManny/vue-simple-selectbox)
[![License](http://mannymanny.com/files/demo.png)](https://github.com/MannyMannyManny/vue-simple-selectbox)

Vue simple selectbox allows you to easy work and change the wrapper on top of the default select element.

#### Install

Install the dependency  with

```
yarn add vue-simple-selectbox
```

or for npm

```
npm install --save vue-simple-selectbox
```

In the Vue you can use it by simply importing it

```javascript
import SelectBox from "vue-simple-selectbox";

export default {
  components: { SelectBox },
  data: function() {
    return {
      options: [
        {value:"blue", text:"It's Blue"},
        {value:"yellow", text:"Well no, it's Yellow"},
        {value:"green", text:"Nice and cool Green"}
      ]
    }
  }
}
```

And use it like any other component
```html
<template>
  <div id="app">
    <SelectBox :placeholder="'Default placeholder...'" :options="options" :selected="'green'" :minWidth="190"></SelectBox>
  </div>
</template>
```

#### Props

| Option | Type | Required | Default | Info |
| --- | --- | --- | --- | --- |
| placeholder | string | false | 'Select item...' | Default placeholder |
| selected | string | false | - | Value of the selected element if none is selected(if not set, then placeholder is displayed) |
| minWidth | number | false | 160 | Min-width css property applied to the select container |
| options | array | true | - | Array of objects {value: 'value', text: 'text'} to be shown in the select box |