---
layout: post
title: "Hiding headers for empty sections in UITableView"
date: 2012-08-31 19:09
comments: true
categories: iOS UITableView Xcode
---

While working on a recent iOS application I found my UITableView showing section headings for sections that did not have any rows in them, i.e. the row-count is zero for that section. I read a lot of different opinions on how to hide the section headings for sections with a zero row-count but in the end it turned out to be quite simple. 

It turned out to be as simple as returning `nil` from the `titleForHeaderInSection` method and of course, `0` in `numberOfRowsInSection`

So implement the UITableViewDataSource protocol as usual:

{% codeblock TableViewController.m %}
	- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
	{
	    // Return the number of sections.
	    return _sections.count;
	}

	- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
	{
	    // Return the number of rows in the section.
	    return _rowsInSection[section].count;
	}

	- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section
	{
	    return _rowsInSection[section].count ? _sections[section] : nil;
	}

	- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
	{
		// configure your cell
	}
{% endcodeblock %}

Hope that helps.

Mark