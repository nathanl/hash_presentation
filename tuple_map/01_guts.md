!SLIDE
# No Problem!

!SLIDE
# Guts

    @@@ Ruby
    [['key1', 'val1'], ['key2', 'val2']...]

!SLIDE
# Basic Methods

    @@@ Ruby
    class TupleMap
      def [](key)
        # 1. Walk through tuples
        # 2. Find matching key
        # 3. Return value
      end

      def []=(key, value)
        # Same, but update value
        # If no match, insert tuple
      end
    end

!SLIDE
# Passing Tests
![Passing Tests](passing_tests.png)

!SLIDE
# Usage

    @@@ Ruby
    h = TupleMap.new
    h['hokey']   = 'pokey'
    h['hokey'] #=> 'pokey'
