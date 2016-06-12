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