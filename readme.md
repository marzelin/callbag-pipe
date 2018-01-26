# callbag-pipe

Utility function for plugging callbags together in chain. This utility actually doesn't rely on Callbag specifics, and is basically the same as Ramda's `pipe` or lodash's `flow`. Anyway, this exists just to play nicely with the ecosystem, and to facilitate the import of the function.

`npm install callbag-pipe`

## example

Create a source with `pipe`, then pass it to an `observe`:

```js
const interval = require('callbag-interval');
const observe = require('callbag-observe');
const combine = require('callbag-combine');
const pipe = require('callbag-pipe');
const take = require('callbag-take');
const map = require('callbag-map');

const source = pipe(
  combine(interval(100), interval(350)),
  map(([x, y]) => `X${x},Y${y}`),
  take(10)
);

observe(x => console.log(x))(source); // X2,Y0
                                      // X3,Y0
                                      // X4,Y0
                                      // X5,Y0
                                      // X6,Y0
                                      // X6,Y1
                                      // X7,Y1
                                      // X8,Y1
                                      // X9,Y1
                                      // X9,Y2
```

Or use `pipe` to go all the way from source to sink:

```js
const interval = require('callbag-interval');
const observe = require('callbag-observe');
const combine = require('callbag-combine');
const pipe = require('callbag-pipe');
const take = require('callbag-take');
const map = require('callbag-map');

pipe(
  combine(interval(100), interval(350)),
  map(([x, y]) => `X${x},Y${y}`),
  take(10),
  observe(x => console.log(x))
);

// X2,Y0
// X3,Y0
// X4,Y0
// X5,Y0
// X6,Y0
// X6,Y1
// X7,Y1
// X8,Y1
// X9,Y1
// X9,Y2
```
