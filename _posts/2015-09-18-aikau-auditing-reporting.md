---
layout: post
title: Aikau Audit Report
---

This is my first Aikau plugin and would like to share my experience building it. While during the days when people used Alfresco Explorer there was an option to view the audit trail for a document using the show_audit.ftl by selecting ***"Preview in Template"***.
Below is a screenshot of how it looked - 

![Alfresco Explorer Audit Trail]({{ site.baseurl }}/img/audit-example-explorer.jpg)

But there is no such option in Alfresco Share to view those audit data, so I thought of building it. Auditing is by default enabled in Alfresco but the data generation for the same is disabled and to enable this you would have to put in the below property in alfresco-global.properties.
{% highlight properties %}
```audit.alfresco-access.enabled=true
```
{% endhighlight %}
You can check the status for auditing by issuing the command:

{% highlight console %}
        curl -u admin:password "http://localhost:8080/alfresco/service/api/audit/control"
{% endhighlight %}

On issuing the above command you would get a JSON response something similar -
{% highlight json %}
    {
       "enabled" : true,
       "applications": 
       [
          {
             "name": "Alfresco Tagging Service",
             "path" : "/tagging",
             "enabled" : true
          }
             ,
          {
             "name": "alfresco-access",
             "path" : "/alfresco-access",
             "enabled" : true
          }
             
       ]
}
{% endhighlight %}

If you looking for more information about Alfresco Audting you should definitely look [here](http://docs.alfresco.com/5.0/concepts/audit-intro.html)







If you look into the Email Form while setting an "Send Email" Action on a folder you would be wondering how we can add new email templates into the list of drop down "Use Template" as shown below -

![MyImage]({{ site.baseurl }}/img/img6.png)

The only option without any customization is to add your templates into the folder : **Repository > Data Dictionary > Email Templates > Notify Email Templates**

#### *How does this work?*
Look for the bean 'ac-email-templates' in alfresco/WEB-INF/classes/alfresco/action-services-context.xml which has the definition as below -

{% gist sujaypillai/657379f60bc621e12ba9 %}

The property *searchPath* is what restricts it from fetching templates from a single folder.

#### *What we need to change?*
Let's bring in our changes so that we can have multiple folders to select the templates from. For this we will be overriding the bean as below -

{% gist sujaypillai/b49b2d53f10647dedfec %}

* The first change is converting the property *searchPath* from a String literal to a List.
{% gist sujaypillai/379da91f20beabf42c04 %}
* Then update the *getAllowableValuesImpl()* method so that it parses each folder path to search for what files to be included in the dropdown.
{% gist sujaypillai/12c8316c9ed923d2ce32 %}
* I have also added an extra method *sortByName* so that the values in the dropdown are displayed in sorted order by their name.

Checkout the code for [FolderContentsParameterConstraint](https://github.com/sujaypillai/alf-tutorials/blob/master/alftutorial-repo/src/main/java/org/ootb/repo/action/constraint/FolderContentsParameterConstraint.java) for a better understanding of how it works.

I made this whole project as 2 step -

1. First is bootstrapping a template under a specified path. [v1.0](https://github.com/sujaypillai/alf-tutorials/releases/tag/v1.0)
2. Make the templates available for selection under Email Action. [v2.0](https://github.com/sujaypillai/alf-tutorials/releases/tag/v2.0)

>The source code for this project lives [here](https://github.com/sujaypillai/alf-tutorials)


