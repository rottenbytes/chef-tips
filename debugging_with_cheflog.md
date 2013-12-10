### Debugging with Chef::Log

It's sometime quite hard to figure out what is going on. You may have to deal with data that has an unexcepted formatting, empty stuff from a search, etc… An easy way to debug this is to print it on the screen, but the “puts” method won't work, attaching a debugger is not an simple way to do it neither.

You can use some chef basic functions to achieve this :

```ruby
search(:node, "platform:debian").each do |r|
  Chef::Log.info("#" * 80)
  Chef::Log.info("Just met #{r['name']}")
  Chef::Log.info("#" * 80)
end
```

With chef 11, this won't work out of the box anymore because of the formatters, just add –force-logger as a switch when running your chef-client command.
