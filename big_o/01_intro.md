# BIG O

Computer science for n00bs, by n00bs

!SLIDE
# Rate of Growth

~~~SECTION:notes~~~
Big O is all about rough rates of growth. As the problem grows, how much does the work grow? 
~~~ENDSECTION~~~

!SLIDE
![Linear and non-linear growth rates](non-linear_growth_rates.png)

~~~SECTION:notes~~~
N = X
Y = steps

Big O is really concerned with just one question.
~~~ENDSECTION~~~

!SLIDE
# How's it shaped?

We don't care about angle or location.

!SLIDE
# Linear Growth

![Linear growth rates](linear_growth_rates.png)

~~~SECTION:notes~~~
You care about these differences, but Big O doesn't. 

All linear. All "straight lines with some slope." Work proportional to N.
~~~ENDSECTION~~~

!SLIDE
# O(1) - Non-growth

![These are all O(1)](one.png)

~~~SECTION:notes~~~
Again, you care, Big O doesn't. "Doesn't grow" is always awesome.

That's what we're aiming for with our hash.
~~~ENDSECTION~~~

!SLIDE
# Linear vs Non-Linear Growth

![Linear and non-linear growth rates](non-linear_growth_rates.png)

~~~SECTION:notes~~~
Any O(N) beats any O(N**2) beats any O(N!) in the long run.

Brute forcing the "Traveling Salesman" grows as O(N!) (N factorial). If 5 cities takes 0.12 seconds, solving for 21 will take 97 billion years.

Can I, practically speaking, keep solving bigger versions of this problem or not? If so,
how painful will it be?
~~~ENDSECTION~~~
