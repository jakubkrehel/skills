# Choosing fonts

`Display` and `Text` cuts, fallback stacks for a type change and the file format to serve.

## Display and Text cuts

"Display" in a font's name does not make it a display font. SF Pro and Heldane ship a `Display` variant for large sizes and a `Text` variant for small ones. Use the variant matching the size you are setting.

For numeric columns, use `tabular-nums` on the interface face before reaching for a monospace one.

## Fallback stacks for a type change

When a type change is asked for, two routes. `system-ui` gives each operating system's own interface face. A commercial face such as Helvetica Now is a brand decision and still needs a fallback stack.

```css
/* The platform's native interface face */
html {
  font-family: system-ui, sans-serif;
}

/* Commercial brand face with safe fallbacks */
html {
  font-family: "Helvetica Now", "Helvetica Neue", Arial, sans-serif;
}
```

## Formats

Serve `.woff2`. Add `.woff` only for a browser matrix old enough to need it, and ship `.ttf` or `.otf` only when nothing else exists. How the files load is the project's concern.
