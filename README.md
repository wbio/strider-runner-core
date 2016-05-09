strider-runner-core
===================

### Changes in this fork
The current version of [strider-docker-runner](https://github.com/Strider-CD/strider-docker-runner) does not run `npm install` during the "prepare" step, so dependencies are never installed for node projects (see [issue #18](https://github.com/Strider-CD/strider-docker-runner/issues/18) on that project). strider-docker-runner uses this module (strider-runner-core) to do the actual command running, so I've forked and modified this to add a (very hacky) fix for that issue.

In this fork, `npm install` is called if another npm command (most likely `npm test`) is called and `npm install` has not yet been run. This means that `npm install` is called during the "test" phase rather than the "prepare" phase, which obviously isn't ideal from an organizational perspective, but it was the simplest fix that I found for the issue. In the future I may see if I can clean this up.

### Rest of the Readme
Just run those jobs. Decoupled from load balancing, job queues, etc.

```js
var core = require('strider-runner-core');

core.process(data, provider, plugins, config, next);
```

- `data` is the mongoose job object. See the main strider repo for a schema.
- `provider` is an instantiated provider, such as [strider-git](https://github.com/Strider-CD/strider-git).
- `plugins` is a map of instantiated plugins (such as [strider-node](https://github.com/Strider-CD/strider-node)) `{id: plugin, ...}`

Config parameters:

- env - a map for augmenting the ENV variables in all commands run
- io - an eventemitter for communication.
- dataDir - the directory to hold your code
- baseDir - base directory for this job
- cacheDir - cache directory
- cachier (see [this file](https://github.com/Strider-CD/strider-simple-runner/blob/master/lib/cachier.js))
- logger
- log - log fn
- error - log errors

`next` is called with any errors as the first argument.

