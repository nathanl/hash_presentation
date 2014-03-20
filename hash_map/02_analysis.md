!SLIDE
# Did we get O(1)?

We want a nice, flat line.

!SLIDE
# LET'S... SEE... THE... GRAPH!!!

~~~SECTION:notes~~~
ARE YOU READY FOR THE GRAPH!!!?
~~~ENDSECTION~~~

!SLIDE
![Operations, zoomed in](operations_zoomed_in.png)

~~~SECTION:notes~~~
APPLAUSE! ... Confusion. Analysis.
~~~ENDSECTION~~~

!SLIDE
# Spikes == Redistribution

!SLIDE
# Hmmm. What about Hash?

!SLIDE
![Native Hash operations](native_hash_operations.png)

!SLIDE
# How big WERE our spikes?

!SLIDE
# Ouch
![HashMap Operations Zoomed Out](operations_zoomed_out.png)

!SLIDE
# But Wait!

Both have linearly-increasing spikes
<!-- HERE show Hash and HashMap side by side, or somehow illustrate that the spikes on both are linear -->

<img src="hash_rehashes_linearly.png" class="sidebyside">
<img src="hashmap_rehashes_linearly.png" class="sidebyside">

!SLIDE
# Zoom again
![HashMap flat between spikes](hashmap_flat_between_spikes.png)

!SLIDE
# Success!

- Reads always <= 10 steps
- Same for writes... unless growth needed

!SLIDE
# It's OK!

- Growth **must** take N steps, but...
- As it gets slower, also gets less frequent
- **Average write speed** stays same
- Amortized analysis: O(1)
- Woo!

!SLIDE
<img src="balloons.jpg" style="height: 800px; width: 900px">

!SLIDE
# Ship it!

![Second email to president](second_email_to_president.png)

!SLIDE
# Optimizations?

- Trade more memory at a time
- Rehash in bg thread?
- Implement in C

~~~SECTION:notes~~~
Not a language issue, an algo issue. C might change Y but not shape.
~~~ENDSECTION~~~

!SLIDE
# Applications

- Hashes are O(1) multi-tool
- Dynamo DBs (Riak, Cassandra) are "distributed hash tables"
