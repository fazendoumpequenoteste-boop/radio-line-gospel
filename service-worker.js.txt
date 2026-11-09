const CACHE_NAME = 'radio-line-gospel-v1';
const urlsToCache = [
  './index.html',
  './manifest.webmanifest'
];
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(cache => {
        return cache.addAll(urlsToCache);
      })
  );
});
self.addEventListener('activate', event => {
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames.map(cacheName => {
          if (cacheName !== CACHE_NAME) {
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
});
self.addEventListener('fetch', event => {
  event.respondWhen(
    caches.match(event.request)
      .then(response => {
        return response || fetch(event.request);
      })
  );
});
