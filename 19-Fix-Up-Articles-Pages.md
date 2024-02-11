News App / 19. Bootstrap Cards
========================================================

* Update article_list.html.

```python
<!-- templates/articles/article_list.html -->
{% extends '_base.html' %}
{% block title %}Articles{% endblock title %}
{% block content %}

{% for article in article_list %}
  <div class="card">
    <div class="card-header"> 
      <span class="font-weight-bold">{{ article.title }}</span>&middot;
      <span class="text-muted">by {{ article.author }} | {{ article.date }}</span>
    </div>
    <div class="card-body">
{{ article.body }}
    </div>
    <div class="card-footer text-center text muted">
      <a href="#">Edit</a> | <a href="#">Delete</a>
    </div>
  </div>
  <br />    
{% endfor %}
{% endblock content %}
```

* Update 

