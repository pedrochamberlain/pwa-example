# Building a Progressive Web Application from Scratch 
<p align="center">
  <img width="500px" src="https://user-images.githubusercontent.com/3104648/28351989-7f68389e-6c4b-11e7-9bf2-e9fcd4977e7a.png">
  <br>
  <a href="https://www.youtube.com/watch?v=sFsRylCQblw">Based on a Fireship tutorial</a>
  
## What is a PWA?
- PWAs are browser based web applications (HTML5 / CSS / JavaScript). By using the latest browser features, they implement native app-typical capabilities and features, including all listed above. Running in a separate browser window without address bar, they are visually and functionally near-indistinguishable to apps. 
- "Progressive" means: A PWA must be designed to maintain basic functionality even if run offline and/or on a browser that does not support some features. 
- PWAs don’t install in the classical sense. An icon is added to the screen and advanced browser caching is used. The browser provides a secure execution environment to which PWAs are ‘installed’.
- Service workers are a huge aspect of PWAs. They cache pages in order to view them offline. [Learn more on Service Workers & Caching here.](https://www.youtube.com/watch?v=ksXwaWHCW6k)

## How to build a PWA
1. Create the following files: `index.html`, `manifest.json`, `service-worker.js`. Download a logo for your PWA. [You can use mine.](https://github.com/pedrochamberlain/pwa-example/blob/main/logo.png)
2. Reference `manifest.json` on `index.html`:
```html
<link rel="manifest" href="manifest.json">
```
3. Load the service worker on `index.html`:
```html
<body>
  [...]
  <script> 
      if ('serviceWorker' in navigator) {
          navigator.serviceWorker.register('/service-worker.js')
      }
  </script>
</body>
```
4. Add some self-explanatory info to the app in your `manifest.json`. Here's an example:
```json
{
    "name": "PWA Example",
    "short_name": "PWA Example",
    "start_url": "/?home=true",
    "icons": [],
    "theme_color": "#000000",
    "background_color": "#FFFFFF",
    "display": "fullscreen",
    "orientation": "portrait"
}
```
You may question why `icons` has no values. Creating assets for a PWA can be quite a hassle because we have to resize the same icon for different devices. To facilitate our process, we'll use the [`pwa-asset-generator`](https://github.com/onderceylan/pwa-asset-generator) library. 

Install the library and run the following command in your project's folder:
```zsh
npx pwa-asset-generator logo.png icons
```
5. In `service-worker.js`, use Workbox, Google's PWA library, via CDN:
```javascript
importScripts('https://storage.googleapis.com/workbox-cdn/releases/6.0.2/workbox-sw.js');

workbox.routing.registerRoute(
  ({request}) => request.destination === 'image',
  new workbox.strategies.CacheFirst()
);
```

Your PWA is now ready to use!

Run `npx serve` in your command line to test it up.

<div align="center">
  <img width="450px" src="https://imgur.com/gHpJ7iI.jpg">
</div>


## Browser Support
Browser|Windows|macOS|Android|iOS|
-------|-------|-----|-------|---|
Chrome|Yes|Yes|Yes|Yes|
Firefox|No|No|Partial|No|
Safari|N/A|Yes|N/A|iOS 11.3+|

*Last updated February 9th, 2021.*
 
## Documentation
- [Workbox – JavaScript library for PWAs](https://developers.google.com/web/tools/workbox/modules/workbox-sw)
- [PWA Asset Generator - Automates asset generation and image declaration](https://github.com/onderceylan/pwa-asset-generator)
- [PWA Tutorial – Fireship](https://www.youtube.com/watch?v=sFsRylCQblw)
- [Intro To Service Workers & Caching - Traversy Media](https://www.youtube.com/watch?v=ksXwaWHCW6k)
