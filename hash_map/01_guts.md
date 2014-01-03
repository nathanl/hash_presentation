!SLIDE
# New Plan

!SLIDE
## Guts

A digest function:

    @@@ Ruby
    index_of('appetizer') # => 2

And a sparse array:

    @@@ Ruby
    [nil, nil, 'fruit salad', nil...]


!SLIDE
# Yay!

!SLIDE
# Problems

!SLIDE
# 1. Collisions
    @@@ Ruby
    index_of('appetizer') # => 2
    index_of('marmoset')  # => 2 - D'oh!

!SLIDE
# Solution: Tuples again!
    @@@ Ruby
    [
      nil,
      nil,
      # Disambiguate!
      [
        ['appetizer', 'fruit salad']
        ['marmoset',  'larry']
      ],
      nil...
    ]

~~~SECTION:notes~~~
For a nice interface, use TupleMap!

At collisions, O(N) again...
~~~ENDSECTION~~~

!SLIDE
# 2. Waste

    @@@ Ruby
    [
      nil,       # waste
      nil,       # waste
      'thingy',
      nil,       # waste
      nil...     # waste
    ]

!SLIDE
# Solution: Get Over It!

Just kidding. Well, a little.

!SLIDE
# Hashes Are A Tradeoff

Memory for Speed

!SLIDE
# Solution: Grow As Needed

![Tradeoff](dial.jpg)


Sparse array - but not TOO sparse

!SLIDE
# "As Needed"

Eg, 

- "When any key reaches 10 collisions"
- "When sparse array has < 25% nils"

!SLIDE
# "Grow"

- Make a new sparse array
- Populate by rehashing every key

!SLIDE
# Hashing strategy

1. Decide on a starting size
2. Compute a key's raw hash value (??)
3. `index = raw_hash_value % array_size`

!SLIDE
# Modulo

Sparse array size 3.

    Raw hash value   | index
    ------------------------
    3                | 0
    7                | 1
    10               | 1
    309406           | 1
    -9384292039948   | 2

!SLIDE
# Growth & Rehashing
    Raw hash value   |% 3|% 7 
    ------------------------------
    3                | 0 | 3
    7                | 1 | 0
    10               | 1 | 3
    309406           | 1 | 6
    -9384292039947   | 2 | 1

!SLIDE
# Growth Strategy

Double size? Doesn't help much with collisions.

!SLIDE
# Solution: Ensure size is prime

Double size, then find the next prime

(Eg: 199, 401, 809, 1619, 3251, 6521...)

!SLIDE
# Proof

    @@@ Ruby
    # Collisions in range 10k to 20k
    r = (10_000...20_000)

    # Using non-primes
    r.select {|i| i % 200 == i % 400 }.count
    #=> 5000 - 50% of these values!

    # Using primes
    r.select {|i| i % 199 == i % 401 }.count
    #=> 0

!SLIDE
# Implementation

    @@@ Ruby
    primes = Prime.each # Enumerator
    # Remembers where it left off
    primes.detect {|p| p > (size * 2) }
