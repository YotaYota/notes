# HTML

Hyper Text Markup Language

[web.dev HTML](https://web.dev/learn/html/welcome)

## Elements

There are two types of elements: **replaced** and **non-replaced**.

Non-replaced elements have opening and (sometimes optional) closing tags that surround them and may include text and other tags as sub-elements.

Replaced elements are replaced by objects. Most form controls are replaced with graphical user interface (UI) widget, most images are replaced by a raster or scalable image file. Being replaced by objects, each comes with a default appearance. Depending on the type of object and the browser, the applicable styles are limited. 

Void elements cannot contain text content or nested elements. Most replaced elements are void elements, but not all. Most void elements are replaced, but not all.

**Note**: mark up your HTML using lowercase letters.

**Note**: HTML should be used to structure content, not to define the content's appearance.

## Attributes

Attributes define the behavior, linkages, and functionality of elements. Attributes only appear in the opening tag. The opening tag always starts with the element type. The type can be followed by zero or more attributes, separated by one or more spaces. Most attribute names are followed by an equal sign equating it with the attribute value, wrapped with opening and closing quotation marks.

Some attributes are global and can appear within any element's opening tag. Some apply only to several elements but not all, and others are element-specific.

```html
<a href="#register" _target="self">Registration</a>
```

**Note**: quoting is always recommended.

## DOM

The Document Object Model (DOM) is the data representation of the structure and content of the HTML document. As the browser parses HTML, it creates a JavaScript object for every element and section of text encountered. These objects are called nodes-element nodes and text nodes, respectively.

The [HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API) provides access to and control of every HTML element via the DOM, providing interfaces for the HTML element and all the HTML classes that inherit from it. The [HTMLElement interface](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement) represents the HTML element and all of its descendant nodes. Every other element implements it via an interface that inherits from it.

## Structure

```html
<!DOCTYPE html> <!-- tells browser to use "standards mode" instead of "quirks mode" -->
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width" />
    <link rel="stylesheet" href="styles.css">
    <link rel="icon" type="image/png" href="/images/favicon.png" />
  </head>
  <body>

  </body>
</html>
```

**Note**: The `lang` attribute is not limited to the `<html>`tag. If there is text within the page that is in a language different from the main document language, the lang attribute should be used to identify exceptions to the main language within the document.

### HEAD

Found in `<head>` are document metadata, including:

- document title `<title>`
- character set `<meta charset>`
- viewport settings `<meta name="viewport">`
- description
- base URL `<base>`
- stylesheet links `<link rel="stylesheet>`
- icons `<link rel"icon">`

Always include

- character set. always use `<meta charset="utf-8" />`
- title
- viewport settings. helps site responsiveness, enabling content to render well by default

**Note**: The character encoding is inherited into everything in the document, even `<style>` and `<script>`.

**Note**: `charset` is actually a pragma directive. It is a shorthand for `<meta http-equiv="Content-Type" content="text/html; charset=<characterset>" />`.

viewport settings content are width, zoom and scalability. zoom and scalability default to accessible values.

`<meta name="viewport" content="width=device-width" />`

**Note**: If you don't declare a favicon, the browser will look for a file named favicon.ico in the top-level directory (the website's root folder). You can use a different file name and location like this

```html
<link rel="icon" sizes="16x16 32x32 48x48" type="image/png" href="/images/mlwicon.png" />
```

The `target` attribute, valid on `<base>` as well as on links and forms, sets where those links should open.

#### CSS

Styles should be in the head for performance reasons.

There are 3 ways to include CSS: `<link>`, `<style>`, and the `style` attribute. `<link rel="stylesheet" href="styles.css">` is preferred, partly because the browser can cache the file.

To import external style sheet styles to be within a cascade layer but you don't have access to edit the CSS file to put the layer information in it, you'll want to include the CSS with

```html
<style>
  @import "styles.css" layer(firstLayer);
</style>
```

#### `<base>`

There can only be one base element in a document. And it should come before aany relative URLs are used (including scripts and stylesheets).

 allows setting a default link URL and target. The href attribute defines the base URL for all relative links.

The target attribute, valid on <base> as well as on links and forms, sets where those links should open. The default of _self opens linked files in the same context as the current document. Other options include _blank, which opens every link in a new window, the _parent of the current content, which may be the same as self if the opener is not an iframe, or _top, which is in the same browser tab, but popped out of any context to take up the entire tab.

#### `<meta>`

Aside from the `charset` exception, all other meta tags defined in the WHATWG HTML specification contain either the `http-equiv` or `name` attribute.

There are two main types of meta tags: pragma directives, with the `http-equiv` attribute, and `name` meta types. Both the `name` and `http-equiv` meta types must include the `content` attribute, which defines the content for the type of metadata listed.

Pragma directives describe how the page should be parsed. Eg:

- `http-equiv="content-language"` (can be set with `<html lang>` instead)
- `http-equiv="refresh"`
- `http-equiv="default-style"`
- `http-equiv="content-type"`
- `http-equiv="set-cookie"`
- `http-equiv="x-ua-compatible"`
- `http-equiv="content-security-policy"`

Named meta tags includes

- `description`
- `theme-color`

avoid `keywords` since it's not useful anylonger.

`<meta name="robots" content="noindex, nofollow" />` tells robots to not index the site and not follow any links.

If you do need to include more than one type of meta tag to support legacy browsers, the legacy values should come after the newer values, as user agents read successive rules until they find a match.

[Open Graph](https://ogp.me/) protocol can be used to control how social media sites displays links to your content. These meta tags have two attributes: `property` and `content`.

Instead of adding meta tags in the html document, we can create a manifest, generally named `manifest.webmanifest` or `manifest.json`, and add it with
```html
<link rel="manifest" href="/manifest.webmanifest" />
```

### Accessibility Object Model (AOM)

As the browser parses the content received, it builds the document object model (DOM) and the CSS object model (CSSOM). It then also builds an accessibility tree. Assistive devices, such as screen readers, use the AOM to parse and interpret content. The DOM is a tree of all the nodes in the document. The AOM is like a semantic version of the DOM.
o.

The global `role` attribute (defined in [ARIA sepcification](https://w3c.github.io/aria/#dfn-role)) describes the role an element has in the context of the document.

Interactive elements, such as buttons, links, ranges, and checkboxes, all have implicit roles, all are automatically added to the keyboard tab sequence, and all have default expected user action support.

Using the role attribute, you can give any element a `role`, including a different role than the tag implies. For example, `<button>` has the implicit role of `button`. With `role="button"`, you can turn any element semantically into a button: `<p role="button">Click Me</p>`. While adding `role="button"` to an element informs screen readers that the element is a button, it doesn't change the appearance or functionality of the element. But use the correct semantic tag instead.

Some tags are **landmarks** depending on how they are nested.

- `<header>` implicit role is `banner`. is a landmark depending on how it's nested
- `<nav>`
- `<main>`
- `<article>`
- `<section>`
- `<aside>`
- `<footer>` implicit role is `contentinfo` if it is a landmark.

A layout with a header, two sidebars, and a footer, is known as the [holy grail layout](https://web.dev/patterns/layout/holy-grail), eg

```html
<body>
  <header>Header</header>
  <nav>Nav</nav>
  <main>Content</main>
  <aside>Aside</aside>
  <footer>Footer</footer>
</body>
```
## Tags

### `<link>`

The `link` element is used to create relationships between the HTML document and external resources, eg downloads or informational. The type of relationship is defined by the value of the `rel` attribute. There are currently 25 available values for the `rel` attribute that can be used with `<link>`, `<a>` and `<area>`, or `<form>`

Eg, using `alternate` for a translation, then `hreflang` attribute must be set.

```html
<link rel="alternate" href="https://www.machinelearningworkshop.com/fr/" hreflang="fr-FR" />
<link rel="alternate" href="https://www.machinelearningworkshop.com/pt/" hreflang="pt-BR" />
```

### `<script>`

The `<script>` tag is used to include scripts; default type is JavaScript. For any other scripting language, include the `type` attribute with the mime type, or `type="module"` if it
s a JavaScript module. Only JavaScript and JavaScript modules get parsed and executed.

`<script>` tags can be used to encapsulate your code or to download an external file.

JavaScript is not only render-blocking, but the browser stops downloading all assets when scripts are downloaded and doesn't resume downloading other assets until the JavaScript has finished execution. For this reason, you will often find JavaScript requests at the end of the document rather than in the head. Also, with JavaScript, you don't want to reference an element before it exists.

There are two attributes that can reduce the blocking nature of JavaScript download and execution:

- `defer`: HTML rendering is not blocked during the download, and the JavaScript only executes after the document has otherwise finished rendering
- `async`: rendering isn't blocked during the download, but once the script has finished downloading, the rendering is paused while the JavaScript is executed.

## Events

### MouseEvents

- onMouseOver and onMouseOut

Events occur when the mouse cursor is placed over specific element.

- onMouseUp and onMouseDown

Events occur when a mouse click is encountered.

- onMouseEnter and onMouseLeave

Event occurs when the mouse is placed on (or removed from) the element. onMouseEnter stays until the mouse is removed from the element.
