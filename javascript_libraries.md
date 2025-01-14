# sanitize-html Package

`sanitize-html` is a Node.js library used to sanitize user-generated HTML input, preventing potential security vulnerabilities like Cross-Site Scripting (XSS). It removes unwanted HTML tags, attributes, and scripts, ensuring safe HTML content.

## Installation

To install the `sanitize-html` package, you can use npm or yarn:

```bash
npm install sanitize-html
yarn add sanitize-html
const sanitizeHtml = require('sanitize-html');

const dirtyHtml = `
  <div>
    <script>alert('XSS');</script>
    <p>Hello <strong>world</strong>!</p>
  </div>
`;

const cleanHtml = sanitizeHtml(dirtyHtml);

console.log(cleanHtml);
// Output: <div><p>Hello <strong>world</strong>!</p></div>
By default, sanitize-html removes unsafe tags like <script> and keeps safe tags like <p> and <strong>.

Custom Configuration
You can customize which HTML tags and attributes are allowed using the options parameter.

Example with Custom Configuration:
javascript
Copy code
const sanitizeHtml = require('sanitize-html');

const dirtyHtml = `
  <div>
    <a href="javascript:alert('XSS')">Click me</a>
    <img src="image.jpg" onerror="alert('XSS')" />
    <p style="color: red;">This is a <strong>test</strong>.</p>
  </div>
`;

const cleanHtml = sanitizeHtml(dirtyHtml, {
  allowedTags: ['a', 'p', 'strong'], // Allow only specific tags
  allowedAttributes: {
    a: ['href'], // Allow href attribute for <a> tags
    p: ['style'], // Allow style attribute for <p> tags
  },
  allowedSchemes: ['http', 'https'], // Restrict URL schemes
});

console.log(cleanHtml);
// Output: <a>Click me</a><p style="color: red;">This is a <strong>test</strong>.</p>
Key Configuration Options
1. allowedTags
An array of allowed tag names. The sanitizer will only allow these tags and remove any others.

Default: ['b', 'i', 'em', 'strong', 'a']
javascript
Copy code
allowedTags: ['a', 'p', 'strong'] // Only allow <a>, <p>, <strong>
2. allowedAttributes
An object where each key is a tag name and its value is an array of allowed attribute names for that tag.

javascript
Copy code
allowedAttributes: {
  a: ['href'], // Allow only 'href' attribute for <a> tags
  p: ['style'], // Allow 'style' attribute for <p> tags
}
3. allowedSchemes
An array of allowed URL schemes for attributes like href or src. Common schemes are http, https, and mailto.

javascript
Copy code
allowedSchemes: ['http', 'https'] // Allow only http and https URLs
4. disallowedTagsMode
Controls how disallowed tags are handled. You can either discard them or escape them.

Options:
'discard' (default): Removes disallowed tags.
'escape': Escapes disallowed tags.
javascript
Copy code
disallowedTagsMode: 'escape' // Escape disallowed tags instead of discarding them
5. selfClosing
An array of tags that should be treated as self-closing (e.g., <img />, <br />).

javascript
Copy code
selfClosing: ['img', 'br', 'hr'] // Treat <img>, <br>, <hr> as self-closing tags
6. enforceHtmlBoundary
If set to true, ensures that sanitization does not alter HTML boundaries, preserving original structure.

javascript
Copy code
enforceHtmlBoundary: true // Enforce HTML boundary
Advanced Usage: Custom Filters
You can define custom tag transformations using the transformTags option. This allows you to modify or validate tags and their attributes.

Example:
javascript
Copy code
const cleanHtml = sanitizeHtml(dirtyHtml, {
  allowedTags: ['a', 'p'],
  transformTags: {
    a: (tagName, attribs) => {
      if (attribs.href && !attribs.href.startsWith('http')) {
        delete attribs.href; // Remove invalid hrefs
      }
      return { tagName, attribs };
    },
  },
});
Use Cases
Preventing XSS in Web Applications: Safely process user inputs, comments, or uploaded content to avoid malicious injections.
Rendering Rich Content Securely: Allow a subset of HTML for WYSIWYG (What You See Is What You Get) editors while maintaining security.
HTML Cleaning: Strip out unwanted HTML elements from user-provided content, ensuring it's safe to display.
Conclusion
The sanitize-html package is a powerful tool for ensuring that user-generated HTML content is clean, secure, and free from potentially harmful code. Its customizable configuration makes it flexible enough to suit a variety of use cases, from simple HTML sanitization to more complex scenarios where specific tags and attributes need to be preserved.

go
Copy code


This Markdown document provides a comprehensive overview of the `sanitize-html` package, including installation instructions, examples, and configuration options. You can copy and paste it into a `.md` file for your documentation needs.






