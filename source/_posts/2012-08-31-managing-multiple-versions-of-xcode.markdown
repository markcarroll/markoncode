---
layout: post
title: "Managing multiple versions of Xcode"
date: 2012-08-31 11:48
comments: true
categories: 
---

I don't know about you, but I am addicted to the 'new and shiny'. That means I am always on the latest and sometimes not-so-greatest version of every application and tool I use.  In the case of Xcode, running beta versions used to be a tricky process thanks to the way Xcode used to be packaged.  Now however, thanks to the new app bundle distribution mechanism, having side-by-side installations of Xcode is a piece of cake.  

As anyone shipping applications to the iOS App Store knows, you may be able to develop on the latest beta (or even Developer Preview) version of Xcode, but in order to submit to the App Store, you need to use a final released version of Xcode.  Fortunately there is a command line tool `xcode-select` that makes switching easy to do, even switching command-line build script paths.

First open `Terminal`. Then to see which path all the Xcode tools are currently run `xcode-select -print-path`:

{% codeblock lang:sh %}
$ xcode-select -print-path
/Applications/Xcode.app/Contents/Developer
{% endcodeblock%}

As you can see, all the developer tools are currently set to use the shipping version of Xcode.  

To change to the beta installation of Xcode (in this case 4.5DP4) use `xcode-select -switch` as super user with the path to the other Xcode installation.

{% codeblock lang:sh %}
sudo xcode-select -switch /Applications/Xcode45-DP4.app
{% endcodeblock%}

If you now run `xcode-select -print-path` you will see that the path has been changed to the Xcode 4.5 DP4 directory.

{% codeblock lang:sh %}
$ sudo xcode-select -switch /Applications/Xcode45-DP4.app
$ xcode-select -print-path
/Applications/Xcode45-DP4.app/Contents/Developer
{% endcodeblock%}


To verify this let's take a look at the path of one of the commonly used Xcode utilities `opendiff`.  If you try to see where `opendiff` resides, however, you will get an interesting answer:

{% codeblock lang:sh %}
$ which opendiff
/usr/bin/opendiff
{% endcodeblock%}

You may have expected something a little different there. If you dig a little deeper you see something even stranger:

``` sh
$ ls -la /usr/bin/opendiff
lrwxr-xr-x  1 root  wheel  5 Jul 30 23:42 /usr/bin/opendiff@ -> xcrun
```

The `opendiff` command is actually linked to and therefore calling `xcrun` instead. it turns out `xcrun` is another tool that Xcode uses to manage path issues in quite an elegant way. 
Try running `xcrun -find opendiff` and you will get the answer you were probably expecting when you ran `which`:

{% codeblock lang:sh %}
$ xcrun -find opendiff
/Applications/Xcode45-DP4.app/Contents/Developer/usr/bin/opendiff
{% endcodeblock%}

Adding a `-v` gives you the full explanation of what is happening here:

{% codeblock lang:sh %}
$ xcrun -v -find opendiff
xcrun: note: opening log file '/var/folders/xg/_f7d3jt955q3vhxyj5xd4zth0000gn/T/xcrun_db.log' with mode 'a'
xcrun: note: SDKROOT = ''
xcrun: note: TOOLCHAINS = ''
xcrun: note: DEVELOPER_DIR = '/Applications/Xcode45-DP4.app/Contents/Developer'
xcrun: note: xcrun via opendiff (xcrun)
xcrun: note: database key is: opendiff|||/Applications/Xcode45-DP4.app/Contents/Developer|
xcrun: note: lookup resolved in '/var/folders/xg/_f7d3jt955q3vhxyj5xd4zth0000gn/T/xcrun_db' : '/Applications/Xcode45-DP4.app/Contents/Developer/usr/bin/opendiff'
/Applications/Xcode45-DP4.app/Contents/Developer/usr/bin/opendiff
{% endcodeblock%}

As you can see, the Xcode developers have put a lot of work behind the scenes to make transitioning between builds of Xcode, and associated developer tools, as easy as possible.

I hope this has been a useful and maybe a little interesting look at managing side-by-side installations of Xcode on OS X.


Mark



