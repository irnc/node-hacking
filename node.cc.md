## Single instances

- `v8_platform`

## `Start`

- `int Start(int argc, char** argv)`
  - calls
    - `PlatformInit()`
    - `Init(&argc, const_cast<const char**>(argv), &exec_argc, &exec_argv)`
    - `Start(uv_default_loop(), argc, argv, exec_argc, exec_argv)`
      - creates `Isolate` by calling `Isolate::New`
      - calls
        - `Start(isolate, &isolate_data, argc, argv, exec_argc, exec_argv)`
          - creates `Context` by calling `Context::New(isolate)`
          - creates `Environment` from `isolate_data` and `context`
          - calls
            - `LoadEnvironment(&env)`
          - loops
            - `v8_platform.PumpMessageLoop(isolate);`
            - `more = uv_run(env.event_loop(), UV_RUN_ONCE);`

## `Init`

- calls
  - `ParseArgs(argc, argv, exec_argc, exec_argv, &v8_argc, &v8_argv)`
