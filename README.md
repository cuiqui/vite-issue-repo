# vite-issue-repo

## Steps to reproduce
1. Clone.
2. `npm install`.
3. `npm run build`.
4. Serve `index.html` (I'm using nginx).

## Expected behaviour
You can navigate smoothly and SVGs will render.

## Actual behaviour
- You can navigate to `/index` without problems, but if you go directly (through URL) to `/one` or `/two` it will fail.
- You can navigate to `/index` then to `/one` and then to `/two` (through clicking links) without problems, but not from `/index` to `/two` directly.

## Error
```
FaIcon.svelte:8 Uncaught (in promise) TypeError: Cannot read property 'icon' of undefined
    at Object.$$self.$$.update (FaIcon.svelte:8)
    at init$1 (index.mjs:1671)
    at new FaIcon (FaIcon.svelte:12)
    at create_fragment (two.e78d4eea.js:8)
    at init$1 (index.mjs:1675)
    at new Two (two.svelte:8)
    at Array.create_default_slot (Route.svelte:114)
    at create_slot (index.mjs:61)
    at create_fragment$3 (index.js:174)
    at init$1 (index.mjs:1675)
```

## Weird bundling?
Taking a look at the `dist/assets` folder I can see that `faPlusCircle` and `faMinusCircle` are pseudo-empty chunks:

```
$ cat faPlusCircle.ef7d87a6.js faMinusCircle.3d152943.js
var faPlusCircle = {};
export { faPlusCircle as f };
//# sourceMappingURL=faPlusCircle.ef7d87a6.js.map
var faMinusCircle = {};
export { faMinusCircle as f };
//# sourceMappingURL=faMinusCircle.3d152943.js.map
```

And that some of the bundled pages (for example in `/index`, where the icon works) have the import concatenated (and also imported):

```
import { f as faMinusCircle } from "./faMinusCircle.3d152943.js";
import { F as FaIcon } from "./FaIcon.2537ee2b.js";
(function(exports) {
  Object.defineProperty(exports, "__esModule", { value: true });
  var prefix = "fas";
  var iconName = "minus-circle";
  var width = 512;
  var height = 512;
  var ligatures = [];
  var unicode = "f056";
  var svgPathData = "M256 8C119 8 8 119 8 256s111 248 248 248 248-111 248-248S393 8 256 8zM124 296c-6.6 0-12-5.4-12-12v-56c0-6.6 5.4-12 12-12h264c6.6 0 12 5.4 12 12v56c0 6.6-5.4 12-12 12H124z";
  exports.definition = {
    prefix,
    iconName,
    icon: [
      width,
      height,
      ligatures,
      unicode,
      svgPathData
    ]
  };
  exports.faMinusCircle = exports.definition;
  exports.prefix = prefix;
  exports.iconName = iconName;
  exports.width = width;
  exports.height = height;
  exports.ligatures = ligatures;
  exports.unicode = unicode;
  exports.svgPathData = svgPathData;
})(faMinusCircle);
```
