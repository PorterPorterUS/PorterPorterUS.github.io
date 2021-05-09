 
<section>
  {% if site.posts[1] %}

    {% capture firstpostyear %}{{ site.posts[1].categories }}{% endcapture %}
    <h3>{{site.posts[1].categories}}</h3>

    {%for post in site.posts %}
      {% unless post.next %}
        <ul>
      {% else %}
        {% capture year %}{{ post.categories }}{% endcapture %}
        {% capture nyear %}{{ post.next.categories }}{% endcapture %}
        {% if year != nyear %}
          </ul>
          <h3>{{ post.categories }}</h3>
          <ul>
        {% endif %}  
      {% endunless %}
        <li><time>{{ post.date | date:"%d %b" }} - </time>
          <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">
            {{ post.title }}
          </a>
        </li>
    {% endfor %}
    </ul>

  {% endif %}
</section>
