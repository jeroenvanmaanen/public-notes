# The (Spanish) RAGE stack

In general I am not very fond of abbreviations. I typically work in large organisations than tend to make up tons of stuff as they go along. Every half-baked idea is summarized in an acronym or TLA (three-letter abbreviation) to help the people involved to align their actions with the local context. After a while the cluster of self-invented terms also provides the people involved with a warm, fuzzy feeling of belonging to the in-crowd.

The useful function an acronym _can_ provide, however, is the function that a lighthouse has too: a beacon. Then the acronym helps new people to get their bearings. "Ah, so we're using MEAN, so this is an all JavaScript universe!".

Over the years I have used many stacks: DyTPL (a home-grown Python stack before there was Django), LAMP, Tomcat/Apache/SQL (where SQL could range from Oracle PL/SQL to Sybase T-SQL to MySQL, to PostgreSQL), J2EE, and, more recently, Spring / Spring Boot. Each of these stacks follows the same lifecycle. It starts as an attempt to get free from some huge disadvantage of the dominating 'best-practice'. More and more people get drawn to the new stack that looks so promising. Solutions are sought to adapt the stack to the needs of basically everyone, but especially to the whims of people who prefer to put complexity in a place where it is no longer visible. Then, all this hidden complexity turns out to be a huge disadvantage, and the world is ready for the invention of a new stack. By that time a following of religious supporters make sure that the overgrown caricature of the original ideas is used to torture newcomers a often as possible.

I believe that it is time for a new stack, RAGE:

* **R**eact
* **A**xonServer
* **G**o
* **E**nvoy

Maybe extended with Elastic Search (**ES** can also mean España, hence Spanish).

Why? Well, for a number of reasons:

* Contrary to de MEAN supporters I think that the presentation layer and the business/persistence layer are sufficiently different to warrant different programming languages and tool sets.
* Business-logic components should have sub-second startup times.
* Business-logic components should not comprise hidden complexity.
* Event sourcing allows for better schema evolution than SQL.
* Complexity should be plainly visible, but clearly separated from custom-made components.
* AxonServer provides an elegant API for routing commands, events, and queries as well as for persisting events.
* Envoy (possibly extended with a proper service mesh control plane like Istio) provides an off the shelf component that can be used to manage the complexities of interacting micro-services.
* All the building blocks can use gRPC to communicate. gRPC provides efficiency, streaming, and schema evolution.
* The payload of the messages exchanged with AxonServer can be encoded with protobuf. This provides schema evolution is interoperable with gRPC. Using the same technology for encoding messages as for communicating with external components reduces complexity.

Let's be clear about one thing, however: in the RAGE framework, complexity will not be hidden like a boggart in a cupboard, ready to rain fear, uncertainty and doubt on innocent newcomers and weary veterans alike.

Are we there yet? No. I am working on an [archetypal example](https://github.com/jeroenvanmaanen/archetype-go-axon).
