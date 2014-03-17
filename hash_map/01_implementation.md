!SLIDE
# New Plan
!SLIDE
# Guts

A digest function:

    @@@ Ruby
    index_of('appetizer') # => 2

A sparse array:

    @@@ Ruby
    [nil, nil, 'fruit salad', nil...]

~~~SECTION:notes~~~
"Digest" like food: one-way process. Can't turn poop back into [same] pizza.
~~~ENDSECTION~~~

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

(But this re-introduces our scaling problem...)

~~~SECTION:notes~~~
For a nice interface, use TupleMap!

At collisions, O(N) again... we could use a binary tree for O(log N) disambiguation, but instead we'll put a hard limit on number of collisions and call it O(1)
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
# Grand Solution?

- Minimize collisions (which hurt speed)
- Minimize wasted memory

!SLIDE
# Grand Solution: Grow As Needed

![Tradeoff](dial.jpg)

!SLIDE
# "As Needed"

## When any key reaches 10 collisions

!SLIDE
# "Grow"

            Old          New

            |--          |-
            |--          |
            |---         |-
            |-           |-
                         |--
                         |
                         |-
                         |-
                         |
                         |-

~~~SECTION:notes~~~
1. Start small
2. Notice when sparse array gets crowded
3. Redistribute value into bigger sparse array
4. Hope for fewer collisions
~~~ENDSECTION~~~

!SLIDE
# Hashing strategy

1. Decide on a starting size
2. Compute a key's raw digest value (??)
3. `index = raw_digest_value % array_size`

!SLIDE
# Modulo

Sparse array size 3.

    Raw digest value | index
    ------------------------
    3                | 0
    7                | 1
    10               | 1
    309406           | 1
    -9384292039948   | 2

!SLIDE
# Growth & Redigesting

    Raw digest value |% 3|% 7 
    ------------------------------
    3                | 0 | 3
    7                | 1 | 0
    10               | 1 | 3
    309406           | 1 | 6
    -9384292039947   | 2 | 1

!SLIDE
# Growth Strategy

## Double size?

!SLIDE
# Solution: Ensure size is prime

## Double size, then find the next prime

(Eg: 199, 401, 809, 1619, 3251, 6521...)

!SLIDE
# Proof

    @@@ Ruby
    # Raw digest values
    r = (10_000...20_000)

    # Re-collisions with non-prime sizes
    r.select {|i| i % 200 == i % 400 }.count
    #=> 5000

    # Re-collisions with prime sizes
    r.select {|i| i % 199 == i % 401 }.count
    #=> 0

!SLIDE
# Whence the Raw Digest Value?

Remember:

1. Decide on a starting size
2. Compute a key's raw digest value (??)
3. `index = raw_digest_value % array_size`

Eh?

!SLIDE
# Try 1: Roll our own

    @@@ Ruby
    def raw_digest(key)
      # Convert to Unicode code points
      key.unpack('U*').join('').to_i
    end

    raw_digest('woah')   #=> 11911197104
    raw_digest('yarrr!') #=> 1219711411411433

!SLIDE
# Whoops

    @@@ Ruby
    raw_digest(:thingy)  #=> NoMethodError
    raw_digest([1,2,3])  #=> NoMethodError

!SLIDE
# Try 2: Object IDs
    @@@ Ruby
    def raw_digest(key)
      # Everything is an object!
      key.object_id
    end

!SLIDE
# Success?
    @@@ Ruby
    raw_digest(:thingy)  #=> 522568
    raw_digest([1,2,3])  #=> 70121947506720

!SLIDE
# Success!
    @@@ Ruby
    key    = [1,2,3]
    h      = HashMap.new
    h[key] = 'stuff'
    h[key] #=> 'stuff'

!SLIDE
# Wait...
    
    @@@ Ruby
    h = HashMap.new
    h[[1,2,3]] = 'stuff'
    h[[1,2,3]] #=> nil
    h['oh']    = 'noes'
    h['oh']    #=> nil

!SLIDE
# Support "same value" keys
    @@@ Ruby
    'foo'         == 'foo'
    [1,2,3]       == [1,2,3]
    {hi: 'there'} == {hi: 'there'}

!SLIDE
# General idea
    @@@ Ruby
    # Assuming obj1.eql?(obj2)
    hash[obj1]   = 'val'
    hash[obj2] #=> 'val'

!SLIDE
# How?

- Can't predict what keys we'll get
- "Same value" is subjective

!SLIDE
# Cheater Pantses

Ruby uses the `#hash` method for this.

!SLIDE
# .hash

    @@@ Ruby
    # "Same val"  => same #hash
    [1,2,3].hash #=> -262151465130803218
    [1,2,3].hash #=> -262151465130803218

!SLIDE
# "Same value" is negotiable

    @@@ Ruby
    class Critter < Struct.new(
      :family, :genus, :species
     )
      def eql?(other)
        # Ignore species
        same_family_and_genus?
      end
      # Reflects our idea of equality
      def hash
        "#{family.hash}#{genus.hash}".to_i
      end
    end

!SLIDE
# See?
    
    @@@ Ruby
    opossum1 = Critter.new('A', 'B', 'C')
    opossum2 = Critter.new('A', 'B', 'X')

    hash[opossum1]    = 'tricky'
    hash[opossum2]  #=> 'tricky'

!SLIDE
# (Bonus Tip)

`.hash` is also used by `Set` to determine uniqueness

!SLIDE
# OK - It works!

![Passing Tests](passing_tests.png)

!SLIDE
# Review
- key => number using `.hash`
- number = index in sparse array
- Growth = fewer collisions = faster
- Growth = more memory used
