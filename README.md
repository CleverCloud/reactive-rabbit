Reactive Streams: AMQP
====

[![Build Status](https://travis-ci.org/ScalaConsultants/reactive-rabbit.svg?branch=master)](https://travis-ci.org/ScalaConsultants/reactive-rabbit)

[Reactive Streams](http://www.reactive-streams.org) driver for AMQP protocol. Powered by [RabbitMQ](https://www.rabbitmq.com/) library.

Available at Maven Central for Scala 2.10 and 2.11:

    # for scala stream 1.X branch
    libraryDependencies += "io.scalac" %% "reactive-rabbit" % "1.0.3"
    # for scala stream 2.X branch
    libraryDependencies += "io.scalac" %% "reactive-rabbit" % "2.0.0"

Example
----

#### Akka Streams - 1.0

```Scala
import akka.actor.ActorSystem
import akka.stream.ActorMaterializer
import akka.stream.scaladsl.{Sink, Source}
import io.scalac.amqp.Connection


// streaming invoices to Accounting Department
val connection = Connection()
val queue = connection.consume(queue = "invoices")
val exchange = connection.publish(exchange = "accounting_department",
  routingKey = "invoices")

implicit val system = ActorSystem()
implicit val mat = ActorMaterializer()

Source(queue).map(_.message).to(Sink(exchange)).run()
```


#### Akka Streams - 2.0

```Scala
import akka.actor.ActorSystem
import akka.stream.ActorMaterializer
import akka.stream.scaladsl.{Sink, Source}
import io.scalac.amqp.Connection


// streaming invoices to Accounting Department
val connection = Connection()
val queue = connection.consume(queue = "invoices")
val exchange = connection.publish(exchange = "accounting_department",
  routingKey = "invoices")

implicit val system = ActorSystem()
implicit val mat = ActorMaterializer()

Source.fromPublisher(queue).map(_.message).to(Sink(exchange)).run()
```
