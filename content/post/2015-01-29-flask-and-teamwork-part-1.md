---
title: Flask and Teamwork – Part 1
author: michael
layout: post
date: 2015-01-29
url: /2015/01/flask-and-teamwork-part-1/
categories:
  - Peanuts
tags:
  - Flask
  - Python
  - Teamwork

---
As much as a reminder to myself as (hopefully) a little help to anyone planning something similar, 
I&#8217;ll start and document my (as of yet, feeble) attempts to get going with [Flask](http://flask.pocoo.org/). 
So far I&#8217;ve been using [Tornado](http://www.tornadoweb.org/en/stable/), and while it is a great framework, I kind of like 
the lighter approach (and the nice extensions) that Flask offer. Also: never hurts to try something new.

The project I chose is a humble one: we are using [Teamwork](http://www.teamwork.com) in the office to organise projects and general to-do lists. It&#8217;s great, but sometimes the web ui is a tad bit limited. Like for example if you plan to add a large number of tasks or tasklists with very specific presets, you are in for a lot of manual labour. Or, you could use the very nice [API](http://developer.teamwork.com) and write your own tools. Which is exactly the plan.

So, first things first, check that Flask and the initial extensions I plan to use are installed:

    pip install Flask
    pip install flask-script
    pip install flask-bootstrap
    pip install flask-moment
    

(if this list somehow feels familiar, there&#8217;s a chance you&#8217;ve also read the excellent [Flask Web Development](http://flaskbook.com/) book, on which much of this is based)

`Flask` is the main framework, `flask-script` offers the Manager object to pass command line options to the web 
server (we&#8217;re going to use the development server for now, since this tool will at best be serving a 
handful of requests every now and then, for a dozen or so users), `flask-bootstrap` incorporates 
Twitter&#8217;s [Bootstrap](http://http://getbootstrap.com/), thus making the front 
end development very easy (also check out [Bootswatch](http://bootswatch.com/) for some nice css style variations) and 
finally `flask-moment` is a helper class for dealing with time zones etc.

For Python I&#8217;m using the 2.7.8 version (64bit) from the [Anaconda](http://continuum.io/downloads) distribution. 
Highly recommended (Flask is actually already in there). It comes with its own package manager called `conda`, 
however many flask extensions have not made it into that repository yet (or are not current). `pip` however works 
fine and finds the correct packages, so I stuck with that.

And with that, all the basics are in place:

_teamflask.py:_

    from flask import Flask, render_template
    from flask.ext.script import Manager
    from flask.ext.bootstrap import Bootstrap
    
    app = Flask(__name__)
    bootstrap = Bootstrap(app)
    manager = Manager(app)
    
    
    @app.route('/')
    def index():
        return render_template('index.html')
    
    if __name__ == '__main__':
        manager.run()
    

As mentioned, the manager offers a number of options to run this, but the most straight forward is just `python teamflask.py runserver`, which will run the server listening on localhost and port 5000. To see all the options just run `python teamflask.py runserver -?` instead.

Before continuing with the templates, let&#8217;s step back for a second and think about folder structure. Static files are by default expected to be in a folder `/static`, so it makes sense to have one of those. Here&#8217;s my structure so far:

    / (.py files)
        static/
            css/
            img/
            js/
        templates/
    

Flask uses the [Jinja2](http://jinja.pocoo.org/docs/dev/) templating engine and it&#8217;s great fun to use (imho). Note that there are many others out there, with similar feature sets. Tornado for example has one built in as well (not sure if it is based on something?). In any case, this gives you the oportunity to build very powerful frontends in a very structured manner. 

Here&#8217;s the very simple _index.html_:

    {% extends "base.html" %}
    {% block content %}
    <div class="container">
        <div class="page-header">
            <h1>This is the index...</h1>
        </div>
    </div>
    {% endblock %}
    

It just extends on a base template containing the entire site&#8217;s skeleton, so for most of the pages following, all we need to fill out is this container block.

_base.html_ extends the Twitter Bootstrap&#8217;s base.html. Again we only need to fill out the stuff we really need to modify:

    {% extends "bootstrap/base.html" %}
    
    {% block head %}
    {{ super() }}
        <link rel="shortcut icon" href="{{ url_for('static', filename = 'favicon.ico') }}" type="image/x-icon">
        <link rel="icon" href="{{ url_for('static', filename = 'favicon.ico') }}" type="image/x-icon">
    {% endblock %}
    
    {% block styles %}
    {{ super() }}
        <link href="{{ url_for('static', filename = 'css/bootstrap.css') }}" rel="stylesheet" media="screen">
    {% endblock %}
    
    {% block title %}
        TeamFlask
    {% endblock %} 
    
    {% block navbar %}
        {% include "navbar.html" %}
    {% endblock %}
    
    {% block content %}
    <div class="container">
        <div class="page-header">
            <h1>Hello, {{ name }}!</h1>
        </div>
    </div>
    {% endblock %}
    

Note two things: first, thanks to the url_for() function we don&#8217;t actually have to know where the static folder is, the correct URL is being generated on the fly for us (and filled in with parameters if required); second, the call to super() makes sure that all the bootstrap stuff is run before mine, so my modified bootstrap.css style sheet (with only a few adjusted entries like `navbar-brand-logo`) will overwrite the basic bootstrap ones her. Also not the include of my navbar as a separate file. Again, I feel it is much easier to have this in its own file.

_navbar.html_:

    <div class="navbar navbar-default" role="navigation">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="/"><img class="navbar-brand-logo" src="{{ url_for('static', filename = 'img/logo.png') }}"></a>
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li><a href="/">Home</a></li>
                    <li><a href="/">Add Tasklists</a></li>
                    <li><a href="/">Add Assets</a></li>
                </ul>
            </div>
        </div>
    </div>
    

By using these bite-sized files it is not only easier for me to edit things now, it is first and foremost much easier for the same me in 5 years, or anyone else who is not familiar with the site, to adjust things based on it being quite obvious what navbar.html will contain and it being just a list of links, basically.

Obviously this is not a very exciting page yet. It&#8217;s showing nothing, really. But it&#8217;s a very simple, very quickly set up base system to build the remaining site on. Part 2 will the look at the Teamwork API and how to use the API key to connect to it.
