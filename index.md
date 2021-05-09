<b>Hi, I am a graduate student at the department of computer science at the unversity of Rochester</b>
### DFS->>> 
<ul>
  {% for post in site.posts %}
    {if post.categories == "DFS"}
        <li>
        <a href="{{ post.url }}">
        
        {{ post.title }}
        </a>
        - <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date_to_long_string }}</time>
      </li>
      <br />
    
  {% endfor %}
</ul>
