---
layout: post
title:  "Using GitHub for Windows Behind Microsoft ISA Proxy"
categories: git
---

If you're not familiar with the pain and torture that [Microsoft ISA Proxy][isaproxy] forces on users consider yourself lucky. As a developer and user that is required to work behind one of these proxy servers I know the pain well. Thankfully, there are other developers out there that have felt the pain and have provided some powerful tools for working around an ISA server. One of the best is [CNTLM][cntlm], which provides a way to reach the internet through an ISA server by silently and automatically authenticating the request first. This is vital for using applications such as the wonderful, new [GitHub for Windows][githubwin].

The first step is to install [CNTLM][cntlm]. Make sure to download and install version 0.92 or newer. Older versions of CNTLM [have a bug that prevent Git from working properly][cntlmbug] through CNTLM. You'll need to follow the installation and configuration instructions provided with CNTLM. I won't cover them here.

Next, install [GitHub for Windows][githubwin]. Thankfully, the installer works behind the ISA proxy server without issue. I suspect the installer uses the system proxy server settings to download the installation files.

Start GitHub and work your way through the configuration. Assuming you have a GitHub.com account (create one now if not) you will see a list of your repositories. If you try to clone one you'll immediately get an error. This is expected. Shutdown GitHub.

GitHub for Windows uses the standard Git configuration file (`.gitconfig`) so this is where you'll need to set the proxy server. This file should already exist in your user profile directory. This is denoted by the <tt>USERPROFILE</tt> environment variable and is usually under `C:\Users` or `C:\Documents and Settings`. Add a section named. `[http]` and set the proxy setting to your local CNTLM server. The default port for CNTLM is 3128.

{% highlight properties %}
[http]
	proxy = http://localhost:3128
{% endhighlight %}

Start GitHub again and you should now be able to clone your repositories. If you run into issues make sure that you configured CNTLM correctly and that the service is running.

[cntlm]: http://sourceforge.net/projects/cntlm/
[cntlmbug]: http://sourceforge.net/tracker/?func=detail&aid=3106663&group_id=197861&amp;atid=963162
[githubwin]: http://windows.github.com/
[isaproxy]: http://en.wikipedia.org/wiki/Microsoft_Forefront_Threat_Management_Gateway