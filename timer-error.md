# Sources

- https://github.com/nodejs/node/blob/v7.9.0/lib/timers.js
- https://github.com/nodejs/node/blob/master/lib/timers.js

# `TimerWrap`

- binding
- https://github.com/nodejs/node/blob/v7.9.0/src/timer_wrap.cc

Registering associated function:

- `uv_timer_start(&wrap->handle_, OnTimeout, timeout, 0)`
  - https://github.com/nodejs/node/blob/v7.9.0/src/timer_wrap.cc#L77
- `wrap->MakeCallback(kOnTimeout, 0, nullptr)`
  - https://github.com/nodejs/node/blob/v7.9.0/src/timer_wrap.cc#L95

Looking for `MakeCallback`:

- https://github.com/nodejs/node/blob/v7.9.0/src/handle_wrap.h
- https://github.com/nodejs/node/blob/v7.9.0/src/async-wrap.h
- implementation of the `MakeCallback`, which actually calls back
  - https://github.com/nodejs/node/blob/v7.9.0/src/async-wrap.cc#L295

# `L` (`internal/linkedlist`)

# `libuv`

_TimerWrap's backing libuv timers implementation (a performant heap-based queue)_
