!SLIDE
# New Plan

!SLIDE
## Guts

Sparse array:

    @@@ Ruby
    [nil, nil, 'fruit salad', nil...]

Plus a digest function:

    @@@ Ruby
    index_of('appetizer') # => 2

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
# Grow

- Make a new sparse array
- Populate by rehashing every key

!SLIDE
# As Needed

Eg, 

- "When any key reaches 10 collisions"
- "When sparse array has < 25% nils"

!SLIDE
# Hashing strategy

1. Decide on a starting size
2. Compute a key's raw hash value
3. `index = raw_hash_value % array_size`

!SLIDE
# Modulo

Sparse array size 3.

    Raw hash value   | index
    ------------------------
    1                | 1
    5                | 2
    6                | 0
    309406           | 1
    -9384292039947   | 0

!SLIDE
# Growth & Rehashing
    Raw hash value   |% 3|% 7 
    ------------------------------
    1                | 1 | 1
    5                | 2 | 5
    6                | 0 | 6
    309406           | 1 | 6
    -9384292039947   | 0 | 1

!SLIDE
# Growth Strategy

Double size? Doesn't help much with collisions.

!SLIDE
# Solution: Ensure size is prime

Double size, then find the next prime

(Eg: 199, 401, 809, 1619, 3251, 6521...)

    @@@ Ruby
    primes = Prime.each # Enumerator
    # Remembers where it left off
    primes.detect {|p| p > (size * 2) }

!SLIDE
# Proof

    @@@ Ruby
    # Collisions in range 10k to 20k
    r = (10_000...20_000)

    # Using non-primes
    r.select {|i| i % 200 == i % 400 }.count
    #=> 5000 - half the range!

    # Using primes
    r.select {|i| i % 199 == i % 401 }.count
    #=> 0
