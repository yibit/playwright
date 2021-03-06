---
id: emulation
title: "Device and environment emulation"
---

Playwright allows overriding various parameters of the device where the browser is running:
  - viewport size, device scale factor, touch support
  - locale, timezone
  - color scheme
  - geolocation

Most of these parameters are configured during the browser context construction, but some of them such as viewport size can be changed for individual pages.

<!-- TOC -->

<br/>

## Devices

Playwright comes with a registry of device parameters for selected mobile devices. It can be used to simulate browser behavior on a mobile device:

```js
const { chromium, devices } = require('playwright');
const browser = await chromium.launch();

const pixel2 = devices['Pixel 2'];
const context = await browser.newContext({
  ...pixel2,
});
```

All pages created in the context above will share the same device parameters.

#### API reference

- [`property: Playwright.devices`]
- [`method: Browser.newContext`]

<br/>

## User agent

All pages created in the context above will share the user agent specified:

```js
const context = await browser.newContext({
  userAgent: 'My user agent'
});
```

#### API reference

- [`method: Browser.newContext`]

<br/>

## Viewport

Create a context with custom viewport size:

```js
// Create context with given viewport
const context = await browser.newContext({
  viewport: { width: 1280, height: 1024 }
});

// Resize viewport for individual page
await page.setViewportSize({ width: 1600, height: 1200 });

// Emulate high-DPI
const context = await browser.newContext({
  viewport: { width: 2560, height: 1440 },
  deviceScaleFactor: 2,
});
```

#### API reference

- [`method: Browser.newContext`]
- [`method: Page.setViewportSize`]

<br/>

## Locale & timezone

```js
// Emulate locale and time
const context = await browser.newContext({
  locale: 'de-DE',
  timezoneId: 'Europe/Berlin',
});
```

#### API reference

- [`method: Browser.newContext`]

<br/>

## Permissions

Allow all pages in the context to show system notifications:
```js
const context = await browser.newContext({
  permissions: ['notifications'],
});
```

Grant all pages in the existing context access to current location:
```js
await context.grantPermissions(['geolocation']);
```

Grant notifications access from a specific domain:
```js
await context.grantPermissions(['notifications'], {origin: 'https://skype.com'} );
```

Revoke all permissions:
```js
await context.clearPermissions();
```

#### API reference

- [`method: Browser.newContext`]
- [`method: BrowserContext.grantPermissions`]
- [`method: BrowserContext.clearPermissions`]

<br/>

## Geolocation
Create a context with `"geolocation"` permissions granted:
```js
const context = await browser.newContext({
  geolocation: { longitude: 48.858455, latitude: 2.294474 },
  permissions: ['geolocation']
});
```
Change the location later:

```js
await context.setGeolocation({ longitude: 29.979097, latitude: 31.134256 });
```

**Note** you can only change geolocation for all pages in the context.

#### API reference

- [`method: Browser.newContext`]
- [`method: BrowserContext.setGeolocation`]

<br/>

## Color scheme and media

Create a context with dark or light mode. Pages created in this context will
follow this color scheme preference.

```js
// Create context with dark mode
const context = await browser.newContext({
  colorScheme: 'dark' // or 'light'
});

// Create page with dark mode
const page = await browser.newPage({
  colorScheme: 'dark' // or 'light'
});

// Change color scheme for the page
await page.emulateMedia({ colorScheme: 'dark' });

// Change media for page
await page.emulateMedia({ media: 'print' });
```

#### API reference

- [`method: Browser.newContext`]
- [`method: Page.emulateMedia`]
