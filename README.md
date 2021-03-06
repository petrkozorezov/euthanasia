euthanasia is a merciful killer for your Erlang processes
=========================================================

## Basic usage

Start euthanasia application:

```erlang
application:start(euthanasia)
```

Register process in the list:

```erlang
euthanasia:register(Pid)
```

You can also call

```erlang
euthanasia:register()
```

to register the currently running process.

If the process' queue grows too large, euthanasia will kill it

If the process' total memory grows too large, euthanasia tries to garbage
collect it first, then kills it if garbage collecting doesn't help

NOTE: euthanasia doesn't restart processes, it's supervisors' job. So, make sure
you've taken care about the process' restart strategy if it needs to be
restarted.

Unregistering process and stopping its monitoring:

```erlang
euthanasia:unregister(Pid)
```

## Advanced options:

```erlang
euthanasia:register(Pid, [{Option1, Value1}, {Option2, Value2}])
```

Maximum queue length (default value is 100):

```erlang
{max_queue_length, 10}
```

Process check interval (default value is 1000 milliseconds):

```erlang
{interval, 100}
```

NOTE: use with care, too frequent process checks may cause too much load

Maximum memory before garbage collecting, in bytes (default value is
20971520, i.e. 20MB):

```erlang
{max_memory, 102400}
```

Number of forced garbage collection attempts before killing it and non-zero
interval between these attempts, in milliseconds; 0 attempts means no forced
garbage collecting for this process (default {2, 1000}):

```erlang
{gc_attempts, {Attempts, Interval}}
```

Process pre-kill hook. You can log some extended process information here,
or even save its queue for further reuse, or whatever else.
Function with arity 1 that accepts the pid as an argument (in a
{Module, Function} format or just a lambda) and a maximum timeout to perform
the hook before force-killing the process. Default value is 'undefined', i.e.
no hook at all:

```erlang
{pre_kill_hook, {{M, F}, Timeout}}
```

You can change default numbers by setting the same keys for euthanasia
application environment variables, for example:

```erlang
application:set_env(euthanasia, max_memory, 4096000)
```

### TODO:

* Add docs and specs into the code
* Add several more tests for better code coverage
