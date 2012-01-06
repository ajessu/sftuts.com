---
layout: post
title: "Using Assetic in Symfony2 for CSS compression"
---

Symfony Standard comes bundled with a great library called
[Assetic](https://github.com/kriswallsmith/assetic)
for Assets Management in PHP 5.3 (CSS, js, and even image optimization
coming soon) developed by [Kris Wallsmith](http://twitter.com/kriswallsmith).

We will be using it to compress our CSS files, thus reducing the time required
to download stylesheets in our Symfony2 projects.
<!--more-->
Note: I will be using twig templates for all examples.

## 1. Install the YUI Compressor

Download the [YUI Compressor](http://yuilibrary.com/downloads/#yuicompressor),
get the jar from the `build` directory and place it either: somewhere in your
system, to share it between different projects, or you can place it inside
the app/java directory of your application, to use it on a project by project
basis.

You can also install the package if you use a linux distribution that provides
it. Ex:

    sudo apt-get install yui-compressor

## 2. Change the way you call your assets

The first thing you'll do in your Symfony project is change the way you call
your assets in your templates.

You can also place the CSS files inside the `Resources/css` directory in your
bundles (this is not mandatory, just personal preference).

Before:

{% highlight html %}
{% raw %}
<link rel="stylesheet" href="{{ asset('css/mystyle1.css') }}" type="text/css" media="screen" />
<link rel="stylesheet" href="{{ asset('css/mystyle2.css') }}" type="text/css" media="screen" />
{% endraw %}
{% endhighlight %}

With Assetic:

{% highlight jinja %}
{% raw %}
{% stylesheets output='css/compressed.css' filter='yui_css'
                '@AcmeFooBundle/Resources/css/mystyle1.css' 
                '@AcmeFooBundle/Resources/css/mystyle2.css' %}
                {# If your files are in web/css/, call them with: 'css/mystyle.css' #}
    <link rel="stylesheet" href="{{ asset_url }}" type="text/css" media="screen" />
{% endstylesheets %}
{% endraw %}
{% endhighlight %}

## 3. Edit your configuration file

Open your production configuration file (`app/config/config.yml`), and let's
tweak our assetic settings

{% highlight yaml %}
# app/config/config.yml

# Assetic Configuration
assetic:
    debug:          "%kernel.debug%"
    use_controller: false
    # Uncomment the following line if your java bin is
    # in a different location
    # java: /usr/bin/java
    filters:
        cssrewrite: ~
        yui_css:
            jar: /usr/share/yui-compressor/yui-compressor.jar
            # Or use the location of your downloaded yuicompressor-2.4.6.jar
            # jar: %kernel.root_dir%/java/yuicompressor-2.4.6.jar
{% endhighlight %}

`%kernel.root_dir%` references the root directory of your Application Kernel
(usually: the app/ directory)

That's all! you should see your CSS compressed when you load to your
application in dev environment: `http://localhost/myapp/app_dev.php`

## 4. Dump your assets for production

Now that everything is setup in dev, we will be dumping our assets with the
symfony console, so that all CSS files are compressed and unified into one
file for our production environment.

    app/console --env=prod assetic:dump

Note: I learned over the weekend, thanks to
[Miha Vrhovnik](https://github.com/mvrhov),
that if you run `cache:clear` on your production cache, it warms up the cache
with debug mode on. If you try to dump assets afterwards, weird things
might happen.

He has submitted a PR (waiting to be merged), to avoid this issue.
To avoid it for now. before dumping assets in prod, clear the cache using the
option `--without-debug`. Keep this in mind, if the `assetic:dump` command is not
working for you as expected.

So we would run the following commands:

    app/console --env=prod cache:clear --without-debug --no-debug
    app/console --env=prod assetic:dump

Assetic should now be working in your dev and your prod environment.
If you have any questions, or problems. Hit the comments!

**Tested with Symfony Standard PR11**