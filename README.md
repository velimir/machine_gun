[![Hex.pm version](https://img.shields.io/hexpm/v/machine_gun.svg)](https://hex.pm/packages/machine_gun)
[![Hex.pm downloads](https://img.shields.io/hexpm/dt/machine_gun.svg)](https://hex.pm/packages/machine_gun)

# MachineGun

HTTP client for Elixir. Based on [Gun](https://github.com/ninenines/gun) and [Poolboy](https://github.com/devinus/poolboy).

* Supports HTTP/1 and HTTP/2 with automatic detection;
* Maintains separate connection pool for each pool_group:host:port combination;
* Allows per-pool configuration;
* Drop-in replacement for [HTTPoison](https://github.com/edgurgel/httpoison).

## Example

```
%MachineGun.Response{body: body, status_code: 200} =
  MachineGun.get!(
    "http://icanhazip.com",
    [{"accept", "text/plain"}],
    %{pool_timeout: 1000, request_timeout: 5000, pool_group: :default})
```

Options are included to show defaults and can be omitted. `pool_timeout` and `request_timeout` default to values specified in pool group configuration. If not specified in pool group configuration they default to the values in the example.

## Configuration

```
config :machine_gun,
  # Default pool group
  default: %{
    pool_size: 4,         # Poolboy size [1]
    pool_max_overflow: 4, # Poolboy max_overflow [1]
    pool_timeout: 1000,
    request_timeout: 5000,
    conn_opts: %{}        # Gun connection options [2]
  }
```

Configuration example shows defaults and can be omitted. 

 1. https://github.com/devinus/poolboy#options
 2. https://ninenines.eu/docs/en/gun/1.0/manual/gun/

## Note

When using MachineGun in a long-living process (for example genserver) make sure to handle messages in the form of `{ref, _}` tuples, which may be produced by pool timeouts [1].

 1. https://hexdocs.pm/elixir/GenServer.html#call/3
