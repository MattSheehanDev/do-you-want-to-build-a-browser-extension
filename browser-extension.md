autoscale: true
slide-transition: true
theme: Fira
text: #c55749
header: #c55749

[.header: alignment(left), line-height(1)]
[.text: alignment(left), line-height(1)]
[.background-color: #c55749]

![original fit](chrome-desktop-ui.png)

-
## Do You Want to Build a Browser Extension?
#### _Matthew Sheehan_

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]

![original fit](chrome-desktop-ui.png)


# Getting Started

- Focus on Chromium-based Browsers
  - Google Chrome, Microsoft Edge
  - Firefox compatibility: `https://www.extensiontest.com`

^ Firefox is highly compatible. Most of the changes are going to be simply finding and replacing the "chrome" namespace with the "browser" namespace.

- Focus on Manifest V3 WebExtension API
  - Service Workers and Promises

^ Chrome stopped accepting Manifest V2 extensions at the beginning of 2022
^ Before service workers there were "background" pages which also ran on a separate thread. But service workers is more in line with the Service Worker API used by PWA.

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]

![original fit](chrome-desktop-ui.png)

# Anatomy of a Browser Extension

- Manifest File

^ -> Manifest File. Gives information to the browser about the extension. Defines the files and capabilities of the extension, as well as what permissions the extension is requesting.

- Service Worker (background page)

^ Contains listeners for browser events. Dormant until needed when an event fires.
^ Has access to the Chrome extension APIs the extension has declared permission to.

- Content Script

^ Runs in an isolated world - has access to the DOM elements but runs in a separate execution environment. Variables, functions, etc are not accessible to the web page and vice versa.

- Toolbar Icon

^ Provides the user easy access to the extension UI

- Popup UI Elements

^ Works very similarly to a webpage. Can include stylesheets and scripts (but not inline javascript).

- Options Page


---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

-
# Architecture

![inline fit](chrome-ext-architecture.png)

^ Background script listens for events from the browser and can also send messages back to the browser or to other extensions.

^ Content script has access to the webpage DOM and communicates to the background script and popup by sending and receiving messages.

^ Popup is loaded when the toolbar icon is clicked and loads any JS.

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

# Message passing :incoming_envelope:

- One-time messages
- Long-lived messages

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

# Example 1 - Sideloading

- Chrome Developer Mode & Sideloading
- Starter Extension Template
  - `https://github.com/MattSheehanDev/chrome-manifest-v3-starter-template`

^ chrome://extensions
^ Chrome Developer Mode -> Load unpacked

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.code: auto(42), text-scale(1)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

# Minimum Manifest File

```json
{
  // Required values
  "manifest_version": 3,
  "name": "Extension Name",
  "version": "0.0.1",

  // Recommended values
  "description": "Plain text description",
  "action": {...},
  "icons": {...},
}
```
https://developer.chrome.com/docs/extensions/mv3/manifest/

^ “name” - appears in chrome web store and browser
  “version” - start small, 0.0.1, each update in the chrome web store has to be incremented
  “description” - 132 characters or less, plain text only (no html or other markup)
  “icons” - 16/32/48/128px size images (preferably PNGs)

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.code: auto(42), text-scale(1)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

# Common APIs

- chrome.actions
  - Control the extensions toolbar icon and toolbar popup UI
- chrome.tabs :arrow_right: `permissions: ["tabs"]`
  - Needed to communicate with a tab's Content Script

^ But can also do things such as create a new tab or modify the tab.

- chrome.storage :arrow_right: `permissions: ["storage"]`
  - A more robust version of the localStorage web API

^ Storage is persistent and asynchronous, works with Chrome sync.

^ There are way to many extension APIs to cover all of them, but these are some common ones.

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

# Example 2 - Debugging

- Background Script Errors

^ When there is an error in the extension you will see an Errors button.

- Inspecting the Background Script

^ You can inspect the background script in chrome://extensions

- Content Script Logs

^ Content script logs log to the webpage console

- Inspecting the Popup UI

^ Popop UI logs log to their own console (inspect the popup html)


---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

# Example 3 - Full Extension

---

[.header: alignment(center), line-height(2), text-scale(1)]
[.text: alignment(left), line-height(1), text-scale(0.75)]
[.background-color: #c55749]
[.autoscale: false]

![original fit](chrome-desktop-ui.png)

# Publishing your Extension

- Create a Chrome Web Store Developer Account
  - https://chrome.google.com/webstore/devconsole/register

- Zip and upload your extension

- Submit your extension
  - Justify any permissions your extension uses
  - Loading and executing remote code, as of Manifest V3, is not accepted
  - Make sure your extension has a single and narrow purpose.

^ Requesting permissions that your app does not use will result in your submission getting rejected.

^ Creating an extension that does too many things will result in your submission getting rejected.

^ There is another way to distribute your extension and that's going to `chrome://extensions` -> Pack extension. That wil create a CRX package that can be distributed on Linux.

