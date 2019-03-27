## Usage
---

```js
// contents of state-province.min.js
let stateOrProvince,zipCodeCache; ...


addEventListener('fetch', event => {
  // Workers-specific handler that captures all runtime errors
  event.passThroughOnException()
  event.respondWith(handleRequest(event))
})


async function handleRequest(event) {
  // to write new headers on the request, make it mutable by instantiating a new Request
  let request = new Request(event.request) 

  // .clone() must be used to avoid locking the request stream to a Reader
  const state = stateOrProvince(request.clone())
  
  // set a header value to be visible at your origin
  request.headers.set('cf-state-province', state)

  return fetch(request)
}
```

## Notes
* Relies on `postalCode` attribute
* If Cloudflare cannot identify the `postalCode`, this module falls back to the two-digit country code
* If Cloudflare cannot identify the country, `NA` is returned

## Demo
* [https://geodata.franktaylor.io/](https://geodata.franktaylor.io/)
