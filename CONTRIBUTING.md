# Contributing to BeeLadybug

Thanks for helping. BeeLadybug is a small Manifest V3 extension: plain JavaScript files, loaded directly by Chrome.

## Report a bug

Open an issue:

https://github.com/antonioprosperi2-svg/BeeLadybug-Chrome-Extension/issues

Please include:

- The extension version (`1.0.5` in `manifest.json`, or the version shown in `chrome://extensions`).
- The page URL, and what you clicked or hovered.
- What you expected, and what the HUD or the HTML report showed instead.
- A screenshot of the panel or the exported report, if you can share the page.

Security issues belong in a private advisory. See [SECURITY.md](SECURITY.md). Please do not file those as public issues.

## Suggest a feature

Use the same issue list. Describe the page you were inspecting and the check or HUD behavior you want. A short example, or a reduced HTML file, is more useful than a general idea.

## Test a change locally

There is no build step and no package install.

1. Clone the repository, or download and unzip it.
2. Open `chrome://extensions`.
3. Turn on **Developer mode**.
4. Click **Load unpacked** and select this folder (the one that contains `manifest.json`).
5. Open a normal web page, click the BeeLadybug icon, then click **Attiva/Disattiva HUD**.

After you edit a file, click the reload button on the extension card in `chrome://extensions`. Then turn the HUD off and on again, so the page picks up the new `content.js` and `entity-bridge.js`.

`popup.html` loads `popup.js` as a module. You can change the popup and reload the extension. The page scripts Chrome actually injects are `entity-bridge.js` and `content.js` (`popup.js` names those two files). `content.src.js` is the boot code written as a module for the engine repository. Editing `content.src.js` alone does not change what the extension runs.

Protected pages cannot be inspected. Use an `https://` page, or a local file page, rather than `chrome://` or the Chrome Web Store.

## How the code is written

Keep it vanilla JavaScript.

- No frameworks, no UI libraries, and no bundler in this repository.
- No build step. Chrome loads `manifest.json`, the popup, `content.js`, and `entity-bridge.js` as they are.
- Match the style of the file you are editing. `popup.js` and `content.src.js` are modules. `entity-bridge.js` is a classic script.
- Change only what the bug or feature needs.

The engine (the HUD, the inspectors, and the report) lives in the main package:

https://github.com/antonioprosperi2-svg/BeeLadybug-universal

A change to how a check works may belong there. A change to the toolbar button, the injection, or the entity bridge belongs here.

By participating, you agree to the [code of conduct](CODE_OF_CONDUCT.md).
