# TODO

## Talk about https://en.wikipedia.org/wiki/Amortized_analysis

Yeah

## Explain how hash internals relate to databases

Mention database applications. Riak, Cassandra, Voldemort are based on Dynamo. "Like Dynamo, Riak employs consistent hashing to partition and replicate data around the ring."

Dynamo: "Each data item identified by a key is assigned to a node by hashing the data item’s key to yield its position on the ring, and then walking the ring clockwise to find the first node with a position larger than the item’s position. Thus, each node becomes responsible for the region in the ring between it and its predecessor node on the ring. The principle advantage of consistent hashing is that departure or arrival of a node only affects its immediate neighbors and other nodes remain unaffected."

"Scalability: Riak automatically distributes data around the cluster and yields a near-linear performance increase as you add capacity." I might compare this by saying: "Hashes automatically distribute data around the sparse array and yield a near-linear performance increase as you increase the size of the sparse array."

In both cases, you pay a linear performance penalty at the moment you redistribute keys. In case of servers, this is O(N) whether you're adding 1 server or 100. But the more you add at a time, the less often you'll have to add them.

## Investigate Rubinius Hash

- Benchmark with it and see if it performs well
- If so, step through it and see what secrets I can glean

Also test jRuby's just for fun.

## Ideas to incporporate

- From http://www.cs.cornell.edu/courses/cs312/2008sp/lectures/lec20.html
  - "a sophisticated implementation can defer the O(n) cost of resizing its hash tables to a point in time when it is convenient to incur it: for example, when the system is idle."
  - "Load factor" is average elements per bucket = keys.count/sparse_array.length. That's typically the measure for when to rehash; "between 1 and 3" is suggested. Too high and performance suffers; too low and memory is wasted.
  - A really sophisticated hash would also size down to save memory if enough keys are deleted.
  - "Because resizing is not visible to the client, it is a benign side effect. Each resizing operation takes O(n) time where n is the size of the hash table being resized. Therefore the O(1) performance of the hash table operations no longer holds in the case of add: its worst-case performance is O(n)."
  - THE BEST QUOTE: "Each resizing operation takes O(n) time where n is the size of the hash table being resized. Therefore the O(1) performance of the hash table operations no longer holds in the case of add: its worst-case performance is O(n).
This isn't as much of a problem as it might sound, though it can be an issue for some real-time computing systems. If the bucket array is doubled in size every time it is needed, then the insertion of n elements in a row into an empty array takes only O(n) time, perhaps surprisingly. We say that add has O(1) amortized run time because the time required to insert an element is O(1) on the average even though some elements trigger a lengthy rehashing of all the elements of the hash table.
To see why this is, suppose we insert n elements into a hash table while doubling the number of buckets when the load factor crosses some threshold. A given element may be rehashed many times, but the total time to insert the n elements is still O(n). Consider inserting n = 2k elements, and suppose that we hit the worst case, where the resizing occurs on the very last element. Since the bucket array is being doubled at each rehashing, the rehashes must all occur at powers of two. The final rehash rehashes all n elements, the previous one rehashes n/2 elements, the one previous to that n/4 elements, and so on. So the total number of hashes computed is n hashes for the actual insertions of the elements, plus n + n/2 + n/4 + n/8 + ... = n(1 + 1/2 + 1/4 + 1/8 + ...) = 2n hashes, for a total of 3n hashing operations.
No matter how many elements we add to the hash table, there will be at most three hashing operations performed per element added. Therefore, add takes amortized O(1) time even if we start out with a bucket array of one element!"
- "Notice that it was crucial that the array size grows geometrically (doubling). It is tempting to grow the array by a fixed increment (e.g., 100 elements at time), but this causes n elements to be rehashed O(n) times on average, resulting in O(n2) asymptotic insertion time!"

I declare my hash a success because 1) all hash tables take O(n) time to rehash, including the native Ruby implementation; it is faster and hence has a shallower slope, but it's unavoidably a linear operation, 2) that's OK because the amortized time is still O(1), 3) if we really wanted to get fancy, we could do the work in a thread or something so that you don't feel the insertion lag, 4) if you had a pretty good idea how many keys you would insert, you could start the sparse array large enough so that you didn't have to rehash at all.

## Presentation

- Side-by-side graph comparison - draw lines
