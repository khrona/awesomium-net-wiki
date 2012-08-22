# Awesomium-Wiki

This is the online wiki for Awesomium, a Web UI Bridge for Native Apps.

Check it out at: <http://khrona.github.com/awesomium-wiki>

## To Create a New Category

Let's make a new category named "My Category" with a path of "/my-category"

1. Create a folder named "my-category" in the root of the repo.
2. Create a file named "index.md" inside this folder
3. Add the following text to this file (modify it as needed):

		---
		layout: page
		title : Category - My Category
		group: Categories
		
		---
		{% include JB/setup %}
		
		<ul>
		  {% assign pages_list = site.pages %}
		  {% assign group = 'My Category' %}
		  {% include JB/pages_list %}
		</ul>
4. Open up _config.yml and add your 'name' and 'path' to the 'groups' list.
5. Start making pages with a group of 'My Category'!

## Other Links

<http://www.awesomium.com>

