# TODO

## Explain how hash internals relate to databases

Mention database applications. Riak, Cassandra, Voldemort are based on Dynamo. "Like Dynamo, Riak employs consistent hashing to partition and replicate data around the ring."

Dynamo: "Each data item identified by a key is assigned to a node by hashing the data item’s key to yield its position on the ring, and then walking the ring clockwise to find the first node with a position larger than the item’s position. Thus, each node becomes responsible for the region in the ring between it and its predecessor node on the ring. The principle advantage of consistent hashing is that departure or arrival of a node only affects its immediate neighbors and other nodes remain unaffected."

"Scalability: Riak automatically distributes data around the cluster and yields a near-linear performance increase as you add capacity." I might compare this by saying: "Hashes automatically distribute data around the sparse array and yield a near-linear performance increase as you increase the size of the sparse array."

In both cases, you pay a linear performance penalty at the moment you redistribute keys. In case of servers, this is O(N) whether you're adding 1 server or 100. But the more you add at a time, the less often you'll have to add them.

## Investigate Rubinius Hash

- Benchmark with it and see if it performs well
- If so, step through it and see what secrets I can glean
