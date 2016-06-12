# Programming in HTML5 with JavaScript and CSS3 Notes

## Block vs Inline content
- Block content 
  - always starts on a new line and takes up the full width available (stetches out to left and right as far as can)
  - Use div for block content
- Inline content
  - does not start on a new line and only takes up as much width as necessary.
  - Use span (also used for styling) for inline content

## Id vs class
- Use id to identify a single unique element (id's are unique). The same id can be used in multiple different pages (i.e. top navigation bar), but their should only be a unique ID on the same page. 
- Use class to identify multiple elements (classes are not unique)

## HTML5 hyperlinks
- The <a> tag is always a hyperlink in HTML5. 
- The href specifies the resource URL link goes to
- You can have a target attribute that opens the linked document in a new window or tab
  - open in new tab or window: target = "_blank"
  - other options are: _self, _parent, _top, or framename. 

## CSS Origins
- Style sheets may have three different origins:
  - Author: The author of the web page define styles for the document. It specifies source document according to conventions of the document language
  - User: The reader, the user of the browser, may have a custom style sheet to tailor its experience. May specify style information for a particular document, e.g. for visual impairment
  - User agent: The browser has a basic style sheet that gives a default style to any document.Conforming user agents must apply a default style sheet (e.g. for visual browsers, the EM element in HTML is presented using an italic font). 
- By default, rules in author style sheets have more weight than rules in user style sheets
  - Precedence is reversed, however, for !important rules (https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)
  - All user and author rules have more weight than rules in the user agent's default style sheet

# CSS Specificity
- Specificity is the weight that is applied to a given CSS declaration. 
- Specificity only applies when the same element is targeted by multiple declarations. 
- When specificity is equal to any of the multiple declarations, the last declaration found in the CSS is applied to the element. https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity
- The following list of selector types is by increasing specificity:
  0. Type selectors (e.g. h1) and pseudo-elements (e.g., :before).
  1. Class selectors (e.g., .example), attributes selectors (e.g. [type="radio"]) and pseudo-classes (e.g. :hover).
  2. ID selectors (e.g. #example)