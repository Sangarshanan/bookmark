# Differential Dataflow

Execution models tells us how a program has to behave, It's how a set of instructions are executed and there can be several ways to execute instructions. The first and the most popular one is the Von Neumann execution model where every program is a **linear series of addressable instructions** stored in memory and the next instruction to execute depends on what happened during the execution of the current instruction.


The dataflow model is a bit different to the Von Neumann model cause it relies on a **graph representation of programs where nodes represent operations and edges represent data paths.** Dataflow is a distributed model of computation since there is no single locus of control & its also asynchronous given that the execution of a node starts when data is available at a node's input ports.


### Timely Dataflow

Timely dataflow is a low-latency cyclic dataflow computational model, It offers

- High throughput of batch processors
- Low latency of stream processors
- The ability to perform iterative and incremental computations.

Timely dataflow is a computational model based on a directed graph in which **stateful vertices send and receive logically timestamped messages along directed edges.** The dataflow graph may contain nested cycles, and the
timestamps reflect this structure in order to distinguish data that arise in different input epochs and loop iterations. The resulting model supports concurrent execution of different epochs and iterations, and explicit vertex notification after all messages with a specified timestamp have been delivered.

**Stateful vertices asynchronously receive messages and notifications of global progress while Edges carry records with logical timestamps that enable global progress to be measured.** Logical timestamps can effectively reflect structure in the graph topology such as loops and make the model suitable for tracking progress in iterative algorithms.

### Differential Dataflow

Differential dataflow is built over timely dataflow is a declarative, data-parallel, incremental framework for performing computations over changing data.

**Differential computation deals with updating a computation as its inputs change**

- **Declarative** Computations are always performed by functions called operators whose inputs and outputs are collections so we never have to think about which data structure to use with this framework. Collections are generalized, multi-versioned multisets that store information in terms of `(data, time, multiplicity)` triples which means **at some discrete time we added multiplicity copies of data in our bag** multiplicity is allowed to be negative. Differential uses collections to represent changes over time and thus positive multiplicities denote inserts and negative multiplicities denote deletes

- **Data parallel** means the framework parallelizes computations by slicing and dicing data to different workers (when possible). Each worker gets a local copy of the data it needs and is then free to do its computation as if it were the only one. As the programmer you are free to write code and can be blissfully unaware of having multiple threads (most of the time).

- **Dataflow** means that you can’t think of your program as a single thread of control, where there are a sequence of instructions Instead, you have various functional operators, like `map`, `reduce` or `join`. Each operator takes one or more collections as input, and spits out a new collection as output. All you can do to write programs is stitch these operators together into a dataflow graph by sending the outputs of some operators into the inputs of others.

- **Incremental** means that as you send more data into your program, Differential will not recompute answers from scratch and instead we can re-use previously computed results to figure out new answers for changing inputs.

Existing computational models for processing continuously changing input data are unable to efficiently support iterative queries except in limited special cases. This makes it difficult to perform complex tasks, such as social-graph analysis on changing data at interactive timescales, which would greatly benefit those analyzing the behavior of services like Twitter. Differential computation extends traditional incremental computation to allow arbitrarily nested iteration and makes it easy to program intractable algorithms with incrementally updated strongly connected components and integrate them with data transformation operations to obtain practically relevant insights from real data streams.

### Notes:

- <https://courses.cs.washington.edu/courses/cse548/06wi/slides/dataflow.pdf>
- <https://www.microsoft.com/en-us/research/wp-content/uploads/2013/11/naiad_sosp2013.pdf>
- <https://news.ycombinator.com/item?id=25867693>
- <https://materialize.com/life-in-differential-dataflow/>
- <https://timelydataflow.github.io/differential-dataflow/>
- <https://www.youtube.com/watch?v=ZN7nOwJTSZ0>
