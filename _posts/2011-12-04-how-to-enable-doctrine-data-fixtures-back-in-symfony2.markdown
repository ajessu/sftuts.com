---
layout: post
title: "How to enable doctrine data fixtures back in Symfony2"
---

**Update: April 23rd**

This post is incomplete, since DoctrineFixturesBundle was moved to it's own
repository.
For up to date instructions, refer to the Symfony Cookbook entry on
[how to setup and configure doctrine data fixtures](http://symfony.com/doc/2.0/cookbook/doctrine/doctrine_fixtures.html#setup-and-configuration)

---

Doctrine data fixtures loading (aka `doctrine:data:load` command)
was recently removed from the Symfony Standard edition.

The Doctrine data fixtures extension is not stable yet, but it's  still very
much usable. Luckily, enabling the extension back is very easy, we'll show
you two ways of doing it.


**a) Updating your vendors.sh in Symfony Standard Edition**

If you're using the Symfony Standard Edition, add this line to your
`bin/vendors.sh`, right after your doctrine git_install command:

{% highlight bash %}
# bin/vendors.sh
# Doctrine Data Fixtures Extension
install_git doctrine-data-fixtures git://github.com/doctrine/data-fixtures.git
{% endhighlight %}

And run `sh bin/vendors.sh` from your root folder to reload your vendors.

**b. Cloning the doctrine data fixtures extension (alternative)**

You just need to download/clone the library into your vendor folder.

{% highlight bash %}
cd vendor
git clone https://github.com/doctrine/data-fixtures.git doctrine-data-fixtures
{% endhighlight %}

Enable the fixtures
-------------------

Then, just enable the data fixtures namespace in you app/autoload.php file,
before the `Doctrine\\Common` namespace that's already there:

{% highlight php startinline %}
'Doctrine\\Common\\DataFixtures' => __DIR__.'/../vendor/doctrine-data-fixtures/lib',
{% endhighlight %}

That's it, you'll learn how easy it is to use data fixtures in your Symfony2
project on our next post.