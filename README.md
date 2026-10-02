This repo was created to reproduce https://github.com/saadeghi/daisyui/issues/4488

This was created as a PHP (Laravel) website, with Vite.

Interestingly, `--color-secondary` happens to be the same for DaisyUI's default `light` and `dark` theme (`--color-secondary: oklch(65% .241 354.308);`).<br/>
For that reason, we will look specifically for that variable in a CSS where these 2 themes are overridden.

## Issue this repo demonstrates

As usual, `npm run build` creates the website's CSS file.

`-color-secondary` in the `light` and `dark` themes are:
 - `light`: `--color-secondary: oklch(94.7% 0.026 71.191);`
 - `dark`: `--color-secondary: oklch(27.581% 0.064 261.069);`

In the output CSS, we can find:
 - 3 occurrences of the original `--color-secondary` divided in:
      - 2x for the `light` theme:
         ```
         :where(:root),
        [data-theme=light] { ...
        ```
        and
        ```
        :root:has(input.theme-controller[value=light]:checked) { ...
        ```
      - 1x for the `dark` theme:
        ```
        @media (prefers-color-scheme:dark) {
        :root:not([data-theme]) { ...
        ```
 - 3 occurrences of the modified values, divided in:
     - 1x for the `light` theme:
       ```
       :is(:root:has(input.theme-controller[value=light]:checked), [data-theme=light]) { ...
       ```
     - 2x for the `dark` theme:
       ```
       @media (prefers-color-scheme:dark) {
        :root:not([data-theme]) { ...
       ```
       and
       ```
       :is(:root:has(input.theme-controller[value=dark]:checked), [data-theme=dark]) { ...
       ```

## NPM packages

Here is the output of `npm list`

```
daisy-4488@ /var/www/daisy-4488
├── @laravel/multiplex@0.4.5
├── @tailwindcss/vite@4.3.3
├── concurrently@10.0.5
├── daisyui@5.7.47
├── laravel-vite-plugin@3.2.0
├── tailwindcss@4.3.3
└── vite@8.3.2
```

## Files location

 - Source CSS: `resources/css`
 - Output CSS: `public/build/assets`
