# Catalogue Flipbooks

Static HTML viewers for a set of 19th-century trade catalogues, part of the [Exhibiting Empire](https://www.ljmu.ac.uk/microsites/exhibiting-empire/catalogues-one) project at LJMU. Each catalogue opens as an interactive page-flip PDF reader in the browser.

| Year | Source PDF |
|------|-----------|
| 1840 | catalogue-1840.pdf |
| 1842 | catalogue-1842.pdf |
| 1844 | catalogue-1844.pdf |
| 1861 | catalogue-1861.pdf |

## Running locally

Each catalogue is a self-contained folder with an `index.html` that can be opened directly in a browser. A local server is recommended as browsers block PDF loading from `file://` URLs:

```bash
npx serve .
```

Catalogues are available at `http://localhost:3000/1840/`, `/1842/`, etc.

## Structure

Each catalogue is a single `index.html` in a year-named folder, pointing to the PDF at its LJMU URL. The structure is:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>1865</title>
    <link href="https://cdn.jsdelivr.net/npm/dearflip/css/dflip.min.css" rel="stylesheet" type="text/css" />
  </head>
  <body>
    <div
      class="_df_book"
      id="flipbook"
      data-option="option_flipbook"
      source="https://www.ljmu.ac.uk/-/media/files/ljmu/microsites/exhibiting-empire/catalogues-i/catalogue-1865.pdf"
    ></div>
    <script src="https://cdn.jsdelivr.net/npm/jquery/dist/jquery.min.js" type="text/javascript"></script>
    <script src="https://cdn.jsdelivr.net/npm/dearflip/dflip.min.js" type="text/javascript"></script>
    <script>
      var option_flipbook = { webgl: false };
    </script>
  </body>
</html>
```

`webgl: false` avoids GPU rendering artefacts on scanned PDFs. Image-based PDFs can use `true` for the 3D page-curl effect.

## Bookmarks / table of contents

DearFlip's outline panel is driven by bookmarks embedded in the PDF itself. [cpdf](https://community.coherentpdf.com/) was used to inject a bookmark tree into each catalogue PDF before uploading it to LJMU.

Bookmarks are defined in a plain-text file where each line is `<depth> "<title>" <page>` (depth 0 = top-level, 1 = child):

```
0 "Cover" 1
0 "Section One" 5
1 "Subsection" 9
```

Then embedded with:

```bash
cpdf -add-bookmarks bookmarks.txt input.pdf -o output.pdf
```

## Features

- Page-flip animation (2D or 3D)
- Single / double-page view (auto-detects viewport width)
- Zoom in/out and fit-to-screen
- Full-screen mode
- In-PDF text search
- Page thumbnail strip
- PDF outline / bookmark panel
- Download and print controls
- Page-turn sound
- Auto-play slideshow
- LTR / RTL reading direction
- Responsive layout

## Dependencies

- [DearFlip](https://js.dearflip.com/) by [DearHive](https://github.com/dearhive) - jQuery PDF flipbook plugin
- [jQuery](https://jquery.com/)

## Credits

DearFlip by [DearHive](https://github.com/dearhive).

- Docs: https://js.dearflip.com/docs/
- GitHub: https://github.com/dearhive/dearflip-js-flipbook
- Support / discussions: https://github.com/dearhive/dearflip-js-flipbook/discussions
