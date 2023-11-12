Falsy values: `"", 0, -0, NaN, null, undefined, false`

## macrotasks vs microtasks
- macrotasks - timeouts
- microtasks - promises
- after 1 macrotask all microtasks are executed, but there is some limit (1000 ticks)
