# Cross-device test design · News site

**What this is:** a personal practice exercise, not client work. I designed it for a news site like the publisher sites I tested at RebelMouse, to put on paper how I approach responsive and mobile testing. The cases are designed, not executed.

The focus is where news sites actually break across devices: layout and breakpoints, touch, browser and OS quirks (mostly Safari on iOS), and slow networks.

## 1. Device matrix

The first decision is **where** to test. Not everything runs everywhere: devices are split into tiers based on the site's real traffic (Google Analytics or similar).

| Tier | Device | OS / Browser | Viewport | Why |
|---|---|---|---|---|
| **1 (always)** | iPhone 15 / 13 | iOS 17-18 · Safari | 390 x 844 | Most mobile traffic on US news sites; Safari has its own quirks |
| **1** | Samsung Galaxy S23 / A54 | Android 14 · Chrome | 360 x 780 | Most common Android; narrow viewport |
| **1** | Laptop | Windows 11 · Chrome | 1366 x 768 | Most used desktop resolution |
| **1** | MacBook | macOS · Safari | 1440 x 900 | Desktop Safari differs from Chrome |
| **2 (every big release)** | iPad 10th gen | iPadOS · Safari | 820 x 1180 | Tablet: the in-between breakpoint is where things break most |
| **2** | iPhone SE | iOS · Safari | 375 x 667 | Small screen: cut-off text, hidden buttons |
| **2** | Wide desktop | Windows · Edge / Firefox | 1920 x 1080 | Large-screen layout, other engines |
| **3 (occasional)** | Low-end Android | Android 11-12 · Chrome / Samsung Internet | 360 x 640 | Performance with little RAM, Samsung's browser |
| **3** | Ultrawide monitor | Chrome | 2560 x 1440 | Content shouldn't stretch too far |

**Breakpoints to cover:** 320, 375, 390, 768, 1024, 1280, 1440+. The edges (767 vs 768, 1023 vs 1024) are where bugs show up.

**Tools:** real devices for tier 1, BrowserStack or LambdaTest for the rest, Chrome DevTools (device mode and network throttling) for a quick first pass.

## 2. Priority

- **High:** breaks reading the article, navigation or revenue (ads, subscriptions) on tier 1 devices.
- **Medium:** hurts the experience but there's a way around it; or a high issue on tier 2.
- **Low:** visual detail, or tier 3 devices.

## 3. Test cases

31 cases in 4 groups. Type: Positive / Negative / Edge.

### Layout and responsive

| ID | Area | Description | Steps | Expected result | Type | Priority |
|---|---|---|---|---|---|---|
| CD-01 | Home | The article grid reflows with the width | 1. Open the home page on desktop at 1440. 2. Shrink to 1024, 768, 390, 320. | Desktop 3-4 columns, tablet 2, mobile 1. No overlaps or gaps. | Positive | High |
| CD-02 | Global | No horizontal scroll on any viewport | 1. Go through home, article, gallery and section pages at 320 and 390. 2. Try to swipe sideways. | The page doesn't move sideways. No element sticks out past the width. | Negative | High |
| CD-03 | Article | A long headline doesn't break the layout | 1. Open an article with a 150+ character headline and one very long word with no spaces (e.g. a URL). 2. View at 320 and 390. | The headline wraps, the long word breaks or fits, nothing leaves the screen. | Edge | Medium |
| CD-04 | Article | Images in the body adapt | 1. Open an article with a horizontal image, a vertical one and an Instagram/X embed. 2. View on mobile, tablet, desktop. | Images don't distort or overflow their container. Embeds fit the width. | Positive | High |
| CD-05 | Article | The right image size is loaded (srcset) | 1. Open an article on mobile with DevTools, Network tab. 2. Check image size and weight. 3. Repeat on desktop. | Mobile downloads small versions; desktop, large ones. No 2000px images on a phone. | Positive | Medium |
| CD-06 | Global | Screen rotation | 1. Open an article on iPhone in portrait, scroll halfway. 2. Rotate to landscape. 3. Back to portrait. | The layout reflows on each turn. The reading position is kept. | Positive | Medium |
| CD-07 | Global | Exact tablet breakpoint | 1. In DevTools, set the viewport to 767px, then 768px. 2. Repeat with 1023 and 1024. | Each edge shows a coherent layout (mobile or tablet, not a mix). Menu and grid switch together. | Edge | High |
| CD-08 | Global | Browser zoom and larger text | 1. On desktop, zoom to 200%. 2. On iOS, turn on Larger Text in Accessibility. 3. Open an article. | Text grows without overlapping or getting cut. The page stays usable. | Edge | Medium |
| CD-09 | Gallery | Photo gallery at different sizes | 1. Open a gallery on mobile, tablet and desktop. 2. Go through several photos. | Photos and captions are fully visible. Controls don't cover the image. | Positive | Medium |

### Touch

| ID | Area | Description | Steps | Expected result | Type | Priority |
|---|---|---|---|---|---|---|
| CD-10 | Navigation | Hamburger menu opens and closes | 1. On mobile, tap the menu icon. 2. Go to a section. 3. Go back and close the menu by tapping the X and by tapping outside. | The menu opens, the page behind doesn't scroll while it's open, it closes both ways. | Positive | High |
| CD-11 | Global | Tap target size | 1. On mobile, tap share icons, tag links, pagination buttons. | Each element can be tapped without missing (at least 44x44 px). The one next to it isn't triggered. | Negative | High |
| CD-12 | Navigation | Menus that depend on hover | 1. On desktop, hover over a menu with a submenu. 2. On tablet and mobile, try to open the same submenu by tapping. | On touch, the first tap opens the submenu (doesn't navigate straight away) and an option can be chosen. | Negative | High |
| CD-13 | Gallery | Swipe between photos | 1. On mobile, swipe left and right in a gallery. 2. Swipe fast several times. | It moves in the right direction, without skipping photos or freezing. | Positive | Medium |
| CD-14 | Gallery | Pinch to zoom | 1. On mobile, pinch to zoom on a gallery photo and an article photo. | Zoom works to see detail. It doesn't break the layout when zooming back out. | Edge | Low |
| CD-15 | Forms | Virtual keyboard on newsletter and search | 1. On iOS and Android, tap the newsletter email field. 2. Type and submit. 3. Repeat with search. | The keyboard doesn't cover the field or the button. The email field shows the keyboard with @. Safari doesn't zoom in on focus (16px or larger font in the field). | Positive | High |
| CD-16 | Ads | Sticky ads don't block content | 1. On mobile, scroll an article with a fixed ad at the bottom. 2. Try to tap the last paragraph and links near the ad. | The ad doesn't cover text or important buttons. It can be closed if it has an X. | Negative | High |
| CD-17 | Share | Native share button | 1. On mobile, tap "Share". 2. On desktop, click the same button. | Mobile opens the system share sheet (WhatsApp, etc.). Desktop shows the site's own options or copies the link. | Positive | Medium |

### Browsers and operating systems

| ID | Area | Description | Steps | Expected result | Type | Priority |
|---|---|---|---|---|---|---|
| CD-18 | Global | Safari iOS address bar | 1. In Safari on iPhone, open an article with a fixed ad or nav bar at the bottom. 2. Scroll down and up (Safari's bar shows and hides). | Fixed elements aren't covered by Safari's bar and don't jump around. | Edge | High |
| CD-19 | Global | Notch safe area | 1. On an iPhone with a notch, open the site in landscape. | No text or button sits under the notch or the home indicator. | Edge | Medium |
| CD-20 | Video | Video playback on iOS and Android | 1. Tap play on an article video in Safari iOS. 2. Repeat in Chrome Android. 3. Enter and exit fullscreen. | The video plays inline (no forced fullscreen unless asked). Fullscreen enters and exits cleanly. | Positive | High |
| CD-21 | Video | Autoplay on mobile | 1. Open an article with an autoplay video in Safari iOS and Chrome Android. | If it autoplays, it starts muted. It never starts with sound before the user taps. | Negative | Medium |
| CD-22 | Global | Rendering across engines | 1. Open home and an article in Chrome, Safari, Firefox and Edge on desktop. 2. Compare fonts, spacing, borders and shadows. | Same design in all four. Minor rendering differences are fine; nothing broken. | Positive | Medium |
| CD-23 | Global | System dark mode | 1. Turn on dark mode on iOS and Android. 2. Open the site. | If the site supports dark mode, it applies correctly. If not, it looks the same as light mode (no black text on a black background). | Edge | Low |
| CD-24 | Global | Samsung Internet | 1. Open home and an article in Samsung Internet. | Looks and works the same as Chrome on Android. | Positive | Low |
| CD-25 | Global | Going back keeps the scroll position | 1. On mobile, scroll the home page halfway. 2. Open an article. 3. Go back (gesture on iOS, button on Android). | Returns to the same point on the home page, not the top. | Positive | Medium |

### Network and performance

| ID | Area | Description | Steps | Expected result | Type | Priority |
|---|---|---|---|---|---|---|
| CD-26 | Article | Loading on a slow network | 1. In DevTools, simulate "Slow 3G" on a mobile viewport. 2. Open an article. | The article text appears first and can be read before images and ads finish loading. | Positive | High |
| CD-27 | Global | Content doesn't jump while loading (CLS) | 1. On mobile with a slow network, open an article and start reading. 2. Watch as images and ads load. | The text doesn't move. Images and ads have their space reserved. | Negative | High |
| CD-28 | Home | Lazy loading of images | 1. Open the home page on mobile with the Network tab. 2. Scroll slowly. | Images further down only load when you get close. They aren't all downloaded upfront. | Positive | Medium |
| CD-29 | Global | Losing the connection | 1. Open an article on mobile. 2. Turn on airplane mode. 3. Tap a link to another article. 4. Reconnect. | A clear error is shown (browser's or site's). After reconnecting and reloading, the article loads normally. | Negative | Low |
| CD-30 | Global | Mobile performance metrics | 1. Run Lighthouse in mobile mode on home and an article. | LCP under 2.5 s, CLS under 0.1, INP under 200 ms (Core Web Vitals thresholds). Record the values. | Positive | Medium |
| CD-31 | Video | Video on a slow network | 1. With "Slow 3G", play a video on mobile. | Quality drops automatically instead of stalling. A loading indicator is shown. | Edge | Medium |
