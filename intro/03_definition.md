!SLIDE
# Hash - a data structure

Aka "map" or "dictionary"

    @@@ Ruby
    h                = Hash.new
    h['appetizer']   = 'fruit salad'
    h['appetizer'] #=> 'fruit salad'

~~~SECTION:notes~~~
You may know this as a "map" or a "dictionary".

This is the interface we want.
~~~ENDSECTION~~~

!SLIDE
# Goal: Implement This

    @@@ Ruby
    h                = MyHashClass.new
    h['appetizer']   = 'fruit salad'
    h['appetizer'] #=> 'fruit salad'
