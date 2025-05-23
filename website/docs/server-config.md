# Server config

To read generally about ways to configure your Devnet instance, check out the [CLI section](./running/cli.md).

## Host and port

Specify the host and the port used by the server with `--host <ADDRESS>` and `--port <NUMBER>` CLI arguments. If running with Docker, check out the [port publishing docs](./running/docker#container-port-publishing).

## Logging

By default, the logging level is `INFO`, but this can be changed via the `RUST_LOG` environment variable.

All logging levels: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`

To specify the logging level and run Devnet on the same line:

```
$ RUST_LOG=<LEVEL> starknet-devnet
```

or if using dockerized Devnet:

```
$ docker run -e RUST_LOG=<LEVEL> shardlabs/starknet-devnet-rs
```

By default, logging of request and response data is turned off.
To see the request and/or response body, additional levels can be specified via the `RUST_LOG` environment variable: `REQUEST` for request body, `RESPONSE` for response body.

:::note

Logging request and response requires at least logging level `INFO`.

For example, the following two commands will log request and response data with log level `INFO`.

```
$ RUST_LOG="REQUEST,RESPONSE" starknet-devnet
```

```
$ RUST_LOG="REQUEST,RESPONSE,INFO" starknet-devnet
```

:::

## Timeout

Specify the maximum amount of time an HTTP request can be served. This makes it possible to deploy and manage large contracts that take longer to execute.

```
$ starknet-devnet --timeout <SECONDS>
```

## Request size limit

There is no HTTP request size limit, but take care when declaring large classes! Devnet is supposed to follow the limits specified in [Starknet Chain Info](https://docs.starknet.io/resources/chain-info/#current_limits). So the current upper limits configured in Devnet are:

- contract class size: 4089446 bytes
- contract bytecode size: 81920 felts
- Sierra length: 80000 felts

## API

Retrieve the server config by sending a `GET` request to `/config` or `JSON-RPC` request with method name `devnet_getConfig` and extracting its `server_config` property.

```
$ curl localhost:5050/config | jq .server_config
```

```
JSON-RPC
{
    "jsonrpc": "2.0",
    "id": "1",
    "method": "devnet_getConfig"
}
```
