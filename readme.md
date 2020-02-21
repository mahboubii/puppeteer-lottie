# puppeteer-lottie

> Renders [Lottie](http://airbnb.io/lottie) animations via [Puppeteer](https://github.com/GoogleChrome/puppeteer) to **image**, **GIF** or **MP4**.

[![NPM](https://img.shields.io/npm/v/puppeteer-lottie.svg)](https://www.npmjs.com/package/puppeteer-lottie) [![Build Status](https://travis-ci.com/transitive-bullshit/puppeteer-lottie.svg?branch=master)](https://travis-ci.com/transitive-bullshit/puppeteer-lottie) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

<p align="center">
  <img width="100%" alt="Logo" src="https://raw.githubusercontent.com/transitive-bullshit/puppeteer-lottie/master/media/logo.gif">
</p>

This module is also available as a [CLI](https://github.com/transitive-bullshit/puppeteer-lottie-cli).

## Install

```bash
npm install --save puppeteer-lottie
```

If you want to generate **GIFs**, you must also install [gifski](https://gif.ski/). On macOS, you can run:

```bash
brew install gifski
```

If you want to generate **MP4s**, you must also install [ffmpeg](https://ffmpeg.org/). On macOS, you can run:

```bash
brew install ffmpeg
```

## Usage

```js
const renderLottie = require('puppeteer-lottie')

// Create an MP4 from a lottie animation
await renderLottie({
  path: 'fixtures/bodymovin.json',
  output: 'example.mp4'
})

// Create a GIF with width 640px from a lottie animation
await renderLottie({
  path: 'fixtures/bodymovin.json',
  output: 'example.gif',
  width: 640
})

// Render each frame of the animation as PNG images with height 128px
await renderLottie({
  path: 'fixtures/bodymovin.json',
  output: 'frame-%d.png', // sprintf format
  height: 128
})

// Render the first frame of the animation as a JPEG image
await renderLottie({
  path: 'fixtures/bodymovin.json',
  output: 'preview.jpg'
})
```

#### Output Size

If you don't pass `width` or `height`, the animation's intrinsic size will be used, and if that doesn't exist it will use 640x480.

If you pass `width` or `height`, the other dimension will be inferred by maintaining the original animation's aspect ratio.

If both `width` and `height` are passed, the output will have those dimensions, but there will be extra whitespace (or transparency if rendering PNGs) due to `lottie-web`'s default `rendererSettings.preserveAspectRatio` of `xMidyMid meet` ([docs](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/preserveAspectRatio) and [demo](https://codepen.io/giodif/pen/VYpaeo)).

For `mp4` outputs, the height may be different by a pixel due to the `x264` encoder requiring even heights.

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### [renderLottie](https://git@github.com/:mahboubii/puppeteer-lottie/blob/bcaa60f976f60ac342bda0fbd31f13353f8b2995/index.js#L60-L416)

Renders the given Lottie animation via Puppeteer.

You must pass either `path` or `animationData` to specify the Lottie animation.

`output` must be one of the following:

-   An image to capture the first frame only (png or jpg)
-   An image pattern (eg. sprintf format 'frame-%d.png' or 'frame-%012d.jpg')
-   An mp4 video file (requires FFmpeg to be installed)
-   A GIF file (requires Gifski to be installed)

Type: `function (opts): Promise`

-   `opts` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Configuration options
    -   `opts.output` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Path or pattern to store result
    -   `opts.animationData` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** JSON exported animation data
    -   `opts.path` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Relative path to the JSON file containing animation data
    -   `opts.width` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Optional output width
    -   `opts.height` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)?** Optional output height
    -   `opts.jpegQuality` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** JPEG quality for frames (does nothing if using png) (optional, default `90`)
    -   `opts.quiet` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Set to true to disable console output (optional, default `false`)
    -   `opts.deviceScaleFactor` **[number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Window device scale factor (optional, default `1`)
    -   `opts.renderer` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Which lottie-web renderer to use (optional, default `'svg'`)
    -   `opts.rendererSettings` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** Optional lottie renderer settings
    -   `opts.puppeteerOptions` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** Optional puppeteer launch settings
    -   `opts.gifskiOptions` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** Optional gifski settings (only for GIF outputs)
    -   `opts.style` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Optional JS [CSS styles](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference) to apply to the animation container (optional, default `{}`)
    -   `opts.inject` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Optionally injects arbitrary string content into the head, style, or body elements. (optional, default `{}`)
        -   `opts.inject.head` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Optionally injected into the document <head>
        -   `opts.inject.style` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Optionally injected into a <style> tag within the document <head>
        -   `opts.inject.body` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)?** Optionally injected into the document <body>
    -   `opts.browser` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)?** Optional puppeteer instance to reuse

## Compatibility

All [lottie-web](https://github.com/airbnb/lottie-web) features should be fully supported by the `svg`, `canvas`, and `html` renderers.

This includes all of the animations on [lottiefiles.com](https://lottiefiles.com/)! 🔥

Also see Lottie's full list of [supported features](https://airbnb.io/lottie/#/supported-features).

## Related

-   [puppeteer-lottie-cli](https://github.com/transitive-bullshit/puppeteer-lottie-cli) - CLI for this module.
-   [puppeteer](https://github.com/GoogleChrome/puppeteer) - Headless Chrome Node API.
-   [awesome-puppeteer](https://github.com/transitive-bullshit/awesome-puppeteer) - Curated list of awesome puppeteer resources.
-   [lottie](http://airbnb.io/lottie) - Render After Effects animations natively on Web, Android and iOS, and React Native.

## License

MIT © [Travis Fischer](https://transitivebullsh.it)
