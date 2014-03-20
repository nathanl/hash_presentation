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
    index_of('mammal'  )  # => 2 - D'oh!

!SLIDE
# Solution: Buckets
    @@@ Ruby
    [
      nil,
      nil,
      # Tuples again!
      [
        ['appetizer', 'fruit salad']
        ['mammal',    'water buffalo']
      ],
      nil...
    ]

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
      (a bucket),
      nil,       # waste
      nil...     # waste
    ]

!SLIDE
# Solution: Get Over It!

Mostly.

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

## When any bucket has 10 keys

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
# "Grow"

Must rehash every key

!SLIDE
# Strategy

1. Start with X buckets
2. Compute a key's raw digest value (??)
3. `bucket_number = raw_digest_value % x`

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

## Double size, then find the next prime

(Eg: 199, 401, 809, 1619, 3251, 6521...)

!SLIDE
# Whence the Raw Digest?

<ol>
<li class="unfocused">Start with X buckets</li>
<li>Compute a key's raw digest value (??)</li>
<li class="unfocused">`bucket_number = raw_digest_value % x`</li>
</ol>

!SLIDE
# Raw Digest

    @@@ Ruby
    def raw_digest(key)
      # ??
    end

!SLIDE
# Raw Digest Requirements

## **Any object** to a number

    @@@ Ruby
    hash['a']
    hash[['a', 'e']]
    hash[Person.first]

!SLIDE
# Raw Digest Requirements

## "Same value" keys interchangable

    @@@ Ruby
    string1 = 'a'
    string2 = 'a'

    hash[string1]   = 'apple'
    hash[string2] #=> 'apple'

!SLIDE
# How?

"Same value" is subjective

    @@@ Ruby
    a, b = PersonCollection.take(2)
    hash[a]  = 'something'
    hash[b] #=> ??

!SLIDE
# Ask the object

    @@@ Ruby
    'hi'.hash       #=> 1426215061227324254
    ['a', 'b'].hash #=> -1590378560689017266 
    Object.new.hash #=> 56268189660135834

!SLIDE
# `.hash` and `.eql?` must agree

    @@@ Ruby
    [1,2,3].hash #=> -262151465130803218
    [1,2,3].hash #=> -262151465130803218

!SLIDE
# Review
- key to number using `.hash`...
- ...Modulo number of available buckets...
- ...is index of bucket to use

!SLIDE
# Review

- Growth = fewer collisions = faster
- Growth = more memory used
