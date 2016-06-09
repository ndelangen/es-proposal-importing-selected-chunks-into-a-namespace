# Proposal for Ecma Script: importing selected chucks of a module into an object / namespace


## intro
Modules are always imported into the root scope of the imported file. But in some cases it can be desirable to load modules into a namespace-object. Especially when importing a large number of parts/chunks from a module. Creating a separate variable for each becomes unwieldy.

There already is a syntax to load all exports of a module as/into into a namespace-object:

```js
import * as myLib from 'myLib';

// usage
myLib.chunkA();
```

Another very useful syntax is importing chunks from a module:
```js
import { chunkA, chunkB } from 'myLib';

// usage
chunkA();
```

However, it’s currently impossible to combine / compose these features: load certain chunks AND bundle the chucks into an object at the same time.

Currently, to achieve this an editor needs to do this:

```js
import { chunkA, chunkB } from 'myLib';
const myLib = { chunkA, chunkB };

// usage
myLib.chunkA();
```

This method has a few problems:
- This still adds variables, which may conflict.
- Duplicates code

---
## Proposal

I propose this syntax would become allowed:

```js
import { chunkA, chunkB } as myLib from 'myLib';

// usage
myLib.chunkA();
```

This can become a useful tool when using multiple libraries with similar utility or namingschemes, for example:

```js
import { parse } from 'xml-lib';
import { parse } from 'json-lib';
import { parse } from 'html-lib';

// will throw
```

One of the ways this can currently be solved is like this:

```js
import { parse as xmlParse } from 'xml-lib';
import { parse as jsonParse } from 'json-lib';
import { parse as htmlParse } from 'html-lib';

// usage
xmlParse();
jsonParse();
htmlParse();
```

Namespacing variables like this can be a valid strategy, but it can become cumbersome:

```js
import { parse as xmlParse, create as xmlCreate, sandwich as xmlSandwich } from 'xml-lib';
import { parse as jsonParse, create as jsonCreate, bacon as jsonBacon } from 'json-lib';
import { parse as htmlParse, create as htmlCreate, orange as htmlOrange } from 'html-lib';
```

It would be nice to have this option:

```js
import { parse } as xmlLib from 'xml-lib';
import { parse } as jsonLib from 'json-lib';
import { parse } as htmlLib from 'html-lib';

// usage
xmlLib.parse();
jsonLib.parse();
htmlLib.parse();
```

## Why

Declaring **exactly** what you depend on is great; you loose some of this when using the `import * as foo from 'bar';` syntax. 
It saves a lot of renaming / namespacing individual chunks.
Small modules with a single export may be the preferred default but there are usecases for modules with many exports.

---

These would be some egde cases - but they could work:
```js
import { parse as read } as xmlLib from 'xml-lib';
```

```js
import xmlLib, { parse, read } as xmlReader from 'xml-lib';
```

```js
import xmlLib, { parse, read } as xmlReader from 'xml-lib';
```
