### Search stats

One of the things that is awesome with chef is the search. You want to use it at many places but it's not costless. And using it too much can harm your perfomances. This is a nice way to measure and submit the data to a statsd dameon, that you can plug onto your favorite graphing tool in the end.

Here we monkey patch the search function to add measurement and then submit it through a TCP socket. Beware; this is only to be used in fork mode under this shape because it will exhaust file descriptors (unless you fix it); I could not get to know why, feedback appreciated :)

```ruby
class Chef
  class Search
    class Query

      alias :original_search :search

      require "socket"

      def search(type, query="*:*", sort='X_CHEF_id_CHEF_X asc', start=0, rows=1000, &block)
        s = UDPSocket.new
        t_start = Time.now.to_f

        rslt = Hash.new
        rslt = self.original_search(type, query, sort, start, rows, &block)
        t_end = Time.now.to_f

        delta = ((t_end - t_start) * 1000).to_i
        s.send("#{type}.chef_search_query.timer:#{delta}|ms", 0, "10.231.40.15", 8125)
        s.send("#{type}.chef_search:1|c", 0, "10.231.40.15", 8125)

        return rslt
      end

    end
  end
end
```
