---
title: Scalable order processing with Cosmos DB on Azure 
description: Proven solution for building a highly scalable order processing pipeline using Cosmos DB on Azure.
author: alexbuckgit
ms.date: 06/26/2018
---

# Scalable order processing with Cosmos DB on Azure

This sample solution is relevant to organizations that need a highly scalable and resilient architecture for online order processing. Potential applications include e-commerce and retail point-of-sale systems, integrated with order fulfillment and inventory management functions. 

By using Azure services such as Cosmos DB and Apache Kafka, companies can eliminate the undifferentiated heavy lifting of an on-premises or IaaS deployment, while reducing costs and leveraging Microsoft's expertise in globally distributed cloud-scale data storage and retrieval. This scenario specifically addresses an e-commerce or retail scenario; if you have other needs for data services, you should review the list of available [fully managed intelligent database services in Azure][product-category].

## Potential use cases

You should consider this solution for the following use cases:

* E-commerce or retail point-of-sale front-end systems.
* Inventory management systems.
* Order fulfillment systems.
* Other integration scenarios relevant to an order processing pipeline.

## Architecture diagram

The architecture of the sample solution is depicted below.

![Sample solution architecture for a scalable order processing pipeline][architecture-diagram]

## Architecture

This solution covers key components of an order processing pipeline. The data flows through the solution as follows:

1. Events are raised in customer-facing applications (via HTTP) and various back-end systems (via Apache Kafka) as event messages, which are passed to a command messaging pipeline.
2. Each event message is ingested and mapped to one of a defined set of commands by a command processor, which executes each command after retrieving any current state needed for command execution.
3. The result of the command execution is committed to an event stream database using Cosmos DB.
4. For each database insert or update performed, an event is raised by the Change Feed by Cosmos DB. Downstream systems can subscribe any event topics that are relevant to that system.
5. All events from the Cosmos DB Change Feed are also sent to a snapshot event stream processor, which calculates any state changes caused by events that have occurred. The new state is then stored in an event stream snapshot database using Cosmos DB.  The snapshot database provides a globally distributed, low latency data source for all current state of data elements, and the event stream database provides a complete record of all events that have occurred through the architecture, which enables robust testing, troubleshooting, and disaster recovery scenarios.  

### Components

* [Cosmos DB][docs-cosmos-db] is Microsoft's globally distributed, multi-model database that enables your solutions to elastically and independently scale throughput and storage across any number of geographic regions. It offers throughput, latency, availability, and consistency guarantees with comprehensive service level agreements (SLAs). This solution uses Cosmos DB for event stream storage and snapshot storage, and leverages Cosmos DB's Change Feed features to provide data consistency and fault recovery. 
* [Apache Kafka on HDInsight][docs-kafka] is a managed service implementation of Apache Kafka, an open-source distributed streaming platform for building real-time streaming data pipelines and applications. Kafka also provides message broker functionality similar to a message queue, for publishing and subscribing to named data streams. This solution uses Kafka for processing incoming as well as downstream events in the order processing pipeline. 
* [F#](https://fsharp.org/) is a mature, open source, cross-platform, functional-first programming language. It empowers users and organizations to tackle complex computing problems with simple, maintainable and robust code. F# is an effective language for building microservices that can be deployed and tested independently from other components of the overall system. 

### Alternatives

Many technology choices are available for real-time message ingestion, data storage, stream processing, storage of analytical data, and analytics and reporting. For an overview of these options, their capabilities, and key selection criteria, see [Big data architectures: Real-time processing](/azure/architecture/data-guide/technology-choices/real-time-ingestion) in the Azure Data Architecture Guide.

### Availability

This solution's event sourcing approach allows system components to be loosely coupled and deployed independently of one another. Cosmos DB offers [high availability][docs-cosmos-db-regional-failover] and provide clear tradeoffs between consistency, availability, and performance, all with [corresponding guarantees].  Apache Kafka on HDInsight is also designed for [high availability][docs-kafka-high-availability].

Azure Monitor provides unified user interfaces for monitoring across various Azure services. For more information, see [Monitoring in Microsoft Azure](/azure/monitoring-and-diagnostics/monitoring-overview). Event Hubs and Stream Analytics are both integrated with Azure Monitor. 

For other availability considerations, see the [availability checklist][availability].

### Scalability

[TO BE ADDED: Scalability info re: Cosmos DB, Kafka]

For general guidance on designing scalable solutions, see the [scalability checklist][scalability] available in the Azure Architecture Center.

### Security

For a deeper discussion on security, see the relevant article in the architecture center.

For general guidance on designing secure solutions, see the [Cloud Security Design Patterns][security] available in the Azure Architecture Center.

### Resiliency

For general guidance on designing resilient solutions, see the [Cloud Resiliency Design Patterns][resiliency] available in the Azure Architecture Center.

## Deploy the solution

[TO BE ADDED]

## Pricing

To examine the cost of running this solution, all of the services are pre-configured in the cost calculator.  To see how the pricing would change for your particular scenario, change the appropriate variables to match your expected data volume.

We have provided three sample cost profiles based on amount of traffic you expect to get:

[TO BE ADDED: Cost details]

* [Small][small-pricing]: 
* [Medium][medium-pricing]: 
* [Large][large-pricing]: 

## Related Resources

* [Jet.com](https://jet.com) uses a more extensive version of this architecture for its end-to-end order processing pipeline. For more information, see the [jet.com technical customer profile][source-document] and [jet.com's presentation at Build 2018][source-presentation]. 
* _[Designing Data-Intensive Applications](https://dataintensive.net/)_ by Martin Kleppmann (O'Reilly Media, 2017).
* _[Domain Modeling Made Functional: Tackle Software Complexity with Domain-Driven Design and F#](https://pragprog.com/book/swdddf/domain-modeling-made-functional)_ by Scott Wlaschin (Pragmatic Programmers LLC, 2018).
* Other [Cosmos DB use cases][docs-cosmos-db-use-cases]
* [Microsoft F# Guide][docs-f-sharp]

<!-- links -->
[product-category]: https://azure.microsoft.com/product-categories/databases/
[source-document]: https://customers.microsoft.com/en-us/story/jet-com-powers-innovative-e-commerce-engine-on-azure-in-less-than-12-months
[source-presentation]: https://channel9.msdn.com/events/Build/2018/BRK3602
[small-pricing]: https://azure.com/e/74149ec312c049ccba79bfb3cfa67606
[medium-pricing]: https://azure.com/e/4fc94f7376de484d8ae67a6958cae60a
[large-pricing]: https://azure.com/e/7da8804396f9428a984578700003ba42
[architecture-diagram]: ./images/architecture-diagram-cosmos-db.png
[docs-cosmos-db]: /azure/cosmos-db
[docs-cosmos-db-change-feed]: /azure/cosmos-db/change-feed
[docs-cosmos-db-regional-failover]: /azure/cosmos-db/regional-failover
[docs-cosmos-db-guarantees]: /azure/cosmos-db/distribute-data-globally#AvailabilityGuarantees
[docs-cosmos-db-use-cases]: /azure/cosmos-db/use-cases
[docs-f-sharp]: /dotnet/fsharp/
[docs-kafka]: /azure/hdinsight/kafka/apache-kafka-introduction
[docs-kafka-high-availability]: /azure/hdinsight/kafka/apache-kafka-high-availability
[docs-event-hubs]: /azure/event-hubs/event-hubs-what-is-event-hubs
[docs-stream-analytics]: /azure/stream-analytics/stream-analytics-introduction
[docs-blob-storage]: /azure/storage/blobs/storage-blobs-introduction
[availability]: /azure/architecture/checklist/availability
[scalability]: /azure/architecture/checklist/scalability
[resiliency]: /azure/architecture/patterns/category/resiliency/
[security]: /azure/architecture/patterns/category/security