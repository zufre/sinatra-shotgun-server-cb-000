
# Troubleshooting Shotgun in Sinatra

## Local Deployment with Shotgun

Shotgun is a command that starts one of Rack's suppoerted servers (in our case, `Thin web server`). It allows us to deploy our application locally so that we can build and test our application.

To use shotgun, merely type `shotgun` in your terminal from inside of a Sinatra project directory. Make sure that your Gemfile contains `gem 'shotgun'`

If you see output that tells you that Shotgun/Thin or anything are listening on an IP address like `127.0.0.1` or `0.0.0.0` or localhost, everything worked! It should look something like:

```
// ♥ shotgun
== Shotgun/Thin on http://127.0.0.1:9393/
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on 127.0.0.1:9393, CTRL+C to stop
```


## Problems with Shotgun

You might get an error about `bundler`, it'll tell you to run `bundle install`, it'll look like this:

```
// ♥ shotgun
bundler: command not found: shotgun
Install missing gem executables with `bundle install`
```

Just run `bundle install` and try `shotgun` again.

You also might get an error about a port being in use. It'll look like this:

```
// ♥ shotgun
== Shotgun/Thin on http://127.0.0.1:9393/
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on 127.0.0.1:9393, CTRL+C to stop
/Users/avi/.rvm/gems/ruby-2.2.2/gems/eventmachine-1.0.8/lib/eventmachine.rb:534:in `start_tcp_server': no acceptor (port is in use or requires root privileges) (RuntimeError)
```

This means you have another shotgun server running somewhere. Do you have another Terminal or Shell open running a web application or shotgun? You need to find that process or tab that is running a web application using shotgun and shut it down with `CTRL+C`.
