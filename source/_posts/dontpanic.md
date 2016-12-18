---
title: Unintentionally Open Source?
categories: [Windows]
tags: [oss,source code]
date: 2012-12-11
alias:
- post/2012/12/11/dontpanic/
- post/2012/12/11/dontpanic.aspx
---

I saw this tweet and decided to respond.

![](/files/dontpanic_01.png)

The technical answer is YES. You can access the source code for Windows Store apps written in HTML/JavaScript. But there are a few reasons that you should forego panic.

First, **this is no surprise**. App packaging and distribution has a ton of design and testing behind it. The designers knew exactly what was going to the client and exactly how much effort it takes to discover it. Developers are responsible for their own obfuscation strategy. If Microsoft created their own, it would just get pwned in a couple weeks and then it would be a senseless inclusion in the product. It&#39;s better for obfuscation to be out of band with the Windows product and for it to be contributed by third parties.

Next, the way it works with HTML/JavaScript apps is actually **similar to many other language stacks**. JavaScript is clear text and translated script so it&#39;s very easy to look for a .js file and read it. Managed languages such as .NET languages are only logistically more difficult to reverse engineer and capture the source from.

Next, the source code is available on the client, but **the package is tamper proof**. If a hacker finds it and changes one of the .js files so that the script now does something it wasn&#39;t intended to do (such as validate that an in-app purchase has been paid for or that the app is not in trial mode), then Windows will not allow that package to execute.

Finally, the bottom line is that you should **never trust code on a client**. It doesn&#39;t matter the language, the platform, or even the obfuscation technique. It can and eventually will be hacked. You should consider how sensitive your intellectual property is and protect accordingly. If you have very valuable business logic that would hurt you or your company if it&#39;s taken then I wouldn&#39;t even put that logic into the client app. I would put it in the cloud and make it available through service calls. That&#39;s a better architecture for a lot of other reasons as well. If you are a hobby app developer and you wrote a silly app, I wouldn&#39;t worry about it. It&#39;s a very small portion of the population that&#39;s going to try to take your code. The chances that your stolen code is going to turn into much added value for them is low and the chances that their efforts with it are going to hurt your business is even lower.