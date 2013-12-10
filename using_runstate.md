### Using run_state

There are times you need to ship some informations from a recipe to another. This can be achieved using attributes, but it's quite an ugly way to do when you don't need to store that data over time. Moreover, it makes your nodes fat and it's not good for their health.

A classy way to do so is to use node.run_state which is a hash that is only present during a run and then vanishes. You can use it to do creative stuff like setting sysctls in different recipes, to push them in a template at the end of the run or you can create a definition that registers collectd checks for apps in different recipes and then creates a nice collectd.conf file.

This is a simple example

```ruby
# cookbook1/recipes/default.rb
node.run_state[:stuff] = "meh"
```

```ruby
# cookbook2/recipes/default.rb

x = node.run_state[:stuff]
Chef::Log.info("stuff that was given to us : #{x}")
```
