---
permalink: /
title: "Welcome! I'm Tom Widdowson"
excerpt: "About me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

---
![Horsehead nebula](/images/nebula.jpg){: .align-right width="300px"}

🪐 I am a third year university student studying toward an MPhys (Astrophysics with space science). 

🌥️ I am an active member of a student led weather and climate club, particpating in coding workshops and helping to maintain a github account

📊 I am interested in expanding my data science skills, putting them to use in tackling climate change

🧑‍🔬 I have experience writing worked physics exam solutions using LaTeX. This has helped hundreds of students develop problem solving skills in preparation for their final exam.  

---

<br/><br/>

## Featured Project 

<div class="featured-projects">
{% assign featured_projects = site.projects | where: "featured", true %}
{% for project in featured_projects %}
  <div class="project">
    <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
    <p>{{ project.excerpt }}</p>
  </div>
{% endfor %}
</div>

<p><a href="/projects/" class="project-link">See all projects</a></p>







