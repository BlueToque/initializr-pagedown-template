#About

I'm a software developer designing a desktop application that is quite complex, and I was searching for a way to provide documentation for the system. I wanted the documentation to have the following features.

1.  It could be deployed with the software
2.  Works offline
2.  It could be deployed on the web
3.  It is easily editable
4.  It could be transformed into other formats

##The Process

Since the [application I am writing][4] already required a web browser to be built in, I decided that the documentation should probably be deployed as HTML. I decided against straight text because it was necessary for some formatting and image embedding. I also decided against Rich Text, although it was an option, because this is definitely not a portable format.

So I looked for some basic HTML boilerplate code and lo and behold...

##HTML5 Boilerplate & Initializr

A group of very smart people had discovered that they were writing the same HTML code over and over and decided to put together a template full of best practises and best of breed frameworks and fittingly entitled this [HTML5 Boilerplate][5].

Now the default web page offered is quite bare, so I opted for one of the templates based on the boilerplate code, known as [Initializr][6]. This gives the added features of a css framework and some pretty formatting and predefined layout classes. All of the work I did not want to do.

##HTML

So now I'm coding in HTML and for a while that's working quite well. The documentation looks good, but I'm spending a lot of time editing the angle brackets, making sure the links are working, and copying and pasting the same damn code into every web page... building a static web site.

Now there is no way I want a [CMS][12]; that violates the necessity of working offline. It is possible to deploy a full fledged wiki and host it in a .NET applicaiton ([done it][13]), and believe me I thought of going this way for this project as well. And then I hit myself in the head to make that thought go away.

##Markdown

[Markdown][1] Is a simple text format that is designed to be human readable, and easily transformed into HTML and other formats. It has many sinple free packages that can turn it into HTML for web-based viewing, or PDF and other formats if you need it.

Now Markdown is also quite easily abused, and many [hipster programmers][2] use it for *the wrong thing*, mainly because it's quite popular and people are looking for things to hit with [this particular hammer][3]. 

Markdown fits this project because of its portability and transformability, and as you'll see in a little bit, I came up with a pretty neat way of fitting it into a web framework.

##Stamping a web site

I thought about writing the code in markdown and processing it into a static web site as part of the build process. This would solve all of the above problems. I did in fact find a tool to do this ([Pandoc][7]), but in the words of one of my first mentors "why should I learn another language just to write documentation" and I let my lazy instincts guide me away from that time sucking solution.

##Loading Markdown into HTML5 Boilerplate

Worrying away at the problem like a dog on a bone I redefine the goal as "how do I load a Markdown file off the server and display it on the client. It turns out there's a way to do this.

###jQuery

The [jQuery get][8] method loads data from the server using an HTTP GET request. This works without a security problem as long as the URL has the same root as the current web page.

###Pagedown

The [Pagedown][9] library is developed by the coders at [StackOverflow][10], and comes with a pure JavaScript implementation that will take a string and output valid HTML

##The Result

The following snippet of code forms the core of the technique

	// get the markdown index, process it and display the html
	// in the markdown-index div
	$.get("index.md", function (data) {
		var converter = new Markdown.Converter();
		$('#markdown').html(converter.makeHtml(data));
	});

As the comment states, this code gets the markdown file from the server, and displays it on the client.

##Deploying a static web site

It turns out that around this time Google announced that Google Drive had [added the ability to host a static web site][11] composed of just HTML, css and JavaScript. The parameters of my solution for a documentation system fit this profile exactly.

#Summary

In summary; I've put together a template using Initializr that demonstrates loading Markdown files from the server and displaying them within the framework. I've also demonstrated hosting this on Google Drive.

The result is something that functions very much like a wiki with raw Markdown files that can be edited with a text editor, providing a simple web framework that's entirely client-side.

[1]: http://daringfireball.net/projects/markdown/
[2]: http://instacod.es/58024
[3]: http://en.wikipedia.org/wiki/Law_of_the_instrument
[4]: http://www.TrueNorthGeospatial.com
[5]: http://html5boilerplate.com/
[6]: http://www.initializr.com/
[7]: http://johnmacfarlane.net/pandoc/
[8]: http://api.jquery.com/jQuery.get/
[9]: http://code.google.com/p/pagedown/
[10]: http://www.StackOverflow.com
[11]: http://googleappsdeveloper.blogspot.ca/2012/11/announcing-google-drive-site-publishing.html
[12]: http://en.wikipedia.org/wiki/Content_management_system
[13]: https://bitbucket.org/bluetoque/bluetoque-pub/src/dbd8af42998b93c2865e56fa6657b6dab0b2f788/DesktopWiki?at=default