## Intro to Distributed Systems
### Distributed Systems Overview
A distributed system can be generically defined as a collection of resources on distinct physical components that appear to function as a single coherent system<br>

Examples: the Internet, Embedded SoC, Mobile Gaming <br>

The Benefits of having a distributed system:<br>
- Geographic distribution of users, peers, providers, etc.<br>
- The distribution of resources can match that of the population<br>
- Cost per resource (processor, memory, storage, network) at scale<br>
- Capacity (processor, memory, storage, network)<br>
- Failure model (high chance of 1 of N failure, lower chance of N of N failure)<br>
- Localized risks<br>

## Computing Over Networks
### Challenges of Distributed Systems

Fundamental distributed systems challenges:<br>

- Timing:<br>
  - How do processes on different networked devices determine the ordering of events that took place? The "Who shot first?" problem<br>
- Concurrency: H<br>
  - How do processes on different networked devices safely access/use a commonly shared resource on yet another device?<br>
- Robustness:<br>
  - How do processes on different networked devices deal with imperfections in the networks that connect them?<br>
- Consistency:<br>
  - How do we guarantee the same outcome from a process regardless of where/how the process runs?<br>

### Timing
<ins>**Reliability**</ins>: The delivery of all packets in order, with correct content, without duplicates. <br> 
- Reliable transport provides applications with guaranteed delivery of message segments in order, with verified content, without duplication (e.g., TCP; useful for bulk data delivery)<br>
  - Reliability is traded for latency (no bounded delay guarantee) and performance overhead (potentially lots of retransmissions)<br>

Reliability != timeliness
- The two are often in direct conflict
- Reliability is only guaranteed on a per-session basis, not globally
- In general, <ins>**time**</ins> is a major challenge to be dealt with.

### Concurrency
<ins>**Coordination**</ins>: 
- Finding “stuff” is tricky (content, resources, …)
- Maybe someone/something needs to keep track of all the things?
- Coordination is a new (or at least amplified) challenge
- In general, <ins>**management**</ins> is a major challenge to be dealt with

### Robustness
<ins>**(Partial) Failure**</ins>: 
- Servers can fail
- Especially if the system comprises a large number of cheap/commodity servers instead of one monolithic, high-performance, many-9s availability server
- If one server fails, the entire system doesn’t fail
- Partial failure usually allows for recovery (ideally invisible to the client) and hopefully doesn’t affect availability
- In general, <ins>**fault tolerance and recoverability**</ins> are highly desirable

Failures are difficult to handle because it is difficult for us to know which specific processing state the failure occurred.<br>

### Consistency
<ins>**Consistency**</ins>: 
- If many clients are interacting with the same data, how do we guarantee that everyone agrees on the same value?
  - Mutexes/locks are great, but how can we extend that from a single OS to an entire DS?
- If we can’t, can the clients at least detect that there’s a problem?
- Consistency is one of the biggest challenges in asynchronous, concurrent, distributed systems

## Coordinating Operations over a Network
### Remote Operation
- I have a task (e.g., compute something, access some data), and I need another networked component to help me
- This is abstracted as a function/subroutine/… -> y = f(x)
  - y: what is the desired outcome that I’m asking for
  - x: what inputs am I providing as part of my request
  - f: what work is being done to provide my outcome, and I don’t care where/how it’s implemented

### Local Operation
- If this was done locally:
  - Find f and x in memory
  - Load x in register(s)
  - Branch to subroutine
  - Subroutine computes y
  - Load y in register(s)
  - Branch return
  - Store y in memory

- The OS hides most of this, so the dev sees a simple <ins>**function/method call**</ins><br>

### Local vs. Remote Operation
Apart from the local operations, <br>
- At the remote entity:
  - Get messages from the other party
  - Unpack f and x (how?)
  - Load x in register(s)
  - Subroutine, compute y
  - Pack into reply message, send
<img width="1065" alt="Screen Shot 2024-01-23 at 1 28 00 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/dc416906-934a-483a-a146-625aad0080a8">

### Remote Procedure Calls (RPC)
- RPC is a way to provide a programmer-friendly abstraction to handle the complexity of coordination required for remote services
- RPC moves inter-procedure communication from stack to network
- Attempts to provide an interface for the programmer that still looks like a local procedure call

### Structure of the RPC
- Client-side:
  - Client applications only see a local “stub” (aka proxy) that presents a simple function/method/API for local use
    - The stub layer abstracts away most networking details
  - Client app calls f(x) presented by stub, stub interacts with library to perform all the details of the remote call but doesn't "do" f(x)
  - Nothing about transport has to change
- Server-side:
  - Same idea on the server side, where the actual service provided doesn’t care about underlying network interactions, just "doing" f(x)
    - Similar server stub (aka “skeleton”) abstracts away all networking details
  - Stub interacts with the library to handle incoming remote requests, decoding them in a way that’s useful for the server, calls the local subroutine, and handles the reply
  - Nothing about transport has to change
<img width="247" alt="Screen Shot 2024-01-23 at 1 32 33 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/a5748446-0d14-4eaa-9ab3-0ad2f9ed535c">
<img width="267" alt="Screen Shot 2024-01-23 at 1 32 46 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/205a8aa2-3e07-4544-a3ae-f6577719f344">

### Benefits and Difficulties of the RPC
Benefits:<br>
- All the programmer sees when using RPC is the immediate call to the stub and the corresponding return
- This provides:
  - Ease of programming with familiar model (just call a function)
  - Hidden complexity and architecture of the remote component
  - Automation of distributed computation

Difficulties:<br>
- While the abstraction and ease of programming are great, some challenges need to be addressed
  - Caller and callee procedures run on different machines, using data in different address spaces, perhaps in different environments/OS, …
  - Must convert to the local representation of data (e.g., variable type, endian conversion)
  - Components or networks can fail

### Marshalling
- Stubs are responsible for marshaling and unmarshalling these interactions
  - Client stub marshals arguments into machine-independent format ⇒ sends request ⇒ waits for response ⇒ unmarshals result
  - Server stub unmarshals arguments and builds stack frame ⇒ calls procedure ⇒ marshals results and replies

- Since marshaling relies on standards and conventions in a variety of places, rather than making some assumption about what those are, stubs rely on an <ins>**interface definition language (IDL)**</ins>

- There are many such IDLs supported by various libraries and middleware, often resembling XML, JSON, TLV, etc.

### RPC Diagram
<img width="1086" alt="Screen Shot 2024-01-23 at 1 39 03 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/cfdfaaa8-ab2d-4318-9418-e1707a216553"><br>

### RPC By Reference
- Early RPC implementations were for stateless/idempotent subroutines, so there was no notion of call-by-reference
  - There is no shared memory space, so what value is a pointer?

- RPC is often limited to call-by-value or call-by-copy-and-restore, where values must be explicitly sent over the network
  - This creates some complications for complex data types and especially for custom data types
  - It is possible to maintain state and create generic "references", like pointers but not for memory
  - Different levels of integration are supported by different systems, depending on the hetero/homogeneity of the system, languages, etc.

### Challenges of RPC
1. Failures

- Partial Failure:
  - In local computing:
    - If the machine fails, the application fails
    - There are much bigger concerns than the application at this point
  -  In distributed computing:
    - If a machine fails, part of the application fails and part doesn’t
    - Can you tell the difference between machine failure and network failure?

## Replication

-  Reasons for Replication
  - Increased throughput/bandwidth/parallelism
  - Increased fault tolerance
  - Improve latency by storing content closer to distributed consumers, e.g., “at the edge”

### Primary and Secondary Replicas
- Primary replicas
  - Exact copies of the original
  - Common for file servers, databases, etc.

- Secondary replicas (eg. Github repo not updated)
  - Derivable from the original
  - Lower fidelity in some way
  - Caches (possibly stale)
  - Thumbnails (lower resolution, faster to transmit, smaller to store)
  - Compressed copies (slower to access, harder to update)

- Primary takes all updates
  - Secondary may get occasional/periodic updates or “listen” to primary updates
  - Primary needs additional overhead to manage, secondary can be managed when possible
  - Frequency of secondary updates can be balanced for desired overhead, consistency, usefulness, etc.

- Secondaries may be read-only caches
  - Leaving primary as the definitive and most up-to-date copy
  - An update of secondary affects how useful it is as a backup for primary

### Efficient Replica Management
- Rather than enforce perfect replication (which has high overhead), it’s often reasonable to allow replicas to vary from each other slightly (e.g., one can be out of date
  w.r.t. others) in trade for significantly less latency/overhead

- Need to be careful not to overly complicate the management of replication, or we could end up with more latency/overhead

- Goal: we want to achieve <ins>one-copy semantics</ins>, meaning we can perform read and write operations on a collection of replicas with the same results as if there were one nonreplicated object

### Quorum
- A group of servers trying to provide one-copy semantics

- Setup of the Quorum
  - N primary replicas, no secondary replicas
  - One-copy semantics
    - Each "read" makes a local copy of multiple replicas, determines which is "correct" and discards others
    - Each "write" overwrites multiple replicas with a single local copy
      - Writes are not edits, they are replacements
      - A "read-then-write" approach is typically used

  - Concurrency controls are assumed
    - All reads and writes are "safe"

### Concurrency Control Addresses Conflict
- Replication conflict
  - Any situation where concurrent actions break the usefulness of replicas

- Writes cause conflicts
  - Write-write
    - If there are 2 replicas, A writes to one while B writes to the other, which is the newest?
    - If there is 1 replica, both A and B write, the outcome is uncertain (race condition)

  - Write-read
    - If there are 2 replicas, A writes to one while B reads from the other, then B reads an old file
    - If there is 1 replica, A writes as B is reading, B may read a mix of old and new content

- Reads don’t cause conflicts
  - Read-read is safe
    - But caution is still needed while reading…

### Read/Write of Replicated Data
- If a large number of users can access and modify replicated data, we have to be careful about who is reading/writing which replica

- Ex: if there are four replicas f1, f2, f3, f4 of a file, what happens if one user writes f3 and f4 while another user reads f1 and f2?

### Read and Write Quora
- Read quorum: Number of servers R a reader should read from
- Write quorum: Number of servers W a writer should write to
- What is the relationship between these?
- Can we tune them to achieve different goals?<br>

<img width="952" alt="Screen Shot 2024-02-06 at 5 43 24 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/d06f50d5-e7c7-4311-86fe-d43d20acd84b"><br>

### Quora Must Overlap
- The read quorum and the write quorum must overlap
  - Why? Without overlap, readers could miss-write and get old value
  - R + W > N  is a necessary condition for correctness
- Requires the ability to identify the most recent version among the replicas that are read (more in a few slides)<br>

<img width="1140" alt="Screen Shot 2024-02-06 at 5 44 17 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/7557db30-986e-48ea-969a-0fe050e8cd4c"><br>
<img width="1118" alt="Screen Shot 2024-02-06 at 5 44 37 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/452a096c-91e8-4c52-8ca3-a4c3832dfca8"><br>

### Tuning a Quorum
- Often true that reading is more common the writing (I access files more often than I update them)

- Increase W, i.e., larger write quorum
  - More redundancy for robustness
  - More local to consumers
  - Higher write cost (network and device throughput, contention, long tail, etc.)

- Decrease R, i.e., smaller read quorum
  - Lower cost to read (network and device throughput, contention, long tail, etc.)
  - More choices of where to read (closer to the user)
  - More expensive writes (but that’s ok since they’re less frequent)

- Larger overlap
  - More tolerance for failure

- Common Practice:
  - Read-one / write-all (i.e., R=1, W=N) is the most common
    - Reads are more common than writes, so make them easy
      - Read gets most current data (requirement)
      - Reads can choose any replica (nearby, failure, etc.)
      - Reads require low bandwidth
      - Makes common case safe and fast

### Version Determination
- If every replica has a version number (like a timestamp or sequence number), then determining which read replica is the newest is easy
Simply choose the replica with the "highest" version number
  - Read〈f,tf〉from R servers ⇒〈f1,tf1〉,〈f2,tf2〉,...,〈fR,tfR〉
  - Keep fj, where j = argmaxi tfi
- Still need to ensure that the overall newest version is guaranteed to be within any set of R replicas (e.g., via quorum)

### Majority Read Quorum
- Make W large enough to guarantee that for any R replicas, strictly more than R/2 of them are the newest version
  - the majority of any read quorum represents the most recent version of the replica
    - E.g., if 3 of the 5 replicas I read are the same, they are guaranteed to be the most recent version
  - the overlap between the write quorum and the read quorum must include the majority of the read quorum
- Overlap must be a majority of any read quorum
  - must write to all but a quorum minority
    - W > (N - R/2)
  - And this doesn’t even account for any read/write failures<br>
<img width="914" alt="Screen Shot 2024-02-06 at 5 54 17 PM" src="https://github.com/AllenJWZhu/CMU_Course_Notes/assets/55110211/fe08c4e2-6d1b-45e6-9a40-80d1c7ec7ea3"><br>
- This is very expensive
  - But we don't even need a reference of time at all and guarantee we could still do versioning!

### Write-all
- If W = N (i.e., "write-all"), then it’s impossible for an old replica to be present in the system since every replica is always overwritten
- Any read of any replica guarantees it is the newest overall replica
  - However, if one write fails, no one can read from the entire system until that server backs up.
    - This could lead to a lockdown system...
    - Thus, this is requiring the system to be perfect thus resulting in slow results.

### Locking
- Uncontrolled concurrent writes can break quorum discipline (and maybe even corrupt data)
  - Also could break version number increment

- What to lock?
  - Object at servers, not whole servers

- How many to lock?
  - L ≥ R ⇐ lock quorum must cover most recent version
  - L ≥ W ⇐ lock quorum must cover every write to protect version and data
  - L ≥ max(R, W) ⇐ lock quorum must cover all reads and writes of updating object

### Failures
- If the overlap between read and write Quora is strictly greater than F, then F failures can be tolerated
  - With version numbers: R + W - F > N
  - Without version numbers: W + R/2 - F > N
  - Still guaranteed to see an up-to-date version
- If not covered by overlap, the failure model becomes important

### Alternatives to Static Quora
- A static quorum is a pre-defined quorum (like our examples)
  - Not adaptive in terms of membership, connectivity, etc.
- We have counted each replica equally (one replica = one vote), but this is not required
- Alternatively, we can assign weights to different hosts based on any reasonable, observable metric; quorum rules still apply, but weighted
- Examples:
  - More weight to more reliable hosts (more unreliable hosts for the same robustness)
  - Less weight to caches or secondary replicas, due to expiration and staleness

### Coda Version Vectors
- CVVs are a form of vector logical timestamp (remember those?)
- A CVV contains one entry for each server, which is the version number of the file on the corresponding server (ideally, all equal)
- If a server doesn’t get an update to the file, its CVV entry will be lower than others

- A Coda client requests a file via a three-step process:
  - It asks all replicas for their version number
  - It requests the file from the replica with the largest version number
  - If the servers don't agree about the file’s version, the client can detect and inform them of the conflict
    - A conflict exists if two CVVs are concurrent, which indicates that each server has seen some but not all changes

- Ideally, a Coda client writes a file as:
  - The client sends the file and original CVV to all servers
  - Each server increments its entry in the file's CVV and sends an ACK to the client
  - The client merges the entries from all of the servers and sends the new CVV back to each server
  - If a conflict is detected, the client can inform the servers, so that it can be resolved automatically, or flagged for mitigation by the user
