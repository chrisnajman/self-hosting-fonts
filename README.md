---
permalink: /index.html
---

[Website (Git Pages)](https://chrisnajman.github.io/self-hosting-fonts)

# Self-hosting fonts

Many of us use Google fonts via a CDN. However, Germany has decided that [this contravenes GDPR](https://blog.runcloud.io/google-fonts-gdpr/), leaving site owners open to litigation.

We can still display Google fonts by:

1. Downloading the font (variants) from [Google Fonts](https://fonts.google.com/), then
2. converting the .ttf files to .woff and .woff2 using a web font generator, then
3. using @font-face in the CSS and
4. preloading the fonts in the HTML.

**Note**: This page concentrates on Google Fonts, but the same principles apply to any other kind of web font.

## Web font generators

[Font Squirrel](https://www.fontsquirrel.com/tools/webfont-generator)
This worked for "Roboto" but failed with the static font versions of both "Lato" and "Open Sans".

[Creative Fabrica](https://www.creativefabrica.com/webfont-generator/)
I used this for "Lato" and "Open Sans" static files. I had to upload each variant, one at a time.

## HTML

I was advised to preload the fonts in the head of the html page. This is supposed to speed up the delivery of the fonts and increase Google's [Core Web Vitals (CWVs)](https://support.google.com/webmasters/answer/9205520?hl=en) performance. However, I saw warnings after doing this (see Testing/Firefox and Firefox Developer Edition, below).

```html
<head>
  <link
    rel="preload"
    href="roboto-light-webfont.woff2"
    as="font"
    type="font/woff2"
    crossorigin="anonymous"
  />
  <link
    rel="preload"
    href="roboto-regular-webfont.woff2"
    as="font"
    type="font/woff2"
    crossorigin="anonymous"
  />
  <link
    rel="preload"
    href="roboto-medium-webfont.woff2"
    as="font"
    type="font/woff2"
    crossorigin="anonymous"
  />
  ...
  <!-- CSS file comes after preload -->
</head>
```

## CSS

```css
@font-face {
  font-family: "Roboto";
  font-weight: 300;
  src: url("roboto-light-webfont.woff2") format("woff2"), url("roboto-light-webfont.woff")
      format("woff");
}
/** Regular: 400 **/
@font-face {
  font-family: "Roboto";
  font-weight: 400;
  src: url("roboto-regular-webfont.woff2") format("woff2"), url("roboto-regular-webfont.woff")
      format("woff");
}
/** Medium: 500 **/
@font-face {
  font-family: "Roboto";
  font-weight: 500;
  src: url("roboto-medium-webfont.woff2") format("woff2"), url("roboto-medium-webfont.woff")
      format("woff");
}
```

## Testing

- Tested on:
- Windows 10
- Chrome
- Firefox, Firefox Developer Edition
  - **Note**. Warnings were displayed in the consoles of only these two browsers. The warnings were of 2 types:
  1.  _Layout was forced before the page was fully loaded. If stylesheets are not yet loaded this may cause a flash of unstyled content._
  2.  The other warnings were for each font variant, and occured after about 10 seconds, e.g. for "Lato":
  - _The resource at “https://chrisnajman.github.io/self-hosting-fonts/fonts/lato/lato-light-webfont.woff2” preloaded with link preload was not used within a few seconds. Make sure all attributes of the preload tag are set correctly._
  - _The resource at “https://chrisnajman.github.io/self-hosting-fonts/fonts/lato/lato-regular-webfont.woff2” preloaded with link preload was not used within a few seconds. Make sure all attributes of the preload tag are set correctly._
  - _The resource at “https://chrisnajman.github.io/self-hosting-fonts/fonts/lato/lato-bold-webfont.woff2” preloaded with link preload was not used within a few seconds. Make sure all attributes of the preload tag are set correctly._
  - Discussion and searching on Stack Overflow has not yielded an understandable answer. However, [I posted a question myself on Stack Overflow](https://stackoverflow.com/questions/75351782/why-does-firefox-show-font-preload-warning-the-resource-at-url-preloaded-with) and if it gets answered, I'll update this README.md with any new information.
- Microsoft Edge

## Acknowledgements

The bulk of the information came from [Self-hosting fonts explained (including Google fonts) // @font-face tutorial (YouTube)](https://youtu.be/zK-yy6C2Nck). Information on preloading/CWVs, and non-GDPR compliance emerged from a comments thread on the same page.
