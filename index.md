---
layout: page
title: ferbass
tagline: iOS and Mac Developer, technology enthusiast
---
{% include JB/setup %}

<ul class="posts">
{% for post in site.posts  %}
    {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}

    {% if forloop.first %}
    <h2 id="{{ this_year }}-ref">{{this_year}}</h2>
    <ul>
    {% endif %}

    <p>
        <li><span>{{ post.date | date: "%d-%m" }}</span></li><a href="{{ post.url }}">{{ post.title }}</a><br />

	    <ul class="list-tag list-linear">
            {% for tag in post.tags %}
                <li><a href="{{ BASE_PATH }}{{ site.JB.tags_path }}#{{ tag }}-ref">{{ tag }}</a></li>
            {% endfor %}
        </ul>
    </p>

    <p>{{post.description}}</p>


    {% if forloop.last %}
    </ul>
    {% else %}
        {% if this_year != next_year %}
        </ul>
        <h2 id="{{ next_year }}-ref">{{next_year}}</h2>
        <ul>
        {% endif %}
    {% endif %}
{% endfor %}
</ul>


