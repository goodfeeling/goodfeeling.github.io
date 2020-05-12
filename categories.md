---
layout: default
---

{% assign sort_categories = site.categories | sort %}

{% for category in sort_categories %}
  {% assign category_name = category | first %}
  {% assign posts_of_category = category | last %}
  {% assign first_post = posts_of_category[0] %}

  {% if category_name == first_post.categories[0] %}
    {% assign sub_categories = "" %}
    {% for post in posts_of_category %}
      {% if post.categories.size > 1 %}
        {% assign sub_categories = sub_categories | append: post.categories[1] | append: "|" %}
      {% endif %}
    {% endfor %}

    {% assign sub_categories = sub_categories | split: "|" | uniq %}
    {% assign sub_categories_size = sub_categories | size %}

<div class="panel-group">
  <div class="panel panel-default">
    <div class="panel-heading" id="{{ category_name }}">
      <i class="far fa-folder"></i>&nbsp;
      <a class="panel-title" href="/category/{{ category_name }}.html">{{ category_name }}</a>&nbsp;
      {% assign top_posts_size = site.categories[category_name] | size %}
      <span class="text-muted small">
        {{ sub_categories_size }} folder{% if sub_categories_size > 1 %}s{% endif %}, {{ top_posts_size }} post{% if top_posts_size > 1 %}s{% endif %}
      </span>

      <a data-toggle="collapse" href="#grp_{{ category_name }}" class=" {% if sub_categories_size <= 0%}inactiveLink{% endif %}">
        <i class="fas fa-angle-down" style="float: right; padding-top: 0.5rem;"></i>
      </a>
    </div>

    {% if sub_categories_size > 0 %}
    <div id="grp_{{ category_name }}" class="panel-collapse collapse">
      <ul class="list-group">
        {% for sub_category in sub_categories %}
        <li class="list-group-item" style="padding-left: 4rem;">
          <i class="far fa-folder"></i>&nbsp;<a href="/category/{{ sub_category }}.html">{{ sub_category }}</a>
          {% assign posts_size = site.categories[sub_category] | size %}
          <span class="text-muted small">&nbsp;{{ posts_size }}</span>
        </li>
        {% endfor %}
      </ul>
    </div>
    {% endif %}
  </div>
</div>
  {% endif %}
{% endfor %}