!SLIDE
# New Plan
!SLIDE
    @@@ Ruby
    my_hash['appetizer'] = 'fruit salad'

<br/>

# Guts

A digest function:

    @@@ Ruby
    index_of('appetizer') # => 2

A sparse array:

    @@@ Ruby
    [nil, nil, 'fruit salad', nil...]

~~~SECTION:notes~~~
"Digest" like food: one-way process
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
