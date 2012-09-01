---
layout: post
title: jQueryMobile pages flash when running full screen on iOS5
date: 2011-12-08 04:19
comments: true
categories: 
---

There seems to be a bug in jQuery Mobile 1.0 (final) that causes a white full screen flash every time the page transitions to a new page.

It doesn’t manifest if I run the page in mobile Safari but if I run the page as a full-screen app (i.e. creating a shortcut to the web app using “Add to Home Screen” from Safari) and setting the meta tag to

    <meta name=”apple-mobile-web-app-capable” content=”yes” />

which gets rid of all the browser chrome, then the flashing bug occurs.

Seems there is no fix in the queue yet but a work around has been found.  Simply comment out the call to `pageTitle.Focus()` on line 2326:

{% codeblock lang:javascript %}
//direct focus to the page title, or otherwise first focusable element
function reFocus( page ) {
	var pageTitle = page.find( ".ui-title:eq(0)" );

	if( pageTitle.length ) {
	// MSC comment out next line due to flashing bug on iOS 5  
		// pageTitle.focus();
	}
	else{
		page.focus();
	}
}
{% endcodeblock %}

Thanks to [hhytt](https://github.com/hhytt) for finding this.