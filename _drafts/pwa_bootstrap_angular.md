---
layout: post
title:  "Progressive Web App With Angular 2 And BootstrapJS"
date:   2016-12-21 00:00:00 +0000
categories: progressive web app angular 2 bootstrap
---
{:.content-main-justify}
Prrogressive web applications (PWAs) are bcoming more popular

{:.content-main-justify}
The instructions that follow assume that you are working on a Debian-based Linux distro such Debian, Ubuntu, Mint, or Elementary (based on Ubuntu 16.04).

<br />
{:.section-header}
Installation

{:.content-main-justify}
To get started let's install "inotify-tools":

{% highlight bash %}
sudo apt install inotify-tools
{% endhighlight %}

<br />

{:.content-main-justify}
Next, we're going to create a script to monitor a folder <code>nano /home/username/monitor.sh</code>:

{% highlight bash %}
#! /bin/bash

folder=<path>
inotifywait -m -q -e create -r --format '%:e %w%f' $folder | while read file
  do
    notify-send "<message>"
  done
{% endhighlight %}

<br />

{:.content-main-justify}
Now this is the tricky part, if our script stops running for whatever reason or to have the script run across reboots, we need to create a service to do so: <code>nano /etc/usystemd/system/monitor.service</code>:

{% highlight bash %}
[Service]
ExecStart=/bin/bash /home/<username>/monitor.sh
Restart=always
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=monitor
User=<username>
Group=<username>

[Install]
WantedBy=multi-user.targe
{% endhighlight %}

<br />

{:.content-main-justify}
Now we start the service:
{% highlight bash %}
sudo systemctl enable monitor
sudo systemctl start monitor
{% endhighlight %}

<br />

{:.content-main-justify}
Here are some of the other commands available to you:
{% highlight bash %}
sudo systemctl stop monitor
sudo systemctl restart monitor
sudo systemctl status monitor
{% endhighlight %}

<br />

{:.content-main-justify}
There exists the possibility that your system may reach its limit of inotify watches:

{% highlight bash %}
Failed to watch /var/log/messages; upper limit on inotify watches reached!
{% endhighlight %}

<br />

To verify your current inotify watches settings:

{% highlight bash %}
cat /proc/sys/fs/inotify/max_user_watches
{% endhighlight %}

<br />

To temporarily set your inotify watches: <code>sudo sysctl fs.inotify.max_user_watches=524288</code>

Permanently set them: <code>524288 | sudo tee -a /proc/sys/fs/inotify/max_user_watches</code>
{% highlight bash %}
cat /proc/sys/fs/inotify/max_user_watches
{% endhighlight %}

<!-- Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %} -->

<!-- Check out the [Jekyll docs][jekyll-docs] for more info on how to get the <code>most</code> out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
