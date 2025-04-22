Reproduction cli log:

```
yarn create rspack
yarn create v1.22.22
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "create-rspack@1.3.5" with binaries:
- create-rspack
[#####] 5/5
◆  Create Rspack Project
│
◇  Project name or path
│  pnp-repro
│
◇  Select framework
│  React
│
◇  Select language
│  TypeScript
│
◇  Select additional tools (Use <space> to select, <enter> to continue)
│  none
│
◇  Next steps ─────────────╮
│                          │
│  1. cd pnp-repro         │
│  2. git init (optional)  │
│  3. yarn install         │
│  4. yarn run dev         │
│                          │
├──────────────────────────╯
│
└  All set, happy coding!

Done in 34.92s.


 yarn 
yarn install v1.22.22
info No lockfile found.
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
warning " > ts-node@10.9.2" has unmet peer dependency "@types/node@*".
[4/4] Building fresh packages...

success Saved lockfile.
Done in 10.26s.
PS > cd pnp-repro
PS > yarn dev 
yarn run v1.22.22
$ rspack dev
rspack.config.ts:54:15 - error TS2351: This expression is not constructable.
  Type 'typeof import("<PWD>/pnp-repro/node_modules/@rspack/plugin-react-refresh/dist/index")' has no construct signatures.

54   isDev ? new RefreshPlugin() : null
                 ~~~~~~~~~~~~~

error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.

```

Diff(`./rspack.config.ts`):
```diff
- import * as RefreshPlugin from "@rspack/plugin-react-refresh";
+ import RefreshPlugin from "@rspack/plugin-react-refresh";
```

```
PS > yarn dev
> pnp-repro@1.0.0 dev
> rspack dev

<i> [webpack-dev-server] Project is running at:
<i> [webpack-dev-server] Loopback: http://localhost:8080/, http://[::1]:8080/
<i> [webpack-dev-server] On Your Network (IPv4): http://192.168.3.41:8080/
<i> [webpack-dev-server] Content not from webpack is served from '<PWD>\pnp-repro\public' directory
●  ━━━━━━━━━━━━━━━━━━━━━━━━━ (100%) emitting after emit
Rspack compiled successfully in 128 ms
<i> [webpack-dev-server] Gracefully shutting down. To force exit, press ^C again. Please wait...
```

It's (still) working!

Move to pnp (diff: https://github.com/reller-sh/pnp-repro/commit/221c34a7c8833649898f071811b5669cc3f84ff5):
```
PS > yarn set version 3.8.7
➤ YN0000: You don't seem to have Corepack enabled; we'll have to rely on yarnPath instead
➤ YN0000: Downloading https://repo.yarnpkg.com/3.8.7/packages/yarnpkg-cli/bin/yarn.js
➤ YN0000: Saving the new release in .yarn/releases/yarn-3.8.7.cjs
➤ YN0000: Done with warnings in 0s 515ms

PS > yarn                  
➤ YN0070: Migrating from Yarn 1; automatically enabling the compatibility node-modules linker 👍

➤ YN0000: ┌ Resolution step
➤ YN0032: │ fsevents@npm:2.3.3: Implicit dependencies on node-gyp are discouraged
➤ YN0002: │ pnp-repro@workspace:. doesn't provide @types/node (paad02), requested by ts-node
➤ YN0000: │ Some peer dependencies are incorrectly met; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code
➤ YN0000: └ Completed in 3s 133ms
➤ YN0000: ┌ Fetch step
➤ YN0013: │ yallist@npm:4.0.0 can't be found in the cache and will be fetched from the remote registry
➤ YN0013: │ yallist@npm:5.0.0 can't be found in the cache and will be fetched from the remote registry
➤ YN0013: │ yargs-parser@npm:21.1.1 can't be found in the cache and will be fetched from the remote registry
➤ YN0013: │ yargs@npm:17.7.2 can't be found in the cache and will be fetched from the remote registry
➤ YN0013: │ yn@npm:3.1.1 can't be found in the cache and will be fetched from the remote registry
➤ YN0000: └ Completed in 0s 364ms
➤ YN0000: ┌ Link step
➤ YN0000: └ Completed in 2s 419ms
➤ YN0000: Done with warnings in 5s 939ms


PS > yarn 
➤ YN0000: ┌ Resolution step
➤ YN0002: │ pnp-repro@workspace:. doesn't provide @types/node (paad02), requested by ts-node
➤ YN0000: │ Some peer dependencies are incorrectly met; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code
➤ YN0000: └ Completed
➤ YN0000: ┌ Fetch step
➤ YN0000: └ Completed
➤ YN0000: ┌ Link step
➤ YN0000: │ ESM support for PnP uses the experimental loader API and is therefore experimental
➤ YN0000: └ Completed
➤ YN0000: Done with warnings in 0s 133ms

PS > yarn dev               
<i> [webpack-dev-server] Project is running at:
<i> [webpack-dev-server] Loopback: http://localhost:8080/, http://[::1]:8080/
<i> [webpack-dev-server] On Your Network (IPv4): http://192.168.3.41:8080/
<i> [webpack-dev-server] Content not from webpack is served from '<PWD>\pnp-repro\public' directory
●  ━━━━━━━━━━━━━━━━━━━━━━━━━ (100%) emitting after emit                                                                                                                                                                                                                                                               
ERROR in ./.yarn/__virtual__/@rspack-dev-server-virtual-23a8342761/0/cache/@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip/node_modules/@rspack/dev-server/client/index.js?protocol=ws%3A&hostname=0.0.0.0&port=8080&pathname=%2Fws&logging=info&overlay=%7B%22errors%22%3Atrue%2C%22warnings%22%3Afalse%7D&reconnect=10&hot=true&live-reload=true 15:0-84
  × Module not found: Can't resolve 'webpack-dev-server/client/overlay.js' in '<PWD>\pnp-repro\.yarn\__virtual__\@rspack-dev-server-virtual-23a8342761\0\cache\@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip\node_modules\@rspack\dev-server\client'
    ╭─[15:45]
 13 │ /* global __resourceQuery, __webpack_hash__ */ /* Rspack dev server runtime client */ /// <reference types="webpack/module" />
 14 │ import webpackHotLog from "@rspack/core/hot/log.js";
 15 │ import { createOverlay, formatProblem } from "webpack-dev-server/client/overlay.js";
    ·                                              ──────────────────────────────────────
 16 │ import socket from "webpack-dev-server/client/socket.js";
 17 │ import { defineProgressElement, isProgressSupported } from "webpack-dev-server/client/progress.js";
    ╰────


ERROR in ./.yarn/__virtual__/@rspack-dev-server-virtual-23a8342761/0/cache/@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip/node_modules/@rspack/dev-server/client/index.js?protocol=ws%3A&hostname=0.0.0.0&port=8080&pathname=%2Fws&logging=info&overlay=%7B%22errors%22%3Atrue%2C%22warnings%22%3Afalse%7D&reconnect=10&hot=true&live-reload=true 16:0-57
  × Module not found: Can't resolve 'webpack-dev-server/client/socket.js' in '<PWD>\pnp-repro\.yarn\__virtual__\@rspack-dev-server-virtual-23a8342761\0\cache\@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip\node_modules\@rspack\dev-server\client'
    ╭─[16:19]
 14 │ import webpackHotLog from "@rspack/core/hot/log.js";
 15 │ import { createOverlay, formatProblem } from "webpack-dev-server/client/overlay.js";
 16 │ import socket from "webpack-dev-server/client/socket.js";
    ·                    ─────────────────────────────────────
 17 │ import { defineProgressElement, isProgressSupported } from "webpack-dev-server/client/progress.js";
 18 │ import { log, setLogLevel } from "webpack-dev-server/client/utils/log.js";
    ╰────


ERROR in ./.yarn/__virtual__/@rspack-dev-server-virtual-23a8342761/0/cache/@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip/node_modules/@rspack/dev-server/client/index.js?protocol=ws%3A&hostname=0.0.0.0&port=8080&pathname=%2Fws&logging=info&overlay=%7B%22errors%22%3Atrue%2C%22warnings%22%3Afalse%7D&reconnect=10&hot=true&live-reload=true 17:0-99
  × Module not found: Can't resolve 'webpack-dev-server/client/progress.js' in '<PWD>\pnp-repro\.yarn\__virtual__\@rspack-dev-server-virtual-23a8342761\0\cache\@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip\node_modules\@rspack\dev-server\client'
    ╭─[17:59]
 15 │ import { createOverlay, formatProblem } from "webpack-dev-server/client/overlay.js";
 16 │ import socket from "webpack-dev-server/client/socket.js";
 17 │ import { defineProgressElement, isProgressSupported } from "webpack-dev-server/client/progress.js";
    ·                                                            ───────────────────────────────────────
 18 │ import { log, setLogLevel } from "webpack-dev-server/client/utils/log.js";
 19 │ import sendMessage from "webpack-dev-server/client/utils/sendMessage.js";
    ╰────


ERROR in ./.yarn/__virtual__/@rspack-dev-server-virtual-23a8342761/0/cache/@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip/node_modules/@rspack/dev-server/client/index.js?protocol=ws%3A&hostname=0.0.0.0&port=8080&pathname=%2Fws&logging=info&overlay=%7B%22errors%22%3Atrue%2C%22warnings%22%3Afalse%7D&reconnect=10&hot=true&live-reload=true 18:0-74
  × Module not found: Can't resolve 'webpack-dev-server/client/utils/log.js' in '<PWD>\pnp-repro\.yarn\__virtual__\@rspack-dev-server-virtual-23a8342761\0\cache\@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip\node_modules\@rspack\dev-server\client'
    ╭─[18:33]
 16 │ import socket from "webpack-dev-server/client/socket.js";
 17 │ import { defineProgressElement, isProgressSupported } from "webpack-dev-server/client/progress.js";
 18 │ import { log, setLogLevel } from "webpack-dev-server/client/utils/log.js";
    ·                                  ────────────────────────────────────────
 19 │ import sendMessage from "webpack-dev-server/client/utils/sendMessage.js";
 20 │ /**
    ╰────


ERROR in ./.yarn/__virtual__/@rspack-dev-server-virtual-23a8342761/0/cache/@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip/node_modules/@rspack/dev-server/client/index.js?protocol=ws%3A&hostname=0.0.0.0&port=8080&pathname=%2Fws&logging=info&overlay=%7B%22errors%22%3Atrue%2C%22warnings%22%3Afalse%7D&reconnect=10&hot=true&live-reload=true 19:0-73
  × Module not found: Can't resolve 'webpack-dev-server/client/utils/sendMessage.js' in '<PWD>\pnp-repro\.yarn\__virtual__\@rspack-dev-server-virtual-23a8342761\0\cache\@rspack-dev-server-npm-1.1.1-32a5f51c63-f251df03dd.zip\node_modules\@rspack\dev-server\client'
    ╭─[19:24]
 17 │ import { defineProgressElement, isProgressSupported } from "webpack-dev-server/client/progress.js";
 18 │ import { log, setLogLevel } from "webpack-dev-server/client/utils/log.js";
 19 │ import sendMessage from "webpack-dev-server/client/utils/sendMessage.js";
    ·                         ────────────────────────────────────────────────
 20 │ /**
 21 │  * @typedef {Object} OverlayOptions
    ╰────


Rspack compiled with 5 errors in 139 ms

```


