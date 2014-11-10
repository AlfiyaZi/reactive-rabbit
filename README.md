Reactive Streams: AMQP
====

Experimental implementation of [Reactive Streams](http://www.reactive-streams.org) for AMQP based on [RabbitMQ](https://www.rabbitmq.com/) library.

Available at Maven Central:

    libraryDependencies += "io.scalac" % "reactive-rabbit_2.11" % "0.2.0"

Examples
----

#### Akka Streams (0.9)

```Scala
import akka.actor.ActorSystem
import akka.stream.scaladsl2.{FlowMaterializer, Sink, Source}
import io.scalac.amqp.Connection


// streaming invoices to Accounting Department
val connection = Connection()
val queue = connection.consume(queue = "invoices")
val exchange = connection.publish(exchange = "accounting_department",
  routingKey = "invoices")

implicit val system = ActorSystem()
implicit val materializer = FlowMaterializer()

Source(queue).map(_.message).connect(Sink(exchange)).run()
```
