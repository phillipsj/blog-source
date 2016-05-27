---
title: Shinken Day One
tags:
  - Monitoring
  - Python
date: 2014-09-07 20:00:00
---

# Shinken Day One

I have been investigating monitoring tools to determine which
one would best fit our skillsets and environment. While I was
at the City of Austin, we used Nagios and I found it an awesome
tool because of the alerting and alert escalation that was built
in. [Shinken](http://www.shinken-monitoring.org/) is a montioring framework written in Python with it’s
heritage being Nagios.  Since it is a Python based framework I
thought what would be better to use.

Today, is the first day of a new sprint where my focus is on getting
our monitoring up and running for our entire stack. I used the tutorials
called Online Course 1 and 2 found on the Shinken [blog](http://shinkenlab.io/) and besides a few
extra dependencies that are not mentioned it went smooth until the active
directory configuration. It basically came down to needed to include the
domain name with the username. Below is the packages I had to install
when using Ubuntu 14.04\. I had monitoring configured on Shinken server like
in the 2nd tutorial.

<div class="highlight-none"><div class="highlight"><pre>$ apt-get install sysstat ntp python-ldap
</pre></div>
</div>

Then at the end of the second tutorial I chose to do active directory
and the sticking point is highlighted below. Make sure to include the
domain in the username.

<div class="highlight-none"><div class="highlight"><pre> define module {
     module_name ActiveDir_UI
     module_type ad_webui
     ldap_uri ldaps://adserver
<span class="hll">     username user@domain
</span>     password password
     basedn DC=google,DC=com
     # For mode you can switch between ad (active dir)
     # and openldap
     mode    ad
 }
</pre></div>
</div>