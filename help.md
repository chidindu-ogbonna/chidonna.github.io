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

