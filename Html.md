# HTML

### Property和Attribute

#### Attribute

1. Attributes are defined by HTML, all definitions inside HTML tag are attributes.
2. The type of attributes is always string.

#### Property

1. Properties belong to DOM, the nature of DOM is an object in  JavaScript. We can get and set properties as we do to a normal object in JavaScript and properties can be any types.

2. Non-custom attributes have 1:1 mapping onto properties, like: id, class, title, etc.

   ```html
   </div><div id="test" class="button" foo="1"></div>
   ```

   ```javascript
   document.getElementById('test').id; // return string: "test"
   document.getElementById('test').className; // return string: "button"
   document.getElementById('test').foo; // return undefined as foo is a custom attribute
   ```

   Notice: We need to use **"className"** when get and set **"class"** by property because **"class"** is a JavaScript reserved word.

   ```javascript
   document.getElementById('test').className = button; 
   document.getElementById('test').setAttribute("class","button");
   ```

3. Non-custom propertiy (attribute) changes when corresponding attribute (property) changes in most cases.

   ```html
   <div id="test" class="button"></div>
   ```

   ```javascript
   var div = document.getElementById('test');
   div.className = 'red-input';
   div.getAttribute('class'); // return string: "red-input"
   div.setAttribute('class','green-input');
   div.className; // return string: "green-input"
   ```

4. Attribute which has a default value doesn't change when corresponding property changes.

   ```html
   <input id="search" value="foo" />
   ```

   ```javascript
   var input = document.getElementById('search');
   input.value = 'foo2';
   input.getAttribute('value'); // return string: "foo"
   ```



### Element.setAttribute(name, value)

设置boolean值：

It is recommended to use **property** in JavaScript as it's much easier and faster. Especially for boolean type attributes like:  "checked", "disabled" and "selected", browser automatically converts  them into boolean type properties.

```javascript
b.setAttribute("disabled", ""); //true
b.removeAttribute("disabled") //false

//better
b.disabled = true
b.disabled = false

```

To get the current value of an attribute, use [`getAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute); to remove an attribute, call [`removeAttribute()`](https://developer.mozilla.org/en-US/docs/Web/API/Element/removeAttribute).



