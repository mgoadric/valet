### RECYCLE Code
{% assign simplename = page.title | replace:' ','' %}
{% assign filename = "games/" | append: simplename | append: page.num_players | append: ".rcy" %}

<a href="{{site.baseurl}}/assets/{{filename}}" download>Download link</a>

{% highlight racket %}
{% include {{filename}} %}
{% endhighlight %}