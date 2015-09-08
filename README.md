---
tags:
level:
languages:
resources:
---

# Sinatra Shotgun Server

## Objectives

1. Objective 1
2. Objective 2

### Problems with Shotgun

*We can move this to it's own README and link to it. Given where this lesson is, I imagine we've already covered this content but just in case, I wrote it and we can remove it.*

If you see output that tells you that Shotgun/Thin or anything are listening on an IP address like `127.0.0.1` or `0.0.0.0` or localhost, everything worked. It should look something like:

```
// ♥ shotgun
== Shotgun/Thin on http://127.0.0.1:9393/
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on 127.0.0.1:9393, CTRL+C to stop
```

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

That means you have another shotgun server running somewhere. Do you have another Terminal or Shell open running a web application or shotgun? You need to find that process or tab that is running a web application using shotgun and shut it down with `CTRL+C`.
