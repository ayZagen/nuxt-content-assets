# Nuxt Content Assets

> Enable locally-located assets in Nuxt Content

[![npm version][npm-version-src]][npm-version-href]
[![npm downloads][npm-downloads-src]][npm-downloads-href]
[![License][license-src]][license-href]
[![Nuxt][nuxt-src]][nuxt-href]

## Overview

Nuxt Content Assets enables locally-located assets in your [Nuxt Content](https://content.nuxtjs.org/) folder:

```
+- content
    +- posts
        +- 2023-01-01
            +- index.md
            +- media
                +- featured.png
                +- mountains.jpg
                +- seaside.mp4
```

In your documents, simply reference assets using relative paths:

```markdown
---
title: Summer Holiday
featured: media/featured.png
---

I loved being in the mountains.

![mountains](media/mountains.png)

Almost as much as being in the sea!

<video src="media/seaside.mp4"></video>
```

The module:

- supports configurable image, media and file types
- supports appropriate HTML tags
- converts markdown and frontmatter content

## Demo

To run the demo online, go to:

- https://stackblitz.com/github/davestewart/nuxt-content-assets?file=demo%2Fapp.vue

You can browse the demo files in:

- .https://github.com/davestewart/nuxt-content-assets/tree/main/demo

To run the demo locally, clone the application and from the root, run:

```
npm run demo
```

## Setup

Install the dependency:

```bash
npm install nuxt-content-assets
```

Configure `nuxt.config.ts`:

```js
export default defineNuxtConfig({
  modules: [
    'nuxt-content-relative-assets', // make sure to add before content!
    '@nuxt/content',
  ]
})
```

Run the dev server or build and assets should now be served alongside markdown content.

## Usage

### Overview

Once the build or dev server is running, paths should be rewritten and assets served automatically. 

Here's how it works:

- the module scans content folders for assets
- these are copied to a temporary build folder
- matching relative paths in markdown are updated
- Nitro serves the assets from the new location

Right now, if you change any assets, you will need to re-run the dev server / build.

### Supported formats

The following file formats are supported out of the box:

| Type   | Extensions                                                              |
|--------|-------------------------------------------------------------------------|
| Images | `png`, `jpg`, `jpeg`, `gif`, `svg`, `webp`                              |
| Media  | `mp3`, `m4a`, `wav`, `mp4`, `mov`, `webm`, `ogg`, `avi`, `flv`, `avchd` |
| Files  | `pdf`, `doc`, `docx`, `xls`, `xlsx`, `ppt`, `pptx`, `odp`, `key`        |

See the [configuration](#output) section for more options.

### Images

The module [optionally](#image-attributes) writes `width` and `height` attributes to the generated HTML:

```html
<img src="..." width="640" height="480">
```

This locks the aspect ratio of the image preventing content jumps.

If you use custom [ProseImg](https://content.nuxtjs.org/api/components/prose) components, you can even grab these values using the Vue `$attrs` property:

```vue
<template>
  <div class="image">
    <img :src="$attrs.src" :style="`aspect-ratio:${$attrs.width}/${$attrs.height}`" />
  </div>
</template>

<script>
export default {
  inheritAttrs: false
}
</script>
```

See the [Demo](demo/components/_content/ProseImg.vue) for an example.

## Configuration

The module **doesn't require** any configuration, but you can tweak the following settings:

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  'content-assets': {
    // where to generate and serve the assets from
    output: 'assets/content/[path]/[file]',
    
    // add additional extensions
    additionalExtensions: 'html',
    
    // completely replace supported extensions
    extensions: 'png jpg',
    
    // add image width and height
    imageAttrs: true,
    
    // print debug messages to the console
    debug: true,
  }
})
```

### Output

The output path can be customised using a template string:

```
assets/img/content/[path]/[name][extname]
```

The first part of the path should be public root-relative folder:

```
assets/img/content
```

The optional second part of the path indicates specific placement of each image: 

| Token       | Description                                | Example            |
|-------------|--------------------------------------------|--------------------|
| `[folder]`  | The relative folder of the file            | `posts/2023-01-01` |
| `[file]`    | The full filename of the file              | `featured.jpg`     |
| `[name]`    | The name of the file without the extension | `featured`         |
| `[hash]`    | A hash of the absolute source path         | `9M00N4l9A0`       |
| `[extname]` | The full extension with the dot            | `.jpg`             |
| `[ext]`     | The extension without the dot              | `jpg`              |

For example:

| Template                             | Output                                             |
|--------------------------------------|----------------------------------------------------|
| `assets/img/content/[folder]/[file]` | `assets/img/content/posts/2023-01-01/featured.jpg` |
| `assets/img/[name]-[hash].[ext]`     | `assets/img/featured-9M00N4l9A0.jpg`               |
| `content/[hash].[ext]`               | `content/9M00N4l9A0.jpg`                           |

Note that the module defaults to all files in a single folder:

```
/assets/content/[name]-[hash].[ext]
```

### Extensions

You can add or replace supported extensions if you need to:

To add extensions, use `additionalExtensions`:

```ts
{
  additionalExtensions: 'html' // add support for html
}
```

To completely replace supported extensions, use `extensions`:

```ts
{
	extensions: 'png jpg' // serve png and jpg files only
}
```

### Image attributes

The module automatically adds `width` and `height` attributes to images.

Opt out of this by passing `false`:

```ts
{
  imageAttrs: false
}
```

## Development

Should you wish to develop the project, the scripts are:

```bash
# install dependencies
npm install

# generate type stubs
npm run dev:prepare

# develop with the demo
npm run dev

# build the demo
npm run dev:build

# run eslint
npm run lint

# run vitest
npm run test
npm run test:watch

# release new version
npm run release
```

<!-- Badges -->
[npm-version-src]: https://img.shields.io/npm/v/nuxt-content-assets/latest.svg?style=flat&colorA=18181B&colorB=28CF8D
[npm-version-href]: https://npmjs.com/package/nuxt-content-assets

[npm-downloads-src]: https://img.shields.io/npm/dm/nuxt-content-assets.svg?style=flat&colorA=18181B&colorB=28CF8D
[npm-downloads-href]: https://npmjs.com/package/nuxt-content-assets

[license-src]: https://img.shields.io/npm/l/nuxt-content-assets.svg?style=flat&colorA=18181B&colorB=28CF8D
[license-href]: https://npmjs.com/package/nuxt-content-assets

[nuxt-src]: https://img.shields.io/badge/Nuxt-18181B?logo=nuxt.js
[nuxt-href]: https://nuxt.com