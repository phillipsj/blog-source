---
title: Moment.js and Ember-CLI
tags:
  - EmberJS
  - Helpers
  - Moment.js
  - Ember-CLI
date: 2014-11-19 19:00:00
---

Moment.js is not inlcuded by default with Ember-CLI, so if you want to create a helper
to generate beautifully formatted dates, then you need to make sure to install the ember-cli-addon
for it.

Here is the command:

{% codeblock lang:shell-script %}
$ npm install --save-dev ember-cli-moment
{% endcodeblock %}

You will also have to update your jshintrc to inlcude “moment” in the predec section. However, I just
discovered the ember-moment addon that creates helpers for you to use so you do not need to create your own.

{% codeblock lang:shell-script %}
$ npm install --save-dev ember-moment
{% endcodeblock %}

You can check both out on ember addons website [here](http://www.emberaddons.com/#/?q=moment), the link takes you to all packages that involve moment
or you can search by the names in the commands above.