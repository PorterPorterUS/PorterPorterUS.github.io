<b>Hi, I am a graduate student at the department of computer science at the unversity of Rochester</b>
<ul>
  {% for category in site.categories %}
  {% for post in site.posts %}
  {% if category == post.categories %}
    <li>
    <a href="{{ post.url }}">    
   {{post.categories}} -->> {{ post.title }}
        </a>
        - <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_long_string }}</time>
      </li>
   
      <br />
        
    
  {% endfor %}
  {% endfor %}
</ul>
