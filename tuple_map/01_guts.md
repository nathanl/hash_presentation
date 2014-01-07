!SLIDE
# No Problem!

!SLIDE
# Guts

    @@@ Ruby
    [['hi', 'hola'], ['cat', 'gato']...]

!SLIDE
# Basic Methods

    @@@ Ruby
    class TupleMap
      # For hash['foo'] (reader)
      def [](key) # For 
        # 1. Walk through tuples
        # 2. Find matching key
        # 3. Return value
      end

      # For hash['foo'] = 'bar' (writer)
      def []=(key, value)
        # Same, but update value
        # If no match, insert tuple
      end
    end

!SLIDE
# Usage

    @@@ Ruby
    h = TupleMap.new
    h['hokey']   = 'pokey'
    h['hokey'] #=> 'pokey'

!SLIDE
# Passing Tests
![Passing Tests](passing_tests.png)

