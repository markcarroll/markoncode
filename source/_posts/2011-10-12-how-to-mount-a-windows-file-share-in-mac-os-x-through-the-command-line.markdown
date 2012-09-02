---
layout: post
title: "How to mount a Windows file share in Mac OS X through the command line"
date: 2011-10-12 01:32
comments: true
categories: macosx windows
description: "I switched from Windows to a Mac a year and a half ago, but every now and them I want to do something that just seemed so much easier in Windows.  In this case, mount a Windows file share so I can access it from the command line (or script it).  Turns out it is pretty easy too if you know the special sauce."
keywords: "macoxs windows, mount drive macosx, mac osx windows drive, mac osx windows server"
---

I switched from Windows to a Mac a year and a half ago, but every now and them I want to do something that just seemed so much easier in Windows.  In this case, mount a Windows file share so I can access it from the command line (or script it).  Turns out it is pretty easy too if you know the special sauce.  

	$ mkdir /Volumes/sharename
	$ mount -t smbfs -o nosuid,nodev //username@servername/sharename /Volumes/sharename