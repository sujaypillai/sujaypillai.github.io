---
layout: post
title: Aikau Audit Report
---

This is my first Aikau plugin and would like to share my experience building it. While during the days when people used Alfresco Explorer there was an option to view the audit trail for a document using the show_audit.ftl by selecting ***"Preview in Template"***.
Below is a screenshot of how it looked - 

![Alfresco Explorer Audit Trail]({{ site.baseurl }}/img/audit-example-explorer.jpg)

But there is no such option in Alfresco Share to view those audit data, so I thought of building it. Auditing is by default enabled in Alfresco but the data generation for the same is disabled and to enable this you would have to put in the below property in alfresco-global.properties.
{% highlight properties %}
        audit.alfresco-access.enabled=true
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




