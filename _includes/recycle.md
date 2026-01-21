### RECYCLE Code
{% assign simplename = page.title | replace:' ','' %}
{% assign filename = "games/" | append: simplename | append: page.num_players | append: ".rcy" %}

The rules for {{ page.title }} are coded in <a href="https://cardstock.readthedocs.io/en/latest/recycle/index.html">RECYCLE</a>, a card game description language, to encourage standardized implementations across different systems. 

You can also <a href="{{site.baseurl}}/assets/{{filename}}" download>Download</a> the code.

{% highlight racket %}
{% include {{filename}} %}
{% endhighlight %}