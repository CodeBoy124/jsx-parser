# jsx-parser

## a lightweight jsx parser

npm

```
npm install jsx-parser
```

how to require

```
import JSXParser form 'index'
```

or 
```html
<script src="index.umd"></script>
```


how to use


```javascript

var str = '<div {...[<div/>]} />'
console.log(JSON.stringify(JSXParser(str), null, " "))
```
output
```json
{
 "type": "div",
 "props": {
  "spreadAttribute": {
   "type": "#jsx",
   "nodeValue": [
    {
     "type": "#jsx",
     "nodeValue": "["
    },
    {
     "type": "div",
     "props": {},
     "children": [],
     "isVoidTag": true
    },
    {
     "type": "#jsx",
     "nodeValue": "]"
    }
   ]
  }
 },
 "children": [],
 "isVoidTag": true
}
```


```javascript

var str = `<div id={ 2222 } kkk={<div class={aaa}/>} className="{111}" >xxx{[1,2,3].map(el=> <div>{el}</div>)}yyy
<span>{xxx}--{yyy}</span></div>`
console.log(JSON.stringify(JSXParser(str), null, " "))
```

```json
{
 "type": "div",
 "props": {
  "id": {
   "type": "#jsx",
   "nodeValue": " 2222 "
  },
  "kkk": {
   "type": "#jsx",
   "nodeValue": [
    {
     "type": "div",
     "props": {
      "class": {
       "type": "#jsx",
       "nodeValue": "aaa"
      }
     },
     "children": [],
     "isVoidTag": true
    }
   ]
  },
  "className": "{111}"
 },
 "children": [
  {
   "type": "#text",
   "nodeValue": "xxx"
  },
  {
   "type": "#jsx",
   "nodeValue": [
    {
     "type": "#jsx",
     "nodeValue": "[1,2,3].map(el=> "
    },
    {
     "type": "div",
     "props": {},
     "children": [
      {
       "type": "#jsx",
       "nodeValue": "el"
      }
     ]
    },
    {
     "type": "#jsx",
     "nodeValue": ")"
    }
   ]
  },
  {
   "type": "#text",
   "nodeValue": "yyy\n"
  },
  {
   "type": "span",
   "props": {},
   "children": [
    {
     "type": "#jsx",
     "nodeValue": "xxx"
    },
    {
     "type": "#text",
     "nodeValue": "--"
    },
    {
     "type": "#jsx",
     "nodeValue": "yyy"
    }
   ]
  }
 ]
}
```
 generative rule：
 1. tagName --> type
 2. attributes -->  props
 3. voidTag that like  `<br >` `<hr/>`  or closing tag that like `<element />`  has **isVoidTag = true** property
 4.  `{...obj}` will add **spreadAttribute** property to props object
 5. text node type is "#text"
 6. `{}` will generate **#jsx** node , that has object nodeValue or array nodeValue


You also can be used directly **evalJSX**, that already contains jsx-parser

```javascript
evalJSX.globalNs  = 'React' // or `anu`  https://github.com/RubyLouvre/anu or `preact`
var Parent = React.createClass({
        getChildContext: function() {
            return {
                papa: 'papa'
            };
        },

        render: function() {
            return evalJSX(`
    <div class="parent">{this.props.children}</div>`, {
                this: this
            });
        }
    });
```

 

