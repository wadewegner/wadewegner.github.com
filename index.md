---
layout: page
title: Wade Wegner's Blog
tagline: 
---
{% include setup %}

<div>

	{% for post in site.posts limit: 6 %}

	<div class="page-header">
		<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
	</div>

	<div class="row-fluid post-full">
		<div class="span12">
			<div class="date">
			<span>{{ post.date | date_to_long_string }}</span>
			</div>
			<div class="content">
			{{ post.content }}
			</div>
		</div>
	</div>

	<hr style="
				border-width: 0px; 
				border-top: 1px solid #BDBDBD; 
				margin-top: 30px;
				margin-bottom: 40px; 
				width: 650px;
				margin-right: 20px;
				text-align: left; ">
	{% endfor %}
</div>
