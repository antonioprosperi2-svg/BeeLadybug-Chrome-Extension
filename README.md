![Bug](https://beeenginejs.com/wp-content/uploads/2026/11/promo1400x560.png)


# BeeLadybug

Inspect layout and CSS problems on the page you are looking at, then download an HTML report. BeeLadybug is a Manifest V3 Chrome extension. Version 1.0.5.

[Install from the Chrome Web Store](https://chromewebstore.google.com/detail/beeladybug/hoeblomajmbdbgkklcggiiolhigfkpak)


[![Version](https://img.shields.io/badge/version-1.0.5-2ea44f)](manifest.json)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![GitHub release](https://img.shields.io/github/v/release/antonioprosperi2-svg/BeeLadybug-Chrome-Extension)](https://github.com/antonioprosperi2-svg/BeeLadybug-Chrome-Extension/releases)
[![npm engine](https://img.shields.io/npm/v/bee-ladybug?label=engine%20on%20npm&color=red)](https://www.npmjs.com/package/bee-ladybug)
[![Vanilla JS](https://img.shields.io/badge/vanilla-JavaScript-f7df1e?logo=javascript&logoColor=black)](#)
[![No frameworks](https://img.shields.io/badge/frameworks-none-brightgreen)](#)
[![Chrome Web Store Users](https://img.shields.io/chrome-web-store/users/hoeblomajmbdbgkklcggiiolhigfkpak?color=green)](https://chromewebstore.google.com/detail/beeladybug/hoeblomajmbdbgkklcggiiolhigfkpak)
[![View sample report](https://img.shields.io/badge/View%20sample%20report-HTML-ff3b5c?style=flat)](https://beeenginejs.com/wp-content/uploads/2026/11/report-20261122-1736.html)
## What it finds

Open the HUD on a normal page and BeeLadybug starts two inspectors: CSS and DOM. A page that contains a `<canvas>` starts the canvas inspector instead, and leaves CSS and DOM off until you turn them on.

The CSS and DOM scans skip `script`, `style`, `link`, and `meta`, nodes with `display: none`, `visibility: hidden`, or a zero-size box, the WordPress admin bar (`#wpadminbar` and classes that start with `ab-`), and BeeLadybug's own overlay. The CSS section of a report keeps at most 80 rows. The DOM problem list stops at 40 rows.

### CSS

These checks run while you hover an element, when you press **S** to scan the page, and again when you export a report.

- **Viewport overflow.** The element extends more than 1px past the left or right edge of the viewport. `html` and `body` are ignored. If a child sticks out by the same amount, the parent is not listed again. Inside a container that clips overflow this is info. Otherwise it is an error.
- **Covered clicks.** The center of the element is covered by another element, so a click there would hit the cover. A fixed or absolute `::before` or `::after` on `html` or `body` counts. The report text is "possible click blocked (verify)".
- **Cookie banner.** If the cover looks like a cookie or consent banner (an id or class matching `cookie`, `consent`, `onetrust`, `ot-sdk`, or `cookiebot`), it is reported once as "cookie banner open, covering page" instead of as a blocked click.
- **Invisible text.** The element has text of its own, or it is a `button`, `a`, `label`, `input`, or `textarea`. The text is treated as invisible when opacity is 0 on the element or an ancestor, when the text color is transparent, or when the text color matches the background painted behind it.
- **`!important` rules.** Readable stylesheets are walked for declarations marked `!important`. Media queries are included only when they currently match. Each element that matches the selector is listed with the property, value, selector, and stylesheet. This is an info row: it shows which `!important` rules apply. A stylesheet that throws when its rules are read (usually a cross-origin sheet) is noted as "unreadable sheet", and its rules are skipped.
- **Negative margin.** A computed `margin-top`, `margin-right`, `margin-bottom`, or `margin-left` below zero. Screen-reader-only boxes, `aria-hidden="true"` content, and elements inside a clipping container are skipped.
- **Distorted image.** An `img` with `object-fit: fill` (the initial value) whose rendered width/height ratio differs from the image file's ratio by more than 5%.
- **Stacking context.** The element has a `z-index` other than `auto`, and an ancestor creates a stacking context. The ancestor qualifies when it has a transform other than `none`, opacity below 1, a filter other than `none`, or a non-static position together with a `z-index`. If something else covers the element's center, the row is an error. Otherwise it is info. `will-change: transform` or `will-change: opacity` on an ancestor is reported as a hint.

### DOM

- **Box model.** Hovering an element outlines its margin, border, padding, and content, and the panel shows the border-box size and `display`.
- **Collapsed content.** The border box has both width and height, but the content box has a width or height of 0. A lazy-loaded `img` that has not loaded yet is skipped.
- **Content larger than the client box.** Reported with the rule name `clipped`. `scrollWidth` or `scrollHeight` is more than 2px above the client box. The row is added when the element itself does not use overflow `hidden`, `auto`, `scroll`, or `clip`, and no ancestor does either. Screen-reader-only boxes and `aria-hidden="true"` content are skipped.
- **Negative margin.** The same negative-margin check, included in the DOM snapshot as info.
- **Mutations.** A count of nodes added, nodes removed, and changes to `class`, `style`, `hidden`, `disabled`, and `open` under `document.body`. A count above zero is a warning in the report. It records activity on the page.

### Canvas

When the page has a `<canvas>` that is not one of BeeLadybug's overlays, the HUD draws hitboxes for game entities.

`entity-bridge.js` reads `window.__BEE_ENTITIES__` every 100ms and posts a plain copy to the same page. The copy keeps `name`, `id`, `label`, `role`, `x`, `y`, `worldX`, `worldY`, `width` or `w`, `height` or `h`, `active`, `colliding`, and `primary`. A box is marked as colliding when its entity says `colliding`, or when two boxes overlap. The report includes each hitbox and, once frames have been sampled, the FPS min, average, and max. If the page never sets `__BEE_ENTITIES__`, the event log says that no entities were found. That message is in Italian.

## How to use

1. Open the page you want to inspect. BeeLadybug can run only on a normal site. It refuses `chrome://`, `chrome-extension://`, `edge://`, `about:`, `devtools:`, `view-source:`, `chrome.google.com`, and `chromewebstore.google.com`. On those pages the popup stays open and explains why, in Italian.
2. Click the BeeLadybug toolbar icon, then click **Attiva/Disattiva HUD**. The popup injects the scripts into that tab and closes. Click the icon and the same button again to remove the HUD.
3. Read the panel on the page. **STATUS** shows frame rate and the simulated clock. **ISSUES** lists causes found while you hover, or after a scan. **INSPECT** shows the findings for the element under the pointer. **EVENTS** is the log. On a normal page, DOM and CSS start on. Move the pointer to see the box model. Press **S** to scan the whole page (this is ignored while you are typing in a field). Press **P** to pin the current element. **F6** turns the CSS hunt on or off. **F2** hides or shows the overlay. Drag the panel, or use **WIDE** and **PILL** to resize it. On a page with a canvas, the canvas view starts first. Use the **DOM** and **F6 CSS** buttons to turn the other inspectors on.
4. Export the HTML report. Click **F8 EXPORT** or press **F8**. The tab downloads `report-YYYYMMDD-HHMM.html`. The file is built in the page. It contains the URL, the viewport, the log, and a snapshot from each attached inspector. The CSS snapshot runs even when the live hunt is off.

**F3 SLOW** sets the simulated clock to 0.25×. **F4 STOP** freezes that clock. **1x LIVE** restores it. Freeze stops the simulated clock only. The overlay keeps running.

The panel markup still has an Ask AI button. This build hides it. No assistant is registered.

## Install manually

1. Download the zip from the [latest GitHub release](https://github.com/antonioprosperi2-svg/BeeLadybug-Chrome-Extension/releases/latest) and unzip it.
2. Open `chrome://extensions` and turn on **Developer mode**.
3. Click **Load unpacked** and select the unzipped folder.

## Permissions

The manifest asks for two permissions:

- **`activeTab`.** After you click the toolbar icon, Chrome grants access to the tab you are looking at. The extension does not run on other tabs, and it does not run until you click it.
- **`scripting`.** The popup uses `chrome.scripting.executeScript` to inject `entity-bridge.js` and then `content.js` into that tab. Both files run in the page's main JavaScript world, so the bridge can read `window.__BEE_ENTITIES__` when a game exposes it.

There are no host permissions and no `storage` permission. The panel remembers its size and position in that page's `localStorage`, under the key `bee-ladybug.panel`. The HTML report is a local download. Entity data is posted to the same window with `postMessage`.

BeeLadybug does not send page content, reports, or entity data to any server.

## Project structure

This is a plain JavaScript extension. There is no framework, no `package.json`, and no build step in this repository. Chrome loads the files below as they are.

| File | What it does |
| --- | --- |
| `manifest.json` | Manifest V3 entry. Name, version 1.0.5, `activeTab` and `scripting`, the popup, and the icons. |
| `popup.html` | The small popup. One button, **Attiva/Disattiva HUD**, and a status line. |
| `popup.js` | Checks that the tab can be inspected, injects the two page scripts, then closes the popup. Clicking again removes the HUD. |
| `content.js` | The script Chrome injects. It contains the HUD, the DOM, CSS, and canvas inspectors, the HTML report, and the on/off boot. The bundled engine version string inside this file is 0.6.14. |
| `content.src.js` | The extension boot written as an ES module (`pagePlan`, entity messages, hiding the Ask button). It imports the engine from `../src/index.js`, which is not in this repository. The popup injects `content.js`, not this file. |
| `entity-bridge.js` | Runs in the page. Copies `window.__BEE_ENTITIES__` into a `postMessage` every 100ms. |
| `icons/` | `icon16.png`, `icon48.png`, and `icon128.png`. |

## Demo

![BeeLadybug HUD on a page](docs/demo.gif)

## The engine behind it

The inspectors, the HUD, and the report come from the BeeLadybug npm package. The extension wires that engine to a toolbar button. The package source, and how to use it outside Chrome, is in the main repository:

https://github.com/antonioprosperi2-svg/BeeLadybug-universal

## Project docs

- [Changelog](CHANGELOG.md)
- [Contributing](CONTRIBUTING.md)
- [Code of conduct](CODE_OF_CONDUCT.md)
- [Security](SECURITY.md)
- [License](LICENSE) (MIT, copyright 2026 Antonio Prosperi)
