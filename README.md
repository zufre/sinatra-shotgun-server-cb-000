# Using the Shotgun Development Server

## Overview

This lesson will introduce you to shotgun and how to use it with Sinatra apps. We'll also cover troubleshooting common problems that you might encounter when running `shotgun`. 

## Objectives

1. Explain how using `rackup` to start a Sinatra application will only read the code once at boot 
2. Describe how `shotgun` allows for automatic code reloading
3. Install shotgun and require it in an application's `Gemfile` 
4. Start and stop a rack or Sinatra application with shotgun
5. Troubleshoot common `shotgun` errors including "command not found, "bundler error", and "port in use."

## Setup

Make sure you run `bundle install` to install the gems in our `Gemfile`. If your operating system is OSX El Capitan, and you have an issue installing `EventMachine`, first check to make sure Open SSL is installed by entering `brew install openssl` in terminal. Once it's installed, enter `brew link openssl --force`.

## Why shotgun

Normally when we develop simple Rack applications like Sinatra applications, we start our application server with `rackup` and load `config.ru`.

For the purpose of this lesson, fork and clone this repo. There are, however, no tests to pass. In your terminal, go ahead and enter `rackup app.rb`. This will start up a rack server. You should see something like this:

<img src="https://s3.amazonaws.com/learn-verified/rackup.png">

Pay attention to the line that says `Listening on localhost:9292`. `localhost:9292` is the url you want to enter in the browser. Go ahead and copy and paste that into the browser. You should see `Welcome to your app!!!!` on the screen.

When starting an application with `rackup`, our application code is read once on boot and never again. Once we start the application locally, if we make changes to our code, our running application server will not read those changes until it is stopped and restarted.

Now add some more text to the string in the controller action:

```ruby
  get '/' do 
    "Welcome to your app!!!! I BUILT THIS!"
  end
```
Refresh the page in the browser. You should just see `Welcome to your app!!!!`. That's because rack isn't aware that we made changes. You can shut down your server by going back to terminal and hitting `ctrl` + `c`. 

Start your server back up by entering `rackup app.rb` and now try visiting `localhost:9292` in the browser. It should work and you should see the text `Welcome to your app!!!! I BUILT THIS!` in your browser window.

This tedious save, stop, and restart cycle makes developing Rack or Sinatra applications near impossible. To avoid this, instead of starting our application with `rackup`, we will use `shotgun`.

[Shotgun](https://github.com/rtomayko/shotgun) is a small Ruby gem that makes it easier to develop Rack-based Ruby web applications locally by starting Rack with automatic code reloading. A gem is just a library of code that developers wrote and made free and available to the public. This gem lets us start Rack to have a development server running to test our app.

When you start an application with `shotgun`, all of your application code will be reloaded upon every request. That means if you change anything in your code and save it, when you hit 'Refresh' in your browser, your application will respond with the latest version of your code.

## Installing shotgun

You can install shotgun via `gem install shotgun`. You should also require it in an application's `Gemfile` so that you can install it and load it via Bundler. Shotgun is already in your gemfile, so go ahead and enter `bundle install` in terminal if you haven't already.

## Starting and Stopping shotgun

Within a rack or Sinatra application directory, you can start the application via shotgun by simply executing `shotgun` in your terminal. You should see something like:

```
$ shotgun
== Shotgun/Thin on http://127.0.0.1:9393/
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on 127.0.0.1:9393, CTRL+C to stop
```

That means your application is loaded and being served from port `9393`, the default shotgun port. The application will respond to requests at `http://127.0.0.1:9393` or more commonly, `localhost:9393` and will reload your code on every request until the process is terminated by typing `CTRL+C`. Go ahead and visit `localhost:9393` in the browser.

A port is just an endpoint on the server that is open for communication. It's typical for a server to regulate the open ports to make sure it can monitor requests appropriately. You'll notice that with rackup you visited the port `9292` but with shotgun `9393`.

A working server responding to a request and then exiting looks like:

![Shotgun Working](https://dl.dropboxusercontent.com/s/0dwm67kbwvbope1/2015-09-15%20at%2011.12%20PM.png)

You can pass to `shotgun` most CLI arguments and flags that `rackup` accepts.

Now, with your shotgun server still running, change the text in the string in the controller action:

```ruby
get '/' do
  "Started my server using shotgun!"
end
```

Save your changes to `app.rb` and visit `localhost:9393` in the browser. Hit 'Refresh' in your browser to make a new request. This time, you should see the text `Started my server using shotgun!` as opposed to seeing the previous text, just like with rackup.

## Troubleshooting Shotgun

### command not found

When you run `shotgun` from your terminal you might get a Shell error that reads something like:

```
$ shotgun
-bash: shotgun: command not found
```

That means the shotgun gem isn't properly installed or available in your PATH. Try fixing it with:

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

### bundler error

You might get an error about `bundler` that will tell you to run `bundle install`. 
It'll look like this:

```
$ shotgun
bundler: command not found: shotgun
Install missing gem executables with `bundle install`
```

Just run `bundle install` and try `shotgun` again. If you still get the error try running via:

```
$ bundle exec shotgun
```

### port in use

You also might get an error about a port being in use. It'll look like this:

```
$ shotgun
== Shotgun/Thin on http://127.0.0.1:9393/
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on 127.0.0.1:9393, CTRL+C to stop
/Users/avi/.rvm/gems/ruby-2.2.2/gems/eventmachine-1.0.8/lib/eventmachine.rb:534:in `start_tcp_server': no acceptor (port is in use or requires root privileges) (RuntimeError)
```

This means you have another shotgun server running somewhere. Do you have another Terminal or Shell open running a web application or shotgun? You need to find that process or tab that is running a web application using shotgun and shut it down with `CTRL+C`.

You can also give `shotgun` a different port to use with the `-p` flag.

```
$ shotgun -p 4200
== Shotgun/Thin on http://127.0.0.1:4200/
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on 127.0.0.1:4200, CTRL+C to stop
```

You'll notice the server started on port `4200` which is hopefully unoccupied. You can supply any port number. But it's best to terminate your servers rather than just start 100s on different ports.


<p class='util--hide'>View <a href='https://learn.co/lessons/sinatra-shotgun-server'>Using the Shotgun Development Server</a> on Learn.co and start learning to code for free.</p>
