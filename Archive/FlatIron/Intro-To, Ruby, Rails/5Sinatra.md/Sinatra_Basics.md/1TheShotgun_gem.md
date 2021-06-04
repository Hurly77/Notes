### ERROR

##### command not found

When you run `shotgun` from your terminal you might get a Shell error that reads something like:

```
$ shotgun-bash: shotgun: command not found
```

That means the Shotgun gem isn't properly installed or available in your PATH. Try fixing it with:

```
$ gem install shotgun
```

If you still get that error, from within your application directory, try running:

```
$ bundle install
```

followed by

```
$ bundle exec shotgun
```

##### bundler error

You might get an error about `bundler` that will tell you to run `bundle install`. It'll look like this:

```
$ shotgunbundler: command not found: shotgunInstall missing gem executables with `bundle install`
```

Just run `bundle install` and then `shotgun` again. If you still get the error, try running via:

```
$ bundle exec shotgun
```

#### port in use

You also might get an error about a port being in use. It'll look like this:

```
$ shotgun== Shotgun/Thin on http://127.0.0.1:9393/Thin web server (v1.6.3 codename Protein Powder)Maximum connections set to 1024Listening on 127.0.0.1:9393, CTRL+C to stop/Users/avi/.rvm/gems/ruby-2.2.2/gems/eventmachine-1.0.8/lib/eventmachine.rb:534:in `start_tcp_server': no acceptor (port is in use or requires root privileges) (RuntimeError)
```

This means you have another Shotgun server running somewhere. Do you have another terminal or shell open running a web application or Shotgun? You need to find that process or tab that is running a web application using Shotgun and shut it down with `CTRL+C`.

You can also tell Shotgun to use a different port with the `-p` flag.

```
$ shotgun -p 4200== Shotgun/Thin on http://127.0.0.1:4200/Thin web server (v1.6.3 codename Protein Powder)Maximum connections set to 1024Listening on 127.0.0.1:4200, CTRL+C to stop
```

You'll notice the server started on port `4200`, which is hopefully unoccupied. You can supply any port number. But it's best to terminate your servers rather than creating hundreds of new ones on different ports.