# The (Spanish) RAGE stack

> Management summary: big software systems are complex, and they need to evolve. RAGE is a technology stack that keeps the complexity in plain view rather than hide it. RAGE is based on design choices that favor the long-term view.

In general I am not very fond of abbreviations. I typically work in large organisations than tend to make up tons of stuff as they go along. Every half-baked idea is summarized in an acronym or TLA (three-letter abbreviation) to help team members to align their actions with the local context. After a while the cluster of self-invented terms also provides the people involved with a warm, fuzzy feeling of belonging to the in-crowd.

The useful function an acronym _can_ provide, however, is the function that a lighthouse has too: a beacon. This kind of acronym helps new people to get their bearings. "Ah, so we're using MEAN, so this is an all JavaScript universe!".

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
* React is a user interface library with a huge market-share because of [reasons](https://medium.com/@TechMagic/reactjs-vs-angular5-vs-vue-js-what-to-choose-in-2018-b91e028fa91d). In my opinion, the fact that React does **not** provide two-way data binding between model and view is an advantage, not a lack of functionality. The commitment to one-way data binding keeps fundamental complexity of user interfaces in plain view.
* AxonServer provides an elegant API for routing commands, events, and queries as well as for persisting events.
* Go is a type-safe programming language that compiles to machine code. Executables created using Go perform extremely well en start up very fast. I confess that I first considered using Haskell (you HEAR?), because I think functional programming shows tremendous promise, but I was disenchanted by the lack of mature libraries for _e.g._, gRPC. It is also much easier to find experienced Go coders.
* Envoy (possibly extended with a proper service mesh control plane like Istio) provides an off the shelf component that can be used to manage the complexities of interacting micro-services.
* All the building blocks can use gRPC to communicate. gRPC provides efficiency, streaming, and schema evolution.
* The payload of the messages exchanged with AxonServer can be encoded using Protocol Buffers. This provides schema evolution is interoperable with gRPC. Using the same technology for encoding messages as for communicating with external components reduces complexity.

Let's be clear about one thing, however: in the RAGE framework, complexity will not be hidden like a boggart in a cupboard, ready to rain fear, uncertainty and doubt on innocent newcomers and weary veterans alike.

Are we there yet? No. I am working on an [archetypal example](https://github.com/jeroenvanmaanen/archetype-go-axon). One of the benefits of using off-the-shelf components for everything except the presentation layer and the business logic is that you can use the example project to start a very small application based on the RAGE stack. when the system stays small, RAGE is not in the way. When the systems needs to grow or integrate with other systems, using RAGE provides natural ways to evolve to the next level.

## Links

* [React](https://reactjs.org/): A JavaScript library for building user interfaces
* [AxonServer](https://axoniq.io/product-overview/axon-server): Zero-configuration message router and event store for Axon
* [Go](https://golang.org/) is an open source programming language that makes it easy to build simple, reliable, and efficient software
* [Envoy](https://www.envoyproxy.io/) is an open source edge and service proxy, designed for cloud-native applications
* [Istio](https://istio.io/): Connect, secure, control, and observe services
* [Elastic Search](https://www.elastic.co/): Search and analyze your data in real time
* [Protocol Buffers](https://developers.google.com/protocol-buffers) are a language-neutral, platform-neutral extensible mechanism for serializing structured data
* [Nix](https://nixos.org/nixos/nix-pills/) is a purely functional package manager and deployment system for POSIX
