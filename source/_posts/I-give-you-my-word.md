---
title: I give you my word...
tags: []
date: 2016-10-02 16:03:46
---

I&rsquo;m excited about what I just implemented. I&rsquo;m always excited when I figure something out or learn something big. This one is big for me. Hopefully this post will add some value to your life as well.

## First the problem statement...

Here&rsquo;s what I wanted to do. I am working on a Windows 8 Metro app using HTML/JavaScript.

I have a data.js file that represents (for now, until I put the data in the cloud) my data service. I am loading data up from a roaming file that contains simple JSON data. Actually, is &ldquo;simply JSON&rdquo; redundant. JSON just IS simple, right?

My hub page then needs to make sure that the application data is loaded and then it needs to request some. I can&rsquo;t just directly request the data as is though because it needs to be massaged into the right shape to back the hub view. My application has items of Type A and Type B and I need to essentially flatten that into a super type that my hub will represent in different groups.

So I need to fetch some data and it needs to happen in an async fashion.

I&rsquo;ve consumed objects that follow the Promise pattern plenty, but here I need to create my own Promise and get all custom about when it is considered complete and what it returns as its payload.

So I have two methods that I need to make asynchronous: loadAppDataAsync() and getHubItemsAsync(). The former is internal to my data.js function, but the latter is &ldquo;exposed&rdquo; to the rest of the app using a WinJS.Namespace called &ldquo;data&rdquo;.

## loadAppDataAsync

This method, as I said, is &ldquo;internal&rdquo; to my data.js file. That is, it&rsquo;s defined inside the modular function that encapsulates everything on my data.js file and thus is not available anywhere else in the application. The purpose of the function is to read the applicationData.json file and populate some arrays with the results. Here&rsquo;s the method definition...

<pre class="brush: js;">
function loadAppDataAsync() {
    return new WinJS.Promise(function (c, e, p) {
        if (loaded) c();
        else {
            appdata.roamingFolder.getFileAsync(&quot;applicationData.json&quot;)
                .then(function (file) {
                    Windows.Storage.FileIO.readTextAsync(file)
                        .then(function (content) {
                            //load the checklists
                            JSON.parse(content).checklists.forEach(function (i) {
                                checklists.push({
                                    title: i.title,
                                    template: i.template,
                                    description: i.description,
                                    backgroundImage: i.backgroundImage,
                                    items: i.items,
                                    type: &quot;checklist&quot;
                                });
                            });

                            //load the templates
                            JSON.parse(content).templates.forEach(function (i) {
                                templates.push({
                                    title: i.title,
                                    description: i.description,
                                    backgroundImage: i.backgroundImage,
                                    items: i.items,
                                    type: &quot;template&quot;
                                });
                            });
                        })
                        .then(function () { c();});
                });
        }
    });
}</pre>

I added the Async suffix to the method name because this method not only calls an async method, but also acts asynchronously itself. Here&rsquo;s how that&rsquo;s implemented.

If I just wanted to wrap up the getFileAsync method call, then I could simple return the result of that call, but I need to add a little bit of my own logic. I only want to make the getFileAsync call in the condition that my data has not already been loaded. So I return a _new_ WinJS.Promise specifying its function (which receives _complete_, _error_, and _progress_ attributes that I&rsquo;ve called _c_, _e_, and _p_). In the function, if the data has already been loaded I call the complete method _c _(passing no attributes) immediately indicating that we&rsquo;re done loading app data (because it&rsquo;s already loaded). If the data has not been loaded, however, we have some work to do and we will not complete (by calling _c_) until we&rsquo;re done with it (that&rsquo;s why it&rsquo;s in the .then).

This one is pretty simple. Let&rsquo;s move on to the next method that we use to get the hub items after the application data has already been loaded. Here&rsquo;s the code...

<pre class="brush: js;">
WinJS.Namespace.define(&quot;data&quot;, {
    getChecklists: function () { },
    getHubItemsAsync: function () {
        var hubItems = [];
        return new WinJS.Promise(function(c, e, p) {
            loadAppDataAsync()
                .then(function () {
                    checklists.forEach(function(cl) {
                         hubItems.push({
                             item: cl,
                             title: cl.title,
                             backgroundImage: cl.backgroundImage,
                             section: &quot;My Checklists&quot;,
                             click: function (ev) {
                                  WinJS.Navigation.navigate(&quot;/html/checklistPage.html&quot;, { item: cl });
                             },
                             get subtitle() {
                                 return (cl.items.filter(function (i) { return i.checked; }).length / cl.items.length)
                                     + &quot;% of &quot; + cl.items.length + &quot; items&quot;;
                             }
                         });
                    });
                    templates.forEach(function(t) {
                         hubItems.push({
                            item: t,
                            title: t.title,
                            backgroundImage: t.backgroundImage,
                            section: &quot;My Templates&quot;,
                            click: function (ev) {
                                WinJS.Navigation.navigate(&quot;/html/template.html&quot;, { item: t });
                            },
                            get subtitle() { return t.items.length + &quot; items&quot;; }
                         });
                    });
                })
                .then(function () { c(hubItems); });
        });
    }
});</pre>

This one is defined in a Namespace so that we can call it from elsewhere in our app - namely from hub.js. What this method does is a little different from the former. Like before, we are returning a _new_ Promise. Unlike before, after we populate our result (hubItems), we complete the promise with a result by calling c(hubItems).

Passing a value to the complete function makes that value available to the caller like this...

<pre class="brush: js;">
data.getHubItemsAsync().then(function(hubItems) {
    hubItems.forEach(function(i) { hubItemList.push(i); });
});</pre>

Simple, eh? That&rsquo;s the way it&rsquo;s supposed to be. I want to get then items and _then_ I want to push each of the results into my WinJS.Binding.List (here called hubItemList).

Now the grid on my hub is bound to a List that gets populated with data _after_ the application data has been loaded and _after_ the getHubItemsAsync method has projected the application data to look how we want it to look.

Phew! This JavaScript stuff is pretty crazy. It&rsquo;s pretty awesome though.

Happy promise keeping!