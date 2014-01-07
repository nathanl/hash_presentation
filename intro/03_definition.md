# "Hash"

!SLIDE

!SLIDE
# Not this

![Hash, the Drug](hash_drug.jpg)

~~~SECTION:notes~~~
Very funny, Mr. Baron Chandler.
~~~ENDSECTION~~~

(From Wikipedia)

!SLIDE
# Or this

![Hash, the Food](hash_food.jpg)

(From Wikipedia)

!SLIDE
# Or even this

    @@@ Ruby
    Digest::MD5.digest('hi') 
    # => "I\xF6\x8A\\\x84\x93\xEC..."

~~~SECTION:notes~~~
Although it's related, as we'll see later.
~~~ENDSECTION~~~

!SLIDE
# Hash - a data structure

Aka "map" or "dictionary"

    @@@ Ruby
    h = {} # or Hash.new
    h['appetizer'] = 'fruit salad'
    h['appetizer'] #=> 'fruit salad'

~~~SECTION:notes~~~
You may know this as a "map" or a "dictionary".

This is the interface we want.
~~~ENDSECTION~~~

!SLIDE
# Goal: Implement This

    @@@ Ruby
    h = MyHashClass.new
    h['appetizer'] = 'fruit salad'
    h['appetizer'] #=> 'fruit salad'
