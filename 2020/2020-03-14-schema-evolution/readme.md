# Schema evolution: From SQL to NoSQL to Event Sourcing

## SQL

Traditionally, database have been structured around an SQL schema. When done right, an SQL schema is defined after careful study of the domain, a well founded choice of entities and attributes and a mathematically sound transformation of the resulting model into a set of interlinked database tables. The assumption is that it the set of relevant entities, attributes and relations can be determined in advance and doesn't change (much) during the life time of a software system. This may have been a valid assumption in the previous century, but currently I am often confronted with:
* Time restrictions during the development of a proof-of-concept (PoC) or a minimally-viable-product (MVP) that preclude the definition of a complete schema.
* Changes in markets, technologies and/or legislation that require a complete reassessment of the domain.
* New opportunities imply new performance requirements that are better served with a data model that is differently structured.
* The necessity to change the structure of large volumes of stored data.
* New technologies make specific queries feasible (_e.g._, Elastic Search, GraphQL)
* Marshalling and data transfer is often more important that efficient storage.
* Queries on normalized data take much more time than retrieval from a document database, search index, or tuple store.

## NoSQL

It is therefore not surprising that the beginning of this century showed a great interest in NoSQL database technologies. Many of these technologies focus on a very specific class of domains that benefit from a particular form of storage. No alternative to SQL has arisen that proved to be equally universal. Still the inflexibility of committing to a schema slows down software development teams around the globe.

Many teams now use a schema management tool like LiquiBase or Flyway to manage schema changes. This is much better than writing instructions for a database administrator (DBA) to follow during a release (often with hours of down-time to make sure that data is not modified while backing up and data migration). It is still a huge pain for each change bigger than adding a column to a table, however.

##  Event Sourcing and CQRS

There is a better way. Model the _events_ in a domain instead of modeling the entities. When a type of event changes more than the addition of a few (optional or derivable) attributes, introduce a new type of event. Events can be stored in a special type of NoSQL database: an Event Store. The most elegant and performant implementation of an Event Store that I know of is [AxonServer](https://axoniq.io/product-overview/axon-server). Where a SQL based database management system is built around a structured _query_ language (SQL), that has some features that make it possible to insert and update information, an Event Sourced system is best built around separate facilities for processing commands and processing queries. This is generally called the Command / Query Responsibility Segregation (CQRS) pattern.

### Commands

In CQRS, for each update to the date, a command has to be sent to the system. Each command is checked against a projection of all past events to verify that the command is valid in the current state. (To make these projections efficient it is a good idea to use [Domain Driven Design](https://dddcommunity.org/) (DDD) to choose appropriate [aggregate](https://martinfowler.com/bliki/DDD_Aggregate.html) boundaries.) If a command is valid, the associated event(s) are added to the Event Store. This guarantees the transactionality of the system.

### Queries

On the query side, events are processed in order by each event processor to build whatever representation is currently practical to satisfy the needs of (one of) the user interface(s). Multiple representations of the same information can exist next to each other. Some of these might still be in a developmental stage, while others carry the full load of the production traffic. There is no need to fight philosophical battles of how the data should be represented. If an alternative representation shows promise: just add it. If new events seem relevant: add them. When a query model no longer serves a purpose: remove it. When events are no longer used: stream the old event store to a new store, filtering out events that are no longer relevant.

### Don't pick the worst of both worlds

It is of course possible to use a SQL database to store query models (or even the event store). Sometimes the habit of normalizing data the SQL way is hard to break. Then events are used to fill a complicated schema of tables. Foreign keys straddle aggregate boundaries, resulting in inconsistencies. Luckily it is quite easy to escape from this situation. Just build new query models that respect aggregate boundaries and that build response documents and/or query indexes that are of a direct benefit to a User Interface rather than conforming to some idealized model of real world entities.

## Evolve!

In summary: use Event Sourcing and CQRS to evolve the way data is stored in such a way that it is flexible and performant while remaining elegant and maintainable.