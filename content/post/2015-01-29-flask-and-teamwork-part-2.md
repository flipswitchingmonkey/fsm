---
title: Flask and Teamwork Part 2
author: michael
layout: post
date: 2015-01-29
url: /2015/01/flask-and-teamwork-part-2/
categories:
  - Peanuts
tags:
  - Flask
  - Python
  - Teamwork

---
I wrote a little_helpers.py that will contain a bunch of functions that will make accessing Teamwork easier. The first step is always to authenticate against Teamwork with the user&#8217;s api key and thus, this is the first function making it in there too:

    def validateApiKey(key):
        url = "https://authenticate.teamworkpm.net/authenticate.json"
        auth = HTTPBasicAuth(key, 'pass')
        r = requests.get(url, auth=auth)
        json_data = r.json()
        if "STATUS" in json_data and json_data["STATUS"] == "OK":
            return (r.status_code, json_data["account"])
        else:
            return (r.status_code, None)
    

Nothing fancy. I decided against using `urllib2` and went with the `requests` module instead. The latter offers a much nice interface and it also has a very simple way to deal with return codes without necessarily having to deal with exceptions. It just writes the return code into `r.status_code` as an integer. I have not done the status code checking in here, because depending on the error code there will be a redirection to specific other routes (Error 401 for example with return to the login page with a specific message).

The authentication URL is thankfully generic and does not require to know the user&#8217;s company id, so this means all you really ever need is the api key, with which you can authenticate and get in return the complete user data for that user, including the correct company url for further requests. The interesting stuff is in under `["account"]`, so that&#8217;s all I&#8217;m returning. It is JSON encoded and can thus be plugged straight into the session variable as e.g. `session["current_user"]`. Note that the response does not contain the api key itself, so that has to be stored separately.

With this it is very easy to create further functions like to get all projects:

    def getProjects(key, user):
        baseurl = user["URL"]
        url = baseurl + "projects.json"
        auth = HTTPBasicAuth(key, 'pass')
        r = requests.get(url, auth=auth)
        json_data = r.json()
        if "STATUS" in json_data and json_data["STATUS"] == "OK":
            return (r.status_code, json_data["projects"])
        else:
            return (r.status_code, None)
    

Pretty much the same, except now we can use the user&#8217;s own URL. And this is basically how I plan to implement most of the API, at least for the read-only functions.

As an initial test I displayed all project&#8217;s start and end dates on a timeline, using the [visjs][1] library&#8217;s Timeline object. Works like a charm.

The next step will be to incorporate user&#8217;s holidays and other blocked times into the timeline to easily spot potential conflicts. This kind of reporting is unfortunately still missing from Teamwork (January &#8217;15), but I hear they are working on it. Until then we&#8217;ll build our own. I am still working out the details on _how_ to display all the infos, though&#8230;

Oh and at first I tried to somehow get a counter (integer) going within a Jinja2 loop in the templates and of course that didn&#8217;t work. Turns out it is very easy though:

      var data = new vis.DataSet([
        {% for project in projects %}
            {id: {{ loop.index }}, content: '{{ project.name }}', start: '{{ project.startDate }}', end: '{{ project.endDate }}'},
        {% endfor %}
      ]);
    

`loop.index` does just that.

And finally, another extension

    pip install flask-wtf
    

to create web forms easily. Only used it for the login dialog so far, but it is very straight forward to use.

Next is creating more Teamwork API wrappers and working out a good way to display the kind of reports mentioned above.

 [1]: http://visjs.org/