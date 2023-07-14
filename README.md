# inotify

Inotify bindings for [Crystal language](https://github.com/crystal-lang/crystal).

## Installation

Add this to your application's `shard.yml`:

```yaml
dependencies:
  inotify:
    github: xtokio/inotify
    version: 1.0.1
```

## Usage

```crystal
require "inotify"

module WatchFiles
  VERSION = "0.1.0"

  # To watch a file or directory ...
  puts "Monitor starts..."
  recursive_monitor = true
  watcher = Inotify.watch "/home/xtokio/monitor", recursive_monitor do |event|
    # your logic
    puts event.type
    puts event.name

    # rsync files
    stdout = IO::Memory.new
    stderr = IO::Memory.new

    # Server (Connection with server is setup with ssh keys)
    command = "rsync -r /home/xtokio/monitor/ root@64.227.6.48:/root/backup/"
    exit_code = Process.run(command, shell:true, output:stdout, error:stderr)

    # Local
    command = "rsync -r /home/xtokio/monitor/ /home/xtokio/backup/"
    exit_code = Process.run(command, shell:true, output:stdout, error:stderr)

    puts exit_code
    puts stdout
    puts stderr
  end

  sleep 300
  watcher.close
end
```

_Note: You have to run something in the main fiber or else your program will exit._

## Development

To enable logging to `STDOUT` using environment variables, follow the instructions in the [api docs](https://crystal-lang.org/api/0.34.0/Log.html#configure-logging-from-environment-variables). Use log source `inotify` and severity level `DEBUG`.

## Contributing

1. [Fork it!](https://github.com/xtokio/inotify/fork)
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request

## Contributors

- [petoem](https://github.com/petoem) Michael Petö - creator, maintainer
