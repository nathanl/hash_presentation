# Our Big O Problem

    @@@ Ruby
    [['key1', 'val1'], ['key2', 'val2']...]

!SLIDE
![This grows badly](tuple_map_growth.png)

~~~SECTION:notes~~~
Random reads and insertions. Insert is N, read averages 1/2 N.

Looks linear (not proof).
~~~ENDSECTION~~~

!SLIDE
# What can we use that's O(1)?

!SLIDE

# Array lookup by index.

    @@@ Ruby
    some_array[328]
    # is just as fast as
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

(Adapted from <br/>http://www.read.cs.ucla.edu/111/2006fall/notes/lec15)

~~~SECTION:notes~~~
This is why we defragged spinning disks - seek times.
(Not with solid state?)
~~~ENDSECTION~~~

!SLIDE
# RAM!

![RAM](ram.png)

(From HowStuffWorks.com)

~~~SECTION:notes~~~
RAM is different. All locations equal.

Actually probably not, because physics. But at this speed and distance, the clock speed rounds off the tiny differences.
~~~ENDSECTION~~~

!SLIDE
# Arrays and RAM

    @@@ Ruby
    some_array[328]
    # item location is:
    # array_start_location +
    #  (pointer_size x index)

~~~SECTION:notes~~~
Calculate RAM address, bada bing.
~~~ENDSECTION~~~
