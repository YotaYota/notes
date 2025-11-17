# CSS

`px` `em` `rem`

## `font-size`

On the web, the default font size is `16px`.

- `1px` is equal to whatever the browser is treating as a single pixel (even if it’s not literally a pixel on the hardware screen).
- `1rem` (root element) is the font-size of the `html` element.
- `em` is the font-size of the *current* element.

**Note**: `em` and `rem` are relative, while `px` is static. `px` values do not scale up or down when the user changes their font size, while `em` and `rem` values do adjust in proportion to font size.

`font-size: 1em` is equivalent to `font-size: 100%`. **Note**: Not always equivalent considering other properties than font-size, eg width.

**Note**: `em` and `rem` are tied to `font-size` **not** zoom level.

**Note**: using `rem` (or `em`), id the user changes the font size, all those elements will be affected - as it should. This is also a very good reason to avoid viewport units, like `vw` or `vh`, when setting font size. Those are also static, and impossible to override by the user (use `clamp()` or media queries instead to set minimum and maximum values).

**Note**: Do not use `px` anywhere, except for design elements that you explicitly do not want to scale with font size.

**Note** Avoid `px` in `@media` queries, since it is not affected if the user changes it's font size.

