# Our Big O Problem

    @@@ Ruby
    [['hi', 'hola'], ['cat', 'gato']...]

!SLIDE
![This grows badly](tuple_map_growth.png)

~~~SECTION:notes~~~
Explain Carefully! Size of hash is X.

Random reads and insertions. Insert is N, read averages 1/2 N.

Looks linear (not proof).
~~~ENDSECTION~~~

!SLIDE
# What can we use that's O(1)?

!SLIDE

# Array lookup by index: O(1)

    @@@ Ruby

    # Equal lookup times
    some_array[328]
    some_array[5]

~~~SECTION:notes~~~
TADA!
~~~ENDSECTION~~~

!SLIDE
# Um... how does THAT work?

!SLIDE
# Spinning Disk vs RAM

!SLIDE
# Spinning Disk

![Spinning Disk](spinning_disk.gif)

<span class="attribution">Adapted from http://www.read.cs.ucla.edu/111/2006fall/notes/lec15</span>

~~~SECTION:notes~~~
This is why we defragged spinning disks - seek times.
(Not with solid state?)
~~~ENDSECTION~~~

!SLIDE
# RAM!

![RAM](ram.png)

~~~SECTION:notes~~~
RAM is different. All locations equal.

Actually probably not, because physics. But at this speed and distance, tiny difference.

More importantly, the computer has a clock whose job is to keep timing synchronized, so it will **ensure** that these lookups take the same amount of time.

~~~ENDSECTION~~~

!SLIDE
# Arrays and RAM

Each array item is as big as a pointer.

    @@@ Ruby
    some_array[328]
    # item location is:
    # array_start_location +
    #  (index x pointer_size)

~~~SECTION:notes~~~
Calculate RAM address, bada bing.
~~~ENDSECTION~~~
