# 🥕 Carotte

## A RabbitMQ client for Gleam

[![Package Version](https://img.shields.io/hexpm/v/carotte)](https://hex.pm/packages/carotte)
[![Hex Docs](https://img.shields.io/badge/hex-docs-ffaff3)](https://hexdocs.pm/carotte/)

```sh
gleam add carotte
```

```gleam
import carotte
import carotte/channel
import carotte/exchange
import carotte/queue
import carotte/publisher

pub fn main() {
  let assert Ok(client) =
    carotte.default_client()
    |> carotte.start()

  let assert Ok(channel) =
    channel.open_channel(client)

  exchange.new("consume_exchange")
  |> exchange.declare(channel)
  queue.new("consume_queue")
  |> queue.declare(channel)

  queue.bind(
    channel: channel,
    queue: "consume_queue",
    exchange: "consume_exchange",
    routing_key: "",
  )

  publisher.publish(
    channel: channel,
    exchange: "consume_exchange",
    routing_key: "",
    payload: "payload",
    options: [],
  )

  queue.subscribe(
    channel: channel,
    queue: "consume_queue",
    callback: fn(payload, _) {
      payload.payload
      |> should.equal("payload")
    },
  )
}
```

Further documentation can be found at <https://hexdocs.pm/carotte>.

## Development

```sh
gleam run   # Run the project
gleam test  # Run the tests
```
