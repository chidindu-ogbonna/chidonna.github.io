# Basic 
> this is * home * slanting
> this ** home ** bold


# Includes
```
{% include footer.html %}
```
to include snippets of code in your blog 
all include files must be in the include directory

# Linking
linking to a site in your blog
```
{{ site.baseurl }}{% link _collection/name-of-document.md %}
```

or even using this for markdown

```
[Link to a document]({{ site.baseurl }}{% link _collection/name-of-document.md %})
```

# Linking to post
```
{{ site.baseurl }}{% post_url /subdir/2010-07-21-name-of-post %}
```
the "subdir" is only if you use subdir in your files

# Gist from github
the filename is optional
```
{% gist parkr/931c1c8d465a04042403 jekyll-private-gist.markdown %}
```

# Horizontal rule
---

# Adding images with caption
![Markdowm Image][/image/url]
<figcaption class="caption">Photo by John Doe</figcaption>

# Bigger images 
![Markdowm Image][/image/url]{: class="bigger-image" }

# adding videos or your presentation
<iframe width="560" height="310" src="https://www.youtube.com/embed/r7XhWUDj-Ts" frameborder="0" allowfullscreen></iframe>

<iframe src="//www.slideshare.net/slideshow/embed_code/key/hqDhSJoWkrHe7l" width="560" height="310" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe>

# adding code
{% highlight js %}
{% endhighlight %}

# Codepen
<p data-height="268" data-theme-id="0" data-slug-hash="gfdDu" data-default-tab="result" data-user="chriscoyier" class='codepen'>
    See the Pen <a href='http://codepen.io/chriscoyier/pen/gfdDu/'>Crappy Recreation of the Book Cover of *The Flame Alphabet*</a> by Chris Coyier (<a href='http://codepen.io/chriscoyier'>@chriscoyier</a>) on <a href='http://codepen.io'>CodePen</a>.
</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

# spoiler a content that appers on hover
<div class="spoiler"><p>your content</p></div>

# special ruler
<div class="breaker"></div>

# evidence to a file by adding this tag
star: true
