---
title: One Sweet Stack
tags: []
date: 2016-10-02 16:03:46
---

Following is a mongo post. A huge post. A massive amount of information. The general recommendation is that blog posts should be short, but rules are made to be broken. You can&rsquo;t tame me. I&rsquo;m like a wild stallion. So here is a huge blog post.

Last Saturday at the Seattle Code Camp I delivered a presentation I called One Sweet Stack which showed how to start with a SQL Azure database (though it would work with any relational database really), connect to it using Entity Framework, and extend it as OData with WCF Data Services.

I chose this stack because&hellip;

*   I come from corporations that have existing database solutions. These aren&rsquo;t modern, green-field databases of the myriad of flavors. These are classic, tried-and-true, and very much relational. I&rsquo;m as excited as the next guy all of the modern ways to persist data, but don&rsquo;t think for a minute that the relational database story is obsolete. Far from it.
*   I love using Entity Framework. I get a little jolt of excitement when I instantiate a DbContext or call SaveChanges(). Geeky? Of course.
*   I think that WCF DS is oft overlooked and recently especially in light of WebAPI (which is also a great product). I&rsquo;m a fan of designing a database, mapping it through an ORM, and providing an elegant API (whether it&rsquo;s internal or external) with so little code that I can write it from scratch in a 1 hr session (including explanations).
*   Windows 8 thrills me even more than EF.

I&rsquo;m hoping to convey virtually all of the content from the presentation here, so it will be a heavy post. Consider it a reference post and come back to it if/when you need it.

The source code for this project is attached. You can find it at the bottom of this post.

## First, the database&hellip;

So, as I said, I started with a SQL Azure database.

You connect to a SQL Azure database using a regular connection string just like any other database, so it will be a no-brainer for you to read this and apply it to a SQL Server on premises or even a MySQL or an Oracle database.

My database is a simple schema. It&rsquo;s just a table of a few attractions that one will find on the island of Kauai, Hawaii and one related table of categories those attractions fall into (i.e. waterfall, scenery, flora, etc.). Here&rsquo;s a diagram&hellip;

[![](http://codefoster.blob.core.windows.net/site/image/60cec6a42b4f4fbea3a37ef9ba881f4c/sweetstack_01_1.png "image")](http://{fix}/image.axd?picture=Windows-Live-Writer/One-Sweet-Stack/07D08BD4/image.png)

(my diagram by the way was done using [asciiflow.com](http://www.asciiflow.com/)&hellip; very geeky indeed)

In the attached zip file, you&rsquo;ll find KauaiAttractionsAzureScript.sql that you can use to create this database on your own Azure (or local if you&rsquo;d rather) instance. Just create the database first and then run the script in the context of your new database. If you want to run through this whole exercise connecting to your own database, however, I would highly recommend it. It would be good practice.

## Next, setting up the solution&hellip;

Follow these mundane steps to get over the snoozer that is creating projects, adding references, and importing packages&hellip;

1.  Create a new solution in VS2012
2.  Add a new Windows Class Library using C# (call it SweetStack.Entities)
3.  Add a new WCF Service Application using C# (SweetStack.Service)
4.  Add a new Cloud project using C# (SweetStack.Cloud)
5.  Add a new Unit Test Project using C# (SweetStack.Tests)
6.  Add a new Navigation App for JavaScript Windows Metro style (SweetStack.Metro)
7.  Add a reference to SweetStack.Entities to the .Service and the .Tests projects
8.  Add the .Service project as a web role to the .Cloud project

        1.  In the .Cloud project
    2.  Right click Roles
    3.  Add | Web Role Project in Solution&hellip;
    4.  Choose the .Service project
9.  Add the latest version of Entity Framework (currently 5.0.0-rc) to the .Entities, .Services, and .Tests projects
10.  Add the latest version of Microsoft.Data.Services (currently 5.0.1) to the .Services project

## Next, creating the .Entities project&hellip;

We have our database already in place, and now we want to create an Entity Framework context that will allow us to access our database using code.

Instead of creating an EF model (.edmx file), we are going to reverse engineer the database to POCO classes. Why? Because it&rsquo;s rad. That&rsquo;s why. First thing you need to do is install the Entity Framework Power Tools Beta 2 (from Tools | Extensions and Updates in VS2012).

Once that is done, you can right click on your .Entities project and choose Entity Framework | Reverse Engineer Code First. Then enter your connection string information. Remember to check the &ldquo;Remember my password&rdquo; box so that it will save your credentials into your connection string for later.

So the tooling should have created a bunch of .cs files in your .Entities project. You not only get POCO classes for each of your database tables, you also get one for the context. That&rsquo;s the one that derives from DbContext. You also get a folder with a map file for each entity.

All of this is beautiful and I&rsquo;ll tell you why. You now have a direct 1:1 relationship between your code and your database, but you also have the complete freedom to modify the mappings so that the two don&rsquo;t necessarily match. If your data architect, for instance, called the database table &ldquo;first_name&rdquo; and you&rsquo;d rather that be called FirstName in your code, then just change that property but keep the mapping to &ldquo;first_name&rdquo;. You can even ignore certain properties or add new ones that don&rsquo;t have a mapping. Furthermore, classes that DO have database mappings can be mixed with other classes that do NOT have mappings. It&rsquo;s all up to you.

## Next, let&rsquo;s test it&hellip;

It&rsquo;s hard to see a Windows class library work without writing a test for it. In the .Tests project write a simple test that looks something like this&hellip;

<pre class="brush: csharp;">
[TestMethod]
public void TestMethod1()
{
    var context = new SweetStack.Entities.Models.KauaiAttractionsContext();
    Assert.IsTrue(context.Attractions.Any());
}</pre>

Before you can run the test, copy the &lt;connectionstrings&gt; element from the app.config, create a new app.config in the .Tests project (right click Add New Item&hellip;), and then paste the &lt;connectionstrings&gt; element into the app.config for .Tests.

That test should pass if you haven&rsquo;t mucked anything up already.

## Next, time to create the .Service&hellip;

This one just FEELS like it&rsquo;s going to take a while. Low and behold, however, I bet I could do it in less than 37.5 seconds (not that I&rsquo;ve timed myself). Do this&hellip;

1.  Delete (from the .Service project) the IService1.cs and Service1.cs files that you got for free (even though you didn&rsquo;t ask for them :)
2.  Right click the .Service project and add a new item&hellip; add a WCF Data Service called Entities.svc
3.  Once your file is created, check out the class name and see how it derives from DataService&lt;T&gt; but the T is undefined. Fill that in with SweetStack.Entities.Models.KauaiAttractionsContext
4.  Now uncomment the line in the InitializeService method that says SetEntitySetAccessRule and in the quotes just specify an asterisk (&ldquo;*&rdquo;). You can change the EntitySetRights.AllRead to .All if you like, but we won&rsquo;t be writing any data in this tutorial anyway, so it doesn&rsquo;t matter so much.
5.  Copy the &lt;connectionstrings&gt; element from the app.config of the .Entities project into the web.config of your .Service project
6.  Put your hands down&hellip; you&rsquo;re done!

Set your .Service project to the startup project and run it. You should get a browser that looks like this&hellip;

[![](http://codefoster.blob.core.windows.net/site/image/272cd01aac23421e9d3558e8d18879fe/sweetstack_02_1.png "image")](http://{fix}/image.axd?picture=Windows-Live-Writer/One-Sweet-Stack/7B64F95B/image.png)

Note: if you get a list of files instead, just click on the Entities.svc first.

## Next, we let&rsquo;s see what we&rsquo;ve got&hellip;

What you&rsquo;re looking at there is a GEN-YOU-WINE OData feed. That&rsquo;s exciting. OData rocks. Not only do you get all of your entities extended through OData, but you get type information about them and you get their relationships with each other. Also, you can ask an OData feed for XML or for JSON and it will say &ldquo;Yes, sir/ma&rsquo;am.&rdquo;

Fire up Fiddler and hit that service root URL appending each of the following and see what you get for responses (also add &ldquo;Accept: application/json;odata=verbose&rdquo; to the headers in Fiddler to request JSON). Issue the following commands against your service and behold the results&hellip;

<table border="0" cellpadding="2" cellspacing="0" style="width: 825px;">
	<tbody>
		<tr>
			<td valign="top" width="351">_{root service URL}?$top=1_</td>
			<td valign="top" width="472">selects just the first entity.</td>
		</tr>
		<tr>
			<td valign="top" width="351">_{root service URL}?$select=Id,Name_</td>
			<td valign="top" width="472">fetches all entities, but projects them to lighter JSON objects by only including the Id and Name properties.</td>
		</tr>
		<tr>
			<td valign="top" width="351">_{root service URL}?$filter=Location%20eq%20&#39;North&#39;_</td>
			<td valign="top" width="472">gives you entitites that have a Location value of &ldquo;North&rdquo;.</td>
		</tr>
		<tr>
			<td valign="top" width="351">_{root service URL}?$filter=substringof(&#39;Falls&#39;,Name)%20eq%20true&rdquo;_</td>
			<td valign="top" width="472">gives you only entites with the word &ldquo;Falls&rdquo; in their Name</td>
		</tr>
		<tr>
			<td valign="top" width="351">_{root service URL}?$select=Name&amp;$orderby=Name_</td>
			<td valign="top" width="472">selects just the Name property and sorts it</td>
		</tr>
		<tr>
			<td valign="top" width="351">_{root service URL}?$expand=Category_</td>
			<td valign="top" width="472">this one brings in the related Category entity&hellip; this a significant point and a differentiator from flat GET web service operations. Look at the JSON message in the response with and without this URL property.</td>
		</tr>
	</tbody>
</table>

If that doesn&rsquo;t turn your crank then you should check your Geek card&hellip; it might be expired.

## Next, we go Metro&hellip;

We&rsquo;re ready to consume our data. We&rsquo;re going to be working here with an HTML/JS Metro application which makes it reasonable brainless to consume JSON. Here we go&hellip;

I had you create your Metro app from the navigation template, so you should have a _pages _folder (assuming your using Visual Studio 2012 as opposed to Visual Studio 11). In there you have home.html, home.css, and home.js. Those three files are all we&rsquo;re going to concern ourselves with for now.

In the .html file, you need to create a ListView and define an item template and a header template (because we want our items to appear in groups). Here&rsquo;s what that would look like&hellip;

<pre class="brush: xml;">
&lt;div id=&quot;itemtemplate&quot; data-win-control=&quot;WinJS.Binding.Template&quot;&gt;
    &lt;div data-win-bind=&quot;onclick:click&quot;&gt;
        &lt;img data-win-bind=&quot;src:ImageUrl&quot; /&gt;
        &lt;div data-win-bind=&quot;innerText:Name&quot;&gt;&lt;/div&gt;
    &lt;/div&gt;
&lt;/div&gt;
&lt;div id=&quot;headertemplate&quot; data-win-control=&quot;WinJS.Binding.Template&quot;&gt;
    &lt;div data-win-bind=&quot;innerText:category&quot;&gt;&lt;/div&gt;
&lt;/div&gt;
&lt;div id=&quot;list&quot; data-win-control=&quot;WinJS.UI.ListView&quot;&gt;&lt;/div&gt;</pre>

Then in the .css file add the following so that our images are the right size and our ListView is tall enough to show two rows&hellip;

<pre class="brush: css;">
.homepage section[role=main] {
    margin-left: 120px;
}

.homepage #list {
    height: 100%;
}

    .homepage #list img {
        width: 280px;
        height: 210px;
    }</pre>

Finally, in the .js file we need to add just a little bit of code. I&rsquo;ll just drop it all on you and then explain each section. Put this inside the page&rsquo;s _ready_ method&hellip;

<pre class="brush: js;">
var attractionsListGrouped = new WinJS.Binding.List().createGrouped(
    function (i) { return i.Category.Name; },
    function (i) { return { category: i.Category.Name }; }
);

var list = document.querySelector(&quot;#list&quot;).winControl;
list.itemDataSource = attractionsListGrouped.dataSource;
list.itemTemplate = document.querySelector(&quot;#itemtemplate&quot;);
list.groupDataSource = attractionsListGrouped.groups.dataSource;
list.groupHeaderTemplate = document.querySelector(&quot;#headertemplate&quot;);

WinJS.xhr({
    url: &quot;http://onesweetstack.cloudapp.net/Entities.svc/Attractions?$expand=Category&quot;,
    headers: {&quot;Accept&quot;:&quot;application/json;odata=verbose&quot;}
    }).then(function(xhr) {
        JSON.parse(xhr.response).d.forEach(function (i) {
            i.click = function (args) { WinJS.Navigation.navigate(&quot;/pages/attraction/attraction.html&quot;, i); }
            i.click.supportedForProcessing = true;
            attractionsListGrouped.push(i);
        });
    });</pre>

The first part (var attractionsListGrouped&hellip;) creates a new WinJS.Binding.List that groups the items by their .Category.Name. This necessitates that we bring our Attraction entities down with that $expand property included to get the related Category, but that&rsquo;s easy so we worry not.

The next part imperatively sets the item and header templates and the data sources of both the items and the groups. This can be done before our list has even been populated with any items. In fact, we need to do it that way because the call we make to get the items is asynchronous and we need that list that we&rsquo;re binding to to exist before we even get back from that call.

The last part is the _xhr _call. You can see the syntax. The xhr expects an object within which we&rsquo;re specifying the url and a custom header. The function we pass in to the subsequent .then is going to run _after_ we get back from the xhr call. At that point, we can look at the response, parse it as JSON, and then for each item, push it into our list. This list is a WinJS.Binding.List which means that it is essentially _observable_ and will tell the UI when updates have been made so it can change accordingly. So when our items are fetched and filled in, the user will see them pop into the ListView in his view.

## Tangent about application/JSON;odata=verbose&hellip;

Remember how we added _application/JSON;odata=verbose_ to our headers for the xhr call? Why would we do that? It&rsquo;s because we&rsquo;re using the prerelease version of WCF DS, and the existing OData JSON syntax has been dubbed &ldquo;verbose&rdquo; to make room for some awesome new methods for expressing rich, typed, interrelated OData while keeping the payload light, light, light. More on that at a later time.

## Conclusion

Attached you&rsquo;ll find the complete source code. Hope it helps you learn Windows 8 development and I hope you get your first app done soon and are rewarded with huge royalty checks :)

And that does it for this walk through. It was a marathon post, so if you&rsquo;re read this far email me your mailing address and I&rsquo;ll send you a gift in the mail. I&rsquo;m betting I won&rsquo;t be troubled to send too many gifts :) (offer expires the end of June 2012)

[OneSweetStackLive.zip (9.90 mb)](/bcms-media/Files/Download?id=737b79eb-74d2-41a3-a90e-a35200ddf2dd)