# Web Componts
## Overview
### Definition
"Web components are a set of web platform APIs that allow you to create new custom, reusable, encapsulated HTML tags to use in web pages and web apps. Custom components and widgets build on the Web Component standards, will work across modern browsers, and can be used with any JavaScript library or framework that works with HTML."
(source: 2021, webcomponents.org, https://www.webcomponents.org/introduction)
- has a background
  - HTML shows differently in browsers, 
  - HTML5 is limited, 
  - JS components are slow and workload to apply
- web API specification
- it's allowing developers to building up their own HTML Elements

### Specifications
- Custom Eements : basic info of designing and new types of DOM elements
  (https://w3c.github.io/webcomponents/spec/custom/)
- Shadow DOM : usage of encapsulated stye and markup
  (https://w3c.github.io/webcomponents/spec/shadow/)
- ES Modules : inclusion and reuse of JS documents
  (https://html.spec.whatwg.org/multipage/webappapis.html#integration-with-the-javascript-module-system)
- HTML Template : declaration of fragments of markup, not on page loading, but on at runtime
  (https://html.spec.whatwg.org/multipage/scripting.html#the-template-element/)
  
### Structure
- Templates
  - wrapping with <Template> tag
  - not rendering and not downloading resources inside <Temp..> Tag
- Decorators : styling of Template elements
- Custom Elements : creating a new element (name, extends, constructor)
  ```
  [slide-component.html]
  ...
  <element name="x-slide" extends="ul" constructor="SlideControl">
    <template>
        <div class="slide">
            <ul>
                <content select="li"></content>
            </ul>
        </div>
    </template>
    <script>
        SlideControl.prototype = {
            currentNum : function(){},
            lastNum : function(){}
        }
        this.lifecycle({
            created: function(root) {}
        });
    </script>
  </element>
  ...
  
  [link the element]
  <link rel="components" href="./slide-component.html">
  ...
  
  [use in 'is' property]
  <x-slide is="x-slide">
    <li><img src="/1.jpeg"></li>
    <li><img src="2.jpeg"></li>
  </x-slide>
  ...
  ```
- Shadow DOM
  - a scoped subtree inside your element
  - A shadow root is a document fragment that gets attached to a "host" element
  - To create shadow DOM for an element, call element.attachShadow()
  ```
  const header = document.createElement('header');
  const shadowRoot = header.attachShadow({mode: 'open'});
  shadowRoot.innerHTML = '<h1>Hello Shadow DOM</h1>'; // Could also use appendChild().
  // header.shadowRoot === shadowRoot
  // shadowRoot.host === header
  ```
