---
layout: mainpage
title: Wade Wegner's Blog
tagline: 
---
{% include setup %}

<div>
	{% assign first = true %}
	{% for post in site.posts limit: 6 %}

	<div class="page-header">
		<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
	</div>

	<div class="row-fluid post-full">
		<div class="span9">
			<div class="date">
				<span>{{ post.date | date_to_long_string }}</span>
			</div>
			<div class="content">
				{{ post.content }}
			</div>
		</div>
		<div class="span3">

			{% if first == true %}
			<div class="profile">
				<div class="top">
					<div class="photo">
						<a href="http://www.wadewegner.com"><img class="img-rounded" alt="Wade Wegner" src="/img/WadeWegner_128x128.jpg"></a>
					</div>

					<a class="social-rss" href="http://feeds.feedburner.com/WadeWegner"></a>
					<a class="social-facebook" href="http://www.facebook.com/wadewegner"></a>
					<a class="social-twitter" href="http://www.twitter.com/wadewegner"></a>
					<a class="social-github" href="http://www.github.com/wadewegner"></a>
					<a class="social-linkedin" href="http://www.linkedin.com/in/wadewegner"></a>
					<a class="social-stackoverflow" href="http://stackoverflow.com/users/1043146/wade"></a>

				</div>

				<p><span style="font-weight:bold;">Wade Wegner</span> is the CTO at Aditi Technologies and an aspiring brewmaster.</p>
			
				<div>
					<iframe id="twitter-widget-0" scrolling="no" frameborder="0" allowtransparency="true" src="http://platform.twitter.com/widgets/follow_button.1382126667.html#_=1382886215451&amp;id=twitter-widget-0&amp;lang=en&amp;screen_name=wadewegner&amp;show_count=true&amp;show_screen_name=true&amp;size=m" class="twitter-follow-button twitter-follow-button" title="Twitter Follow Button" data-twttr-rendered="true" style="width: 230px; height: 20px;"></iframe>
				</div>

				<div class="top-buffer-20">
					<div class="bottom-buffer-10" style="border-bottom: 1px solid #eeeeee;">
						<strong>Additional Resources</strong>
					</div>
					<a href="/about">About Me</a><br/>
					<a href="/speaking">Speaking Engagements</a><br/>
					<a href="/projects">My Projects</a><br/>
					<a href="/contact">Contact Me</a><br/>
				</div>
			</div>
			{% endif %}

		</div>
	</div>

	{% assign first = false %}
	{% endfor %}
</div>
