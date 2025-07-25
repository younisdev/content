---
title: "URLPattern: username property"
short-title: username
slug: Web/API/URLPattern/username
page-type: web-api-instance-property
browser-compat: api.URLPattern.username
---

{{APIRef("URL Pattern API")}} {{AvailableInWorkers}}

The **`username`** read-only property of the {{domxref("URLPattern")}} interface is a
string containing the pattern used to match the username part
of a URL. This value may differ from the input to the constructor due to
normalization.

## Value

A string.

## Examples

The below example creates a {{domxref("URLPattern")}} object with `admin` for
the `username` part. This pattern matches only if the username part of the URL
is `admin`.

```js
const pattern = new URLPattern({ username: "admin" });
console.log(pattern.username); // 'admin'
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}
