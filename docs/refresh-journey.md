# `refreshJourney(refreshToken, [opt])`

`refreshToken` must be a string, taken from `journey.refreshToken`.

With `opt`, you can override the default options, which look like this:

```js
{
	stopovers: false, // return stations on the way?
	polylines: false, // return a shape for each leg?
	tickets: false, // return tickets? only available with some profiles
	remarks: true, // parse & expose hints & warnings?
	language: 'en' // language to get results in
}
```

## Response

As an example, we're going to use the [VBB profile](../p/vbb):

```js
const createClient = require('hafas-client')
const vbbProfile = require('hafas-client/p/vbb')

const client = createClient(vbbProfile)

// Hauptbahnhof to Heinrich-Heine-Str.
client.journeys('900000003201', '900000100008', {results: 1})
.then(([journey]) => {
	// later, fetch up-to-date info on the journey
	client.refreshJourney(journey.refreshToken, {stopovers: true, remarks: true})
	.then(console.log)
	.catch(console.error)
})
.catch(console.error)
```

`refreshJourney()` will return a *single* [*Friendly Public Transport Format* `1.1.1`](https://github.com/public-transport/friendly-public-transport-format/tree/1.1.1) `journey`, in the same format as with `journeys()`.
