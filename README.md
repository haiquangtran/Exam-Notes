# Programming in HTML5 with JavaScript and CSS3 Notes

# CSS3

## @media
- Sets the media types for a set of rules in a stylesheet object.
- Used for responsiveness (styling for different devices and sizes etc)

## Animations: CSS Keyframes Animations
- @keyframes allow you to specify values of a CSS property needs at different points during the animation.
  - e.g. @keyframes fadeOut { from {} to{} }
## Text: Font-Size is Complicated
- Generally, 1em = 12pt = 16px = 100%
  - When using these font-sizes, when you increase the base font size (using the body CSS selctor) from 100% to 120% they all change differently.
  - http://kyleschaeffer.com/development/css-font-size-em-vs-px-vs-pt-vs/
- Sizing with px
  - Provide reliability and consistency, but IE will not allow adjustments to font size (although zoom gets round this)
- Sizing with em
  - Measurements are relative to parent element, so they compound with nested elements
- Sizing with rem
  - "Root em": measurements are relative to the root element
- https://snook.ca/archives/html_and_css/font-size-with-rem
- @font-face is a css rule which allows you to download a particular font from your server to render a webpage if the user hasn't got that font installed
  - Web designers no longer have to adhere to particular set of web safe fonts that the user has preinstalled on their computer.

## Layout: Block Styles
- display
  - block: formatted down the page one after another and respect padding, border, and margin values
  - inline: formatted one after another based on the baseline of their text content until they break down onto another line but they ignore height and width values. 
  - inline-block: formatted one after another based on baseline of their text content, but they keep height and width values.
  - table: enables you to identify blocks on the page as tables, rows, cols, and cells. Blocks are aligned by their edges rather than their content, and sized to fit the computed table.
  - flex/ms-flexbox: choose in which direction boxes are laid out and how boxes are sized, depending on how any excess whitespace around blocks should be handled.

## Flex layout: Enabling Flex layout (apply to container)
- Look up the flex layout
- Give the container ability to alter its items width, height, and order to best fill the available space (mostly to accommodate to all kind of display devices and screen sizes)
- Flex layout involves setting properties on both the flex container and the flex itemsS
- To establish the main-axis, defining the direction, flex items are placed in the flex container. 
- flex allows a flex item to grow if necessary
  - if all items have flex-grow = 1, they will use equal size inside the container; if you were to give an item flex-grow = 2, that child would take up twice as much space

## Pseudo Elements and Pseudo Classes
- pseudo-class is made of one colon (:) followed by the name of the pseudo class. i.e. a:link
  - If you define CSS rules that match against more than one of these pseudo-classes, it is important that you specify these pseudo-classes in the following order (with prefix is a:): link, visited, focus, hover, active.
  - a:hover MUST come after a:link and a:visited in the CSS definition in order to be effective!
  - a:active MUST come after a:hover in the CSS definition in order to be effective!
- pseudo-element is made of two colons (::) followed by the name of the pseudo element.
  - Only one pseudo-thingy may appear per selector so you cannot combine :hover and ::after, for example.

## clear
- turning off float by using clear
  - left, right, both, none, inherit
- Elements after the floating element will flow around it unless you use the clear property
- The clear property specifies which sides an element other floating elements are not allowed

## Id vs class
- Use id to identify a single unique element (id's are unique). The same id can be used in multiple different pages (i.e. top navigation bar), but their should only be a unique ID on the same page. 
- Use class to identify multiple elements (classes are not unique)

## CSS Origins
- Style sheets may have three different origins:
  - Author: The author of the web page define styles for the document. It specifies source document according to conventions of the document language
  - User: The reader, the user of the browser, may have a custom style sheet to tailor its experience. May specify style information for a particular document, e.g. for visual impairment
  - User agent: The browser has a basic style sheet that gives a default style to any document.Conforming user agents must apply a default style sheet (e.g. for visual browsers, the EM element in HTML is presented using an italic font). 
- By default, rules in author style sheets have more weight than rules in user style sheets
  - Precedence is reversed, however, for !important rules (https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)
  - All user and author rules have more weight than rules in the user agent's default style sheet

## The order styles are applied to the pages using HTML5 and CSS3
- user agent style sheets
- user normal style sheets
- author normal style sheets
- author important style sheets
- user important style sheets
- http://stackoverflow.com/questions/13638038/style-sheets-priority-order

## CSS Specificity
- Specificity is the weight that is applied to a given CSS declaration. 
- Specificity only applies when the same element is targeted by multiple declarations. 
- When specificity is equal to any of the multiple declarations, the last declaration found in the CSS is applied to the element. https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity
- The following list of selector types is by increasing specificity:
  0. Type selectors (e.g. h1) and pseudo-elements (e.g., :before).
  1. Class selectors (e.g., .example), attributes selectors (e.g. [type="radio"]) and pseudo-classes (e.g. :hover).
  2. ID selectors (e.g. #example)

# HTML5

## HTML <input> tag
- used within a form to declare input controls that allow users to input data. 
- Differences between HTML 4.01 and HTML5
  - In HTML4.01, the align attribute is deprecated, and is not supported in HTML5. Use CSS to align <input> elements.
  - In HTML5, the input tag has several new attributes, and the type attribute has several new values. 

## Block vs Inline content
- Block content 
  - always starts on a new line and takes up the full width available (stetches out to left and right as far as can)
  - Use div for block content
- Inline content
  - does not start on a new line and only takes up as much width as necessary.
  - Use span (also used for styling) for inline content

## HTML5 hyperlinks
- The <a> tag is always a hyperlink in HTML5. 
- The href specifies the resource URL link goes to
- You can have a target attribute that opens the linked document in a new window or tab
  - open in new tab or window: target = "_blank"
  - other options are: _self, _parent, _top, or framename. 

## HTML5 Web Storage
- With HTML5, web pages can store data locally within the user's browser
  - Earlier, this was done with cookies. However, Web Storage is more secure and faster. 
  - The data is not included with every server request, but ONLY used when asked for. 
  - It is possible to store large amounts of data, without affecting the website's performance.
- HTMLStorageObject
  - Represents list of key/value pairs from the Web Storage area
- localStorage property
  - Retrieves the Web Storage area specific to the current document
- sessionStorage property
  - Retrieves the Web Storage area for the session.

## Differences between SVG and Canvas
- Canvas: 
  - Resolution dependent
  - No support for event handlers
  - Poor text rendering capabilities
  - Can save resulting image as .png or .jpg
  - Well suited for graphic-intensive games
- SVG:
  - Resolution independent
  - Support for event handlers
  - Best suited for applications with large rendering areas (Google Maps)
  - Slow rendering if complex (anything that uses the DOM a lot will be slow)
  - Not suited for game applications

## Semantic HTML5 Elements
- **article**
  - Defines independent self-contained content. 
  - Should make sense on its own and should be possible to distribute it independently from the rest of the site.
  - Authors are encouraged to use article element instead of section element when it would make sense to syndicate (group) the contents of the element
- **aside**
  - Defines some content aside from the content it is placed in. 
  - Should be related to the surrounding content. (i.e. The aside content could be placed as a sidebar in an article or a pull-quote)
  - Used for tangentially related content
  - Ask yourself if the content within the aside can be removed without reducing the meaning of the main content. 
- **div**
  - Division or section in an HTML document
  - Used to group block-elements to format them with CSS
  - Have no semantic meaning
- **nav**
  - Defines a set of navigation links
  - Intended for only major block of navigation links (Not all links must be nav elements)
  - Browsers such as screen readers for disabled users can use this element to determine whether to omit the initial rednering of this content.
- **section**
  - Defines sections such as chaptors, headers, footers of the document
  - Used for grouping together thematically-related content
- **span**
  - Used to group inline-elements in a document. 
  - Provdes no visual change by itself (can be used to add styling though).
- **figure**
  - specifies self-contained content, like illustartions, diagrams, photos, code listings, etc. 
  - While the content of the figure element is related to the main flow, its position is independent of the main flow, and if removed it should not affect the flow of the document.
- http://www.w3schools.com/tags/default.asp and http://www.anthonycalzadilla.com/2010/08/html5-section-aside-header-nav-footer-elements-not-as-obvious-as-they-sound/

## Section and Headings
- If you have h1 elements in nested sections they get smaller and smaller, like h2, and h3

## Boolean Attributes
 - The presence of a boolean attribute on an element = true value
 - The absence of the attribute = false value
 - "true" and "false" are NOT allowed on boolean HTML attributes. So you must use the above (presence/absence of the attribute). e.g. the required attribute applies if on an element if you use it, but if omitted, is not applied. 

## iframe
- Specifies an inline frame
- An inline frame is used to embed another document within the current HTML document. 
- New attributes in HTML 5:
  - sandbox = allow-forms, allow-same-origin, allow-scripts, allow-top-navigation
- The sandbox attribute should always be used to restrict what data can be placed into an iFrame.

# JavaScript

## == vs ===
- Use === to check equality of value and type
- Use == to check only the value
- Always use === or !== unless you are sure you only want to check for the truthiness of an expression.

## How to Deal with Floating Point Numbers (Floating Point Arithmetic)
- Squeezing real numbers into a finite number of bits can require an approximate representation.
- JS uses 64-bit floating point representation, which is same as C#'s double.
- NEVER compare floats and double with ==
- Instead, compare the absolute value of their differences and make sure that this difference is as small as possible:
  - var x = 0.1 + 0.2; 
  - var difference = Math.abs(x - 0.3);
  - if (difference < 0.0000001) etc...

## Array.join vs Array.concat
- To combine array items into a comma-separated string use join()
- To combine two arrays into a new array use concat

## Function overloading
- JS functions don't have typed signatures which mean they can't do overloading
- When you define multiple functions with the same name, the one that appears last in your code replaces any previous ones.
- Can simulate overloading by checking for named arguments (if statements).

## Referencing and Executing Functions
- For a named function, pass its name
- If you use parentheses, you are executing the function and assigning whatever it returns as the reference.

## Passing Parameters by Value and by Reference
- Numbers and Strings are passed by value
- Objects are passed by reference

## DOM Event Handling
- If an element and one of its ancestors have an event handler for the same event, which one should fire first?
  - Capturing means A, then B,
  - Bubbling means B, then A
  - W3C supports both: capturing happens first, then bubbling.

## Null-Coalescing Operator
- A simple way is to use the || operator which is equivalent to the ?? operator in C#
- answer = x || someDefault;

## Promises
- Represent the next great paradigm in JS Programming
- A promise represents the result of an action, which may or may not have completed.
- A promise has a function called then, which can be given callbacks to be called when the promise is fulfilled or has failed.
- The real power of promises comes from chaining many of them together
  - Calling promise.then(func) returns a new promise, which is not fulfilled until func has completed
  - jQuery's $.Deferred (done, fail, always, then, and so on)

## JavaScript 1.7 or higher: let blocks
- let allows you to declare variables, limiting its scope to the block, statement, or expression on which it is used.
  - if (a === 5) let a = 4; // scope is inside the if-block
- Unlike var keyword, which defines variable globally, or locally to an entire function.

## Scoping: Hoisting variables
- Function and var declarations are always moved invisibly to top of their containing scope by JS interpreter.
- Best practice: Each function should have single var at top for all variables.
- Hositing with functions differs depending on if you are using a named or anonymous function. 
  - calling foo() before var foo = function() will not work.
  - calling foo() before function foo() will work.

## JQuery
- Created by John Resig, Jan 2006
- Light-weight, CSS3 complaint, cross-browser, feature-rich library
- Two major version branches
  - jQuery 1.x
  - jQuery 2.x
- 2.x has the same API, but does not support Microsoft IE 6,7, or 8 so use 1.x version unless you are certain no IE 6,7,8 users are visiting your site.

### jQuery: Attribute Selectors
- Search characters in HTML attributes with [...] i.e. #('a[href*=firebrand')
  - *= searches for the term in all text
  - ~= searches for the word (term delimited by spaces) in all text
  - ^= searches for the term at the beginning
  - $= searches for the term at the end
  - != searches for non-mathces

### jQuery: Serializing Forms
- serialize() method
  - returns a string in standard URL-encoded notation
  - Encode a set of form elements as a string for submission

### jQuery: Should I Use success or Done? (AJAX Calls)
- Old: success, error, complete
- New: done, fail, always
- Since the implementation of $.Deferreds, done is the preferred way to implement success callbacks.

## JS unit test tools for TDD
- There are many test tools for TDD with JS
- QUnit can be used to perform TDD with web apps
- QUnit-Metro can be used to perform TDD with Windows 8 apps.
- JsUnit seems to be the best option, but it is not perfect because:
  - no simple and integrated way to run JS unit test
  - forces you to write unit tests in html files 
  - forces you to have local installation of JsUnit framework to avoid hard coded path to reference js unit files.

## HTML input element
- <input> elements are used within a <form> element to declare input controls that allow users to input data
- The <input> element is empty, it contains attributes only
- Use the <label> element to define labels for <input> elements

## Getting Data for Validation using jQuery
- val()
  - Get's current value of the first element in the set of matched elements or set the value of every matched element
  - Primarily used to get values of form elements such as input, select and text area

# Communicating with a Remote Server
- Encoding and Decoding
  - For concatenating together and splitting apart text strings in URI parts
  - EncodedURI takes something that's nearly a URI, but has invalid characters such as spaces in it, and turns it into a real URI
  - encodeURI and decodeURI are intended for use on the full URI
  - encodeURIComponent and decodeURIComponent are inteded to be used on URI components i.e. any part that lies between separators (; / ? : @ & = + $ , #)
- Look up the JQuery: $.get calls for AJAX


## Regular Expressions
- When validating input, include the leading caret (^) and trailing dollar to avoid security vulnerabilities.
  - ^ means start of input; $ means end of input

# JSON

## Serialization
- The data is put into a format for transmission.
- In JSON use JSON.stringify() to serialize data

## Deserialization
- The transmitted data is converted into something that can be worked with, such as a custom object.
- In JSON use JSON.parse() to deserialize data

## JSON is a subset of OLN
- Objects, arrays, strings, finite numbers, true, false, and null are fully supported so they can be serialized and restored.

## Limitations of JSON
- Only the enumerable own properties of an object are serialized 
- NaN, Infinity, and -Infinity are serialized to null
- Date objects are serialized to ISO-formatted date strings, but JSON.parse() leaves it as a string rather than restoring the Date
- Function, RegExp, and Error objects and the undefined value cannot be serialized; if a property value cannot be serialized, that property is simply omitted from the stringified output.

## Adding Offline Support to Web Applications

### localStorage vs sessionStorage both extend Storage
- sessionStorage = represents set of storage areas specific to current top-level browsing context
- localStorage = provides a Storage object for an origin
  - no difference between them except for intended "non-persistence" of sessionStorage
  - items = key/value pairs (string/string)

### ApplicationCache
- Any page the user navigates to that includes a manifest will be implicitly added to the application cache
- You can see urls that are controlled by the app cache by visiting chrome://appcache-internals/ in chrome
  - If the manifest itself returns a 404 or 410, the cache is deleted
  - If the manifest or a resource specified in it fails to download, the entire cache update process fails.
  - Once an app is offline it remains cached until one of the following happens:
    - The user clears their browser's data storage for your site
    - The manifest file is modified

## Implementing Real-time Communication by Using Web Sockets

### The WebSocket API
- API enables web apps to maintain bidirectional communications with server-side processes.
- Dependencies: HTML and WebIDL (Web Interface Definition Language)
- Look this up (W3C)

### Web Sockets: When to use
- Achieving zero-lag connectivity between Web clients and servers requires going beyond the HTTP protocol
  - The new WebSocket Protocol aims to overcome a structural limitation of the HTTP protocol that makes it inefficient for Web applications hosted in browsers to stay connected to the server over a persistent connection.
- Great for real-time communication and updates
- http://msdn.microsoft.com/en-us/magazine/hh975342.aspx

### Web Sockets: SignalR
- ASP.NET SignalR is a lib for ASP.NET devs that makes it simple to add real-time web functionality to your apps.
  - SignalR uses WebSockets under the covers when available
  - SignalR will fallback to other techniques when it isn't
  - Your app code stays the same

## Performing Background Processing by Using Web Workers

### WebWorker API
- WebWorker defines a way to run scripts in the background. 
- WebWorker provides web application authors to spawn background scripts that run in parallel with the main page.
  - Can spawn several threads to use for long-running tasks.
  - All communciation to and from the worker thread is through messages.
  - Note: You cannot send references to DOM objects to a worker thread. Web workers are limited in what data they can access. Only JS primitives such as Object or String values are allowed.
- Traditionally browsers have been single-threaded, forcing all the scripts in your app to run together in a single UI thread. 
  - Although you can create illusion of several things happening at same time by using DOM events and setTimeout,
  it only takes one computationally intensive task to affect the user experience.


### Web Workers: What can you use inside a web worker?
- Worker has a global scope separate from the page
- Worker can use: most JS APIs, External script libraries.
- Limitations of Web Workers:
  - Can't access the DOM or window
  - Has a single thread. Should process and return as quick as possible, otherwise messages will be queued up (or additional workers could be spawned).

### Web Workers: Message Passing
- In both the page and the worker
  - Handle the message (aka onmessage) event to receive messages
  - Call the postMessage() method to send messages
- In the page: create Worker instances, post objects to them, and handle received objects.

## Cross Domain Requests 

### Understanding the Same-Origin Policy Problem
- Two pages have same origin if the protocol, port (if one is specified), and host are the same for both pages.
  - The same-origin policy controls interactions between two different origins, such as when you use XMLHttpRequest
  - Under the policy, a web browser allows scripts contained in a first web page to access data in a second web page, but onyl if both web pages have the same origin.
  - http://security.stackexchange.com/questions/8264/why-is-the-same-origin-policy-so-important
- Use JSONP or CORS to allow cross-origin access
- JSONP (JSON with Padding)
  - Requests for JSONP retrieve JS code which is not blocked by same origin policy as JSON would be. (Evaluated by JS interpreted, not by a JSON parser)
  - JSON payload woul dbe blocked
  - Equivalent JSONP payload (JS) is let through
  - For each new JSONP request, the browser must add a new <script> element, or reuse an existing one.
    - JSONP can be said to allow browser pages to work around the same origin policy via script element injection and function name negotiation with the server.
  - Using JSONP with ASP.NET Web API: JSONP is not supported by default so we need to add a media type formatter manually.
- https://www.w3.org/TR/cors/

### Cross-Origin Resource Sharing (CORS)
- Works by adding new HTTP headers that allow servers to describe the set of origins that are permitted to read that information
- A simple cross-site request is one that only uses GET, HEAD or POST with no custom headers 
- Might need to configure IIS to Enable CORS

### Preflight Requests
- Preflighted reqeusts first send an HTTP Options request header in order to determine whether the actual request is safe to send.

### Requests with Credentials
- Requests that are aware of HTTP Cookies and HTTP Authentication information.

## Using the XMLHttpRequest Object (XHR)
- JS provides built-in support for receiving HTML data via the XMLHttpRequest object.
- The object makes a call to a web service. 
- XMLHttpRequest makes an HTTP request and expects to receive back data in XML format.
- You can make server request using the XMLHttpRequest Object by:
  - using GET to obtain data from the server.
  - using POST to send data to the server.

## To serialize and deserialize binary data you need to:
- Use GET method and set responseType to blob and pass it to URL.createObjectURL() method
- use POST method for submitting binary data to server and set responseType to blob

## NuGet 
- Open source package management system for .NET platform 
- Simplifies the addition of third-party libraries into your code.