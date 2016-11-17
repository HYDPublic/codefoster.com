---
title: Creating an Observable Object in Windows 8 JavaScript
categories: []
tags: []
date: 2012-04-10
permalink: observables
---

Living in the JavaScript world for a while will help you to appreciate the offerings of C# for sure. Many concepts like classes, inheritance, observable collections, list extensions (LINQ) are  simply absent and so a creative alternative has been created either in WinJS or just in recommended practice.
<!-- xmore -->

One of these is in the area of data binding.

First of all, I recommend Stephen Walther&#39;s excellent article on Metro: [Declarative Data Binding](http://bit.ly/IbSFCY). He&#39;s much more thorough than I intend to be here. I only want to relay my experience with following the instructions on dev.windows.com &ndash; specifically the article entitled [How to bind a complex object](http://msdn.microsoft.com/en-us/library/windows/apps/hh700355.aspx), and following the sample project entitled [Programmatic binding sample](http://code.msdn.microsoft.com/windowsapps/ProgrammaticBinding-de038b64). If you want to learn a lot more about Windows 8 development using HTML and JavaScript, you can check out Stephen&#39;s book [Windows 8.1 Apps with HTML5 and JavaScript](http://www.amazon.com/gp/product/B00HJUBRQK/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;camp=1789&amp;creative=9325&amp;creativeASIN=B00HJUBRQK&amp;linkCode=as2&amp;tag=codefostercom-20).

Here are the steps I took to get a very simple object to bind well.

*   Write a simple class
*   Add an _initObservable method
*   Mix it
*   Instantiate it
*   Bind the properties
*   Change a property to test

That&#39;s it. I&#39;ll elaborate a bit on each point.

**Write a simple class**

Here&#39;s the rocket science class I wrote. It has a single property &ndash; _name_.

``` js
var Widget = WinJS.Class.define(
    function (name) {
        this.name = name;
    }
);
```

**Add an _initObservable method**

``` js
var Widget = WinJS.Class.define(
    function (name) {
        this._initObservable();
        this.name = name;
    }
);
```

This is necessary so that "the observablility implementation is completed properly".

**Mix it**

``` js
WinJS.Class.mix(Widget,
    WinJS.Binding.mixin,
    WinJS.Binding.expandProperties(Widget)
);
```

This adds the necessary bindability to your custom class. If you only want a select few of your properties, you can specify a subset of properties in the parameter for the expandProperties method call. See the [Programmatic binding sample](http://code.msdn.microsoft.com/windowsapps/ProgrammaticBinding-de038b64) to see what I mean.

**Instantiate it**

Now you&#39;re ready to instantiate your object.

``` js
var widget = new Widget("Widget1");
```

**Bind the properties**

What you&#39;re doing in the binding is specifying a function that you want to run whenever a property is changed.

``` js
widget.bind("name", function (newValue, oldValue) {
    var foo = document.querySelector("#foo");
    foo.innerText = newValue;
});
```

**Change a property to test**

Now, somewhere in your code, change value of that property like this...

``` js
widget.setProperty("name", "Hi, Mom!");
```

And there you have it. The simplest possible example I could come up with for data binding a complex object by turning it into the JavaScript version of an observable object.

Happy binding!