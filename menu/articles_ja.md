---
layout: page
title: Articles(ja)
---
<script type="text/javascript">
Hatena.Star.SiteConfig = {
  entryNodes: {
    'li.post': {
      uri: 'li.post a',
      title: 'span.post-title',
      container: 'p.post-date'
    }
  }
};
</script>

<ul class="posts">
  {% for post in site.categories.ja %}

    {% unless post.next %}
      <h3>{{ post.date | date: '%Y' }}</h3>
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        <h3>{{ post.date | date: '%Y' }}</h3>
      {% endif %}
    {% endunless %}

    <li itemscope class="post">
      <a href="{{ site.github.url }}{{ post.url }}"><span class="post-title">{{ post.title }}</span></a>
      <p class="post-date"><span><i class="fa fa-calendar" aria-hidden="true"></i> {{ post.date | date: "%B %-d %Y " }}</span></p>
    </li>

  {% endfor %}
</ul>
