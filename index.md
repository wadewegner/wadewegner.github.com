---
layout: page
title: Wade Wegner's Blog
tagline: 
---
{% include setup %}

<style>

.profile
{
	margin-top: 15px;
	width: 240px;
}
.profile p
{
	font-size: 13px;
}
.profile .top
{
	height: 140px;
}
.profile .photo
{
	float: left;
	margin-right: 15px;
}



.social-rss,.social-facebook,.social-linkedin,.social-github,.social-twitter,.social-stackoverflow{display:inline-block;float:left;margin:5px;height:32px;width:32px;background-size:32px 32px}
.social-rss{background-color:#ff8300;background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAABs0lEQVR42u3XO0hCURzHcbXiDmWWjxB6rIVESy5RBE5SQ5N7tNbmFmFDY0FzGA3tQUgJLREhCEE1uEQPbIjoQWqJVBBi3yX4c7gKV6/a4A8+yzlH+HHPPUe1lEqlpmoVaBUoN2GDE3ZYm1HAiVs8IoUYVhFAZyMKuFGAXtLYgK/eBb5QKd/YwlA9Cjhwijt8oFJesVCvU6DBiyksI4ly2YLWiGM4iT3o5RBdZhTowDhG4CizZhY3UBOHVmsBD/Io4glHWIRXZ10MaqJmnII81LwgotwF7diFmvlan0AR5XKBMaXEAWRyGKi2gB1RnOAZeslgWnzGhTRkts04BU6EkICaHEbF2iBkfjBs1jG0IayzNSnlnYhBZtPseyCkUyIi5icg84jOagsMYg5+ZTwMmXf0wQIrziETqKbADDL4yzqsYjvOILMkPrsCmTWjBTRcQ41f2QqZYzE3BZm40QK9KEBNSKzpQVbMvYlr24tPMXeFNqM/yeKQKeh89ych4xNP8EGMZ9Ft9B3oxz6ecImgzpod5AS5RQkxfg9XtcfQAVuFG9MjdMhtFONu2Fr/C1oF/mWBX96rvJ4t0gubAAAAAElFTkSuQmCC)}
.social-facebook{background-color:#3B5998;background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAA0ElEQVRYw2P4//8/w0BihlEHjDqASgZxA7EIEAsAMTs9HeAOxJuB+BEQvwXil0C8E4iZ6OGAlP/YwQ16hIAkEH/B4YCr9HBAKJql24E4EiruTA8HZKI5wJbeuSBjoB2QhuYAa3o4gBmIlwHxESC+h+aAy1BxEF5Kq2zIAsRP/hMGN2kVAiAHPCPCARdoGQU90CA+jWbpHqg4CFfSIxEmoTnAlN65AL0csBt1wKgDRpwDstAcYE9vByQA8XskbElvB3ACsSgSZh3tF4w6YMg6AAASl1o+Ox5KuwAAAABJRU5ErkJggg==)}
.social-twitter{background-color:#39A9E0;background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAABO0lEQVR42u3WsUsCUQDH8c5zMCgc0oaWwyFBhyAIjaaEtmjxH2gQwgQb+x9qamtsCJol0NXbqqlRwcZDHBKFIAzhnt8tkHzXvXsVxPvBh1se734c7957S0KIP2UKmAKmgM7JEtiEo6uAhY1vTlRBF1O8o40iUqhiS6XAOl5wGPDyGr7KBAM04cAOW2AbgnzgZMGYNYywKD4eUEEsbIEMfPGZe+zOjdlDUC5hqy7CJubj4hwlHEOWIRKqizCNMjzpJ5bHw7JqgRzeMIZqHqP8hjG4iJKrqBtRFh2oZl/HTniANsLmGXEdBWpQyZGusyCOa4TJzU8cRgXcISguVjQdRjwBB3X0IEsDSR2noYUSbtGBD1n6OIOl8z5gYwcXeMIrpvAxgYcWTpH+jQtJClnkkcGquZKZAqbAvygwA0L3uA3c62FdAAAAAElFTkSuQmCC)}
.social-github{background-color:#4183C4;background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAB3klEQVR42u3XO0scURjGcXdd75dZtRA0leksLALWKWxEFAQRFdfLKthok6+QztIoNhHxCoqFeCkFERH8BAoqIjhrQtAiETVrDOO/eIrDNM7FC8K+8IM9Z/Y9+5zZHWY2y3GcN5UJkAkQpjkH5ZL7WgEs9GMdNtKSwgaSsF4qQA9sONQfbGFKtvAbjsL0PWeAKCa0+ImClCHLJY5uHOm9k8gOHcD48BkUeHh/PqahEOECdGqhOURQ7SFAFWKYUW930AAluMCZdv4Jt9hGE5oxjBG0ogGbuEEj8nCKnygNEiChHfRq3AivlXSvESTAmnZsBQjQr55S3GDTb4AYUtjVOFuvvdah8YPdwQ/k+Alg4R7zGn/AP/ipOvXOqrfMb4A0FjSuwX/4qXpXgLjfr8DGnsbF+AWvlUaVendxgZjnALKKv8ap+w6vtWGcyTusB7kKurTYoMaV2MdTdYCP6klqLhEkQBFssTRXiAEs4tp1ylcwjLhxCZ4jhWLfAaQdjhaPGvO1uDMCPOCzcTyCZR3rCHszGtNCS8buhuCur8bOFzU3/hx3wwi+acFTtOEL3DWKFuN2PIFo2ACmBGw8VSn0vuQjWRJrOMYlrnCsuQHEX+uhNBcVkpf5X5AJ8G4DPAJQo8gomjglGQAAAABJRU5ErkJggg==)}
.social-stackoverflow{background-color:#F47920;background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAABjklEQVR42u3WTSiDcRzAce9NK2+lsK2FCWeZqHFSCxEtB0mK2sHBYQl5KQ6GVnKzowt5KylFLlJORJRyXzIHJHlPj6/6HZ7Top7/luxfn3r+v6ee//fw9GwJmqbFVDwgHqDqwYlIi1WAFecIRCsgDSW6fRY+cRSNgCQs4w423fwEDzArDRBefK9t3WxRZqXKA8SaHOiVvU/2TSoCktGHYt0sDzd4gg11EjCkIqBGHv4IH0wyb5P5LuxyvaEiwIxuhOSQMzTIvaDM/DjFusp3IBMz0GQtoQrHGJD7qUYH9GIObmTLrEj3Et7CovJTvAlN1jP20I9KeDGo4rfAiSK5Tkc1RnGAD13QJRxwIcOogCSEEZDDK2CBCSkogBuzWIVLYmqNDLjCBJzQZL3jGqfYQhCdKJf7LqMD/MjHMOaxgn1cIIxXLMChKmAaVrSgFR50oAf9GEc9ylQFjKELkdYUClUFTCIbjRHYVQWEsIN2NEfgwYiKgEM84u0HXnAPpyEBIge5v5T67/+WxwP+TsAXa9RqRlUJvAQAAAAASUVORK5CYII=)}
.social-linkedin{background-color:#007FB1;background-image:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAABJUlEQVR42u3WPUuCURiH8QoJAhellyXyBZuCoMYIGpqDbNJa2hT7AEFfoKWoJZoKos2v0BJN0RApuNTeFkhBS8bjNdzD4U9BmM8p8NzwW+7DwYsHDjgURdGfCgEh4F8HjKGCA5Qx4jNgGJdwZ89nQArvEtDyGTCKpgTUvQWYBVzhEXVMD9wrWEUZJWxi0fZJ2M6wM1Wc4wK7KPwm4BbunNg+D51t3EPnFVu9BlzDnSPbZ/EB+aFvp4P5fgZkLEDnBTd4hs5ZPwNmoAEPyNj5FBpy/oREnF9gQ+7W5LyNdJwBS3J3Tc7fMBlnwPIPAiZiegX+A3LoDHRAHp8+Au7gzqntZ6GzIneLkUwvr2AHx45124/jUM5ycndOzveRDP+KQ0AI+EoXgZ24oNRosBkAAAAASUVORK5CYII=)}


</style>

<div>
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
		<span class="span3">

			<div class="profile">
				<div class="top">
					<div class="photo">
						<a href="http://wadewegner.com"><img alt="Wade Wegner" src="/img/WadeWegner_128x128.jpg"></a>
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
			</div>

		</span>
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
