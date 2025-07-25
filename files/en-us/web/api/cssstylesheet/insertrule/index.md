---
title: "CSSStyleSheet: insertRule() method"
short-title: insertRule()
slug: Web/API/CSSStyleSheet/insertRule
page-type: web-api-instance-method
browser-compat: api.CSSStyleSheet.insertRule
---

{{APIRef("CSSOM")}}

The **`CSSStyleSheet.insertRule()`**
method inserts a new [CSS rule](/en-US/docs/Web/API/CSSRule) into the [current style sheet](/en-US/docs/Web/API/CSSStyleSheet).

> [!NOTE]
> Although `insertRule()` is exclusively a method of
> {{domxref("CSSStyleSheet")}}, it actually inserts the rule into
> `{{domxref("CSSStyleSheet", "", "", "1")}}.cssRules` — its internal
> {{domxref("CSSRuleList")}}.

## Syntax

```js-nolint
insertRule(rule)
insertRule(rule, index)
```

### Parameters

- `rule`
  - : A string containing the rule to be inserted. What the inserted
    rule must contain depends on its type:
    - **For [rule-sets](/en-US/docs/Web/CSS/CSS_syntax/Syntax#css_statements)**, both
      a [selector](/en-US/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) and a
      style declaration.
    - **For [at-rules](/en-US/docs/Web/CSS/CSS_syntax/At-rule)**, both an
      at-identifier and the rule content.

- `index` {{optional_inline}}
  - : A positive integer less than or equal to `stylesheet.cssRules.length`,
    representing the newly inserted rule's position in
    `{{domxref("CSSStyleSheet", "", "", "1")}}.cssRules`. The default is
    `0`. (In older implementations, this was required. See [Browser compatibility](#browser_compatibility) for details.)

### Return value

The newly inserted rule's index within the stylesheet's rule-list.

### Exceptions

- `IndexSizeError` {{domxref("DOMException")}}
  - : Thrown if `index` > `{{domxref("CSSRuleList", "", "", "1")}}.length`.
- `HierarchyRequestError` {{domxref("DOMException")}}
  - : Thrown if `rule` cannot be inserted at the specified index due to some CSS constraint; for instance: trying to insert an {{cssxref("@import")}} at-rule after a style rule.
- `SyntaxError` {{domxref("DOMException")}}
  - : Thrown if more than one rule is given in the `rule` parameter.
- `InvalidStateError` {{domxref("DOMException")}}
  - : Thrown if `rule` is {{cssxref("@namespace")}} and the rule-list has more than just `@import` at-rules and/or `@namespace` at-rules.

## Examples

### Inserting a new rule

This snippet pushes a new rule onto the top of my stylesheet.

```js
myStyle.insertRule("#blanc { color: white }", 0);
```

### Function to add a stylesheet rule

```js
/**
 * Add a stylesheet rule to the document (it may be better practice
 * to dynamically change classes, so style information can be kept in
 * genuine stylesheets and avoid adding extra elements to the DOM).
 * Note that an array is needed for declarations and rules since ECMAScript does
 * not guarantee a predictable object iteration order, and since CSS is
 * order-dependent.
 * @param {Array} rules Accepts an array of JSON-encoded declarations
 * @example
addStylesheetRules([
  ['h2', // Also accepts a second argument as an array of arrays instead
    ['color', 'red'],
    ['background-color', 'green', true] // 'true' for !important rules
  ],
  ['.myClass',
    ['background-color', 'yellow']
  ]
]);
*/
function addStylesheetRules(rules) {
  const styleEl = document.createElement("style");

  // Append <style> element to <head>
  document.head.appendChild(styleEl);

  // Grab style element's sheet
  const styleSheet = styleEl.sheet;

  for (let rule of rules) {
    let i = 1,
      selector = rule[0],
      propStr = "";
    // If the second argument of a rule is an array of arrays, correct our variables.
    if (Array.isArray(rule[1][0])) {
      rule = rule[1];
      i = 0;
    }

    for (; i < rule.length; i++) {
      const prop = rule[i];
      propStr += `${prop[0]}: ${prop[1]}${prop[2] ? " !important" : ""};\n`;
    }

    // Insert CSS Rule
    styleSheet.insertRule(
      `${selector}{${propStr}}`,
      styleSheet.cssRules.length,
    );
  }
}
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("CSSStyleSheet.deleteRule")}}
- [Constructable Stylesheets](https://web.dev/articles/constructable-stylesheets) (web.dev)
