### Delete unreferenced files

Sometimes you need a directory to contain only the files you want, which are managed by Chef, thus deleting all extra files (conf.d directories mostly)

This is a way to properly delete unreferenced files, backing them up in Chef's backup directory:

```ruby
# node.run_state[:php][:extensions] contains managed files list.

unreferenced_file = lambda do |filename|
  file filename do
    action :delete
  end
end

# drop unreferenced extensions config
ruby_block "drop_unreferenced_config" do
  block do
    Dir.glob(File.join(node["php"]["ext_config_dir"], "*")).each do |file|
      unreferenced_file.call(file) unless node.run_state[:php][:extensions].include?(file)
    end
  end
end
```

## Credits

I can't find the page where I originally found that trick.
If you are the original author, please tell me and I'll update that section :)
