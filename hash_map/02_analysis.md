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
# Hmmm. What about Hash?

!SLIDE
![Native Hash operations](native_hash_operations.png)

!SLIDE
# How big WERE those spikes?

!SLIDE
# Ouch
![HashMap Operations Zoomed Out](operations_zoomed_out.png)

!SLIDE
# But Wait!

Both have linear spikes
<!-- HERE show Hash and HashMap side by side, or somehow illustrate that the spikes on both are linear -->

<img src="native_hash_operations.png" class="sidebyside">
<img src="operations_zoomed_out.png" class="sidebyside">

!SLIDE
# Zoom again
![Operations, zoomed in](operations_zoomed_in.png)

!SLIDE
# Success!

- Reads always <= 10 steps
- Same for writes between spikes

!SLIDE
# Seriously!

- Each rehashing unavoidably O(n), BUT...
- Amortized insertion time still O(1) - woo!

!SLIDE
<img src="balloons.jpg" style="height: 800px; width: 900px">

!SLIDE
# Optimizations?

- Trade more memory at a time
- Rehash in bg thread?
- Implement in C

~~~SECTION:notes~~~
Not a language issue, an algo issue. C might change Y but not shape.
~~~ENDSECTION~~~
