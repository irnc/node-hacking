# node-hacking

_Hello, and welcome to hacking node.js!_

Start hacking from an error, check out `error.js` and explore its stack trace:

- [`bootstrap_node.js`][]
  - checkout `bootstrap_node.js`, look around for code structure
  - it does not call defined function, instead that file is executed as a
    string and function is returned to `LoadEnvironment`, which calls it
    passing `process` object
- https://github.com/nodejs/node/blob/master/src/node.cc
  - `Start`, `LoadEnvironment`

[`bootstrap_node.js`]: https://github.com/nodejs/node/blob/master/lib/internal/bootstrap_node.js

Lets explore an interesting parts, which are where module boundaries are
crossed. According to stack trace, this happens when `run` function calls
`Module.runMain`, where `Module = NativeModule.require('module')`.

## Side note on `NativeModule.require`

Here _native_ means native to Node.js module system, e.g. module written in
JavaScript.

`NativeModule` is defined right here at `bootstrap_node.js`, it is a
constructor for objects representing modules as seen by Node.js. Instances of
that object is known as `module` in Node.js scripts.

`NativeModule.require` is exactly the same function which Node.js developers
call `require`. It  returns module exports, right after compiling module
using `NativeModule.prototype.compile`.

`compile` gets the source, wraps it into a function (which would receive all
the things related to Node.js module system), runs the source to get the
function and calls it.

Here lays another two points of interest, `_source` used to get the source code
and `runInThisContext`, which is used to get function instantiating exports for
each and every native module.

## V8 intro

To understand how source is received and executed, we need to understand a few
concepts from V8, namely `Isolate` and `Context`. Which are explained perfectly
in following StackOverflow answers.

http://stackoverflow.com/questions/19383724/what-exactly-is-the-difference-between-v8isolate-and-v8context

Also Node.js core and a few base concepts:

- `Environment` which could give us isolates,
- `IsolateData` which is created from `v8::Isolate` and `uv_loop_s`

`Environment::GetCurrent(isolate)` suggests that environment in Node.js is
similar to a context in V8.

`GetCurrent` also accepts:

- `FunctionCallbackInfo<Value>`
- `PropertyCallbackInfo<Array>`

CreateEnvironment

## `_source`

`_source` is defined as `NativeModule._source = process.binding('natives')`,
thus we need to understand the `process` object and its `binding` function.

Other than public API, process has non-public `binding` function which returns
internal modules, written in C++, helpful to understand how things work inside
Node.js.

http://stackoverflow.com/questions/24042861/nodejs-what-does-process-binding-mean

- bindings are usually registered using [`NODE_MODULE_CONTEXT_AWARE_BUILTIN`][]
  macros
- which is about functions, but `_source` is used as an object, so it should be
  registered differently
- lets search for `node_module_register` and `natives`
  - one hit, seems to be not relevant to what we look
- `standup` calls `_process.setupConfig(NativeModule._source);`
  - consumes `_source.config`
  - `config` contains JSON, parsed into `process.config`
  - so somewhere there is `config` module exporting JSON string
    - looks like it is created at build time
    - https://nodejs.org/api/process.html#process_process_config

[`NODE_MODULE_CONTEXT_AWARE_BUILTIN`]: https://github.com/nodejs/node/blob/ba4847e879424ad173289e8fb96cc86a09ee899b/src/node.h#L478

## `runInThisContext`

`runInThisContext` uses `contextify` binding.

https://github.com/nodejs/node/blob/524f693872cf453af2655ec47356d25d52394e3d/src/node_contextify.cc

Calls `script->Run()` to get result, which is from V8.

TK http://jcla1.com/blog/exploring-the-v8-js-engine-part-1/
