# Programming in HTML5 with JavaScript and CSS3 Notes

# CSS3

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

## CSS Specificity
- Specificity is the weight that is applied to a given CSS declaration. 
- Specificity only applies when the same element is targeted by multiple declarations. 
- When specificity is equal to any of the multiple declarations, the last declaration found in the CSS is applied to the element. https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity
- The following list of selector types is by increasing specificity:
  0. Type selectors (e.g. h1) and pseudo-elements (e.g., :before).
  1. Class selectors (e.g., .example), attributes selectors (e.g. [type="radio"]) and pseudo-classes (e.g. :hover).
  2. ID selectors (e.g. #example)

# HTML5

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
  - encodeURI and decodeURI are inteded for use on the full URI
  - encodeURIComponent and decodeURIComponent are inteded to be used on URI components i.e. any part that lies between separators (; / ? : @ & = + $ , #)
- Look up the JQuery: $.get calls for AJAX


## Regular Expressions
- When validating input, include the leading caret (^) and trailing dollar to avoid security vulnerabilities.
  - ^ means start of input; $ means end of input

# JSON

## JSON is a subset of OLN
- Objects, arrays, strings, finite numbers, true, false, and null are fully supported so they can be serialized and restored.

## Limitations of JSON
- Only the enumerable own properties of an object are serialized 
- NaN, Infinity, and -Infinity are serialized to null
- Date objects are serialized to ISO-formatted date strings, but JSON.parse() leaves it as a string rather than restoring the Date
- Function, RegExp, and Error objects and the undefined value cannot be serialized; if a property value cannot be serialized, that property is simply omitted from the stringified output.




