title: Push git onto Github
date: 2013-04-21 02:26:58
tags: [git]
---

{% codeblock bash lang:sh %}
$ cd project_folder
$ git init
$ git add .
$ git commit -a -m 'init'
$ git remote add github https://github.com/evenchange4/101-1_MMP_HW1_Calculator.git
$ git remote -v
$ git pull github master
$ git push github master

github login
{% endcodeblock %}  
<!-- more -->