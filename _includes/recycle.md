### RECYCLE Code

{% assign filename = "games/" | append: page.title | append: page.num_players | append: ".rcy" %}

<a href="{{site.baseurl}}/assets/{{filename}}" download>Download link</a>

{% highlight racket %}
{% include {{filename}} %}
{% endhighlight %}