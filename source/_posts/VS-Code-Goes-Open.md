---
title: VS Code Goes Open
tags: []
date: 2016-10-02 16:03:46
---

<div style="direction:ltr;border-width:100%">
<div style="direction:ltr;margin-top:0in;margin-left:0in;width:7.6041in">
<div style="direction:ltr;margin-top:0in;margin-left:0in;width:7.6041in">

<cite>Me: What do you think of Visual Studio Code?</cite>

<cite>Some Dude: It&#39;s awesome. I just wish it were open source.</cite>

<cite>Me: You need to fork it? Tweak it?</cite>

<cite>Some Dude: No.</cite>

<cite>Me: Okay.</cite>

&nbsp;

I get it. I like open source stuff too.

Realistically, there are few products I have time to fork and fewer still that I have <span style="font-style:italic">need</span> to fork.

But even when I have no need to fork a project and no intention to submit a pull request any time soon, still I want it to be open source. Why? Because&hellip; freedom.

I like closed source products too, actually. Closed source products can be sold. Selling products earns a company money. Companies with money can create big research and development departments that can tinker with stuff and make new, cool stuff. And ultimately, I like new cool stuff.

The best scenario for me, a consumer, though, is when a big company with a big research and development department can afford to make something cool and free _and open_, because they make money on other products.

Some products (think Adobe Photoshop) are obviously a massive mess of proprietary code that feel right to belong to their parent company. They need the first-party control.

Others, like Code feel more like they belong to the community. That&#39;s how I feel anyway.

&nbsp;

<big>And now I can. Visual Studio Code is officially OSS! *screams*</big>

&nbsp;

In case you missed it, Microsoft announced at [Connect() 2015](https://channel9.msdn.com/Events/Visual-Studio/Connect-event-2015) that Code was graduating from preview to beta status and that it would be open sourced.

To see Code&#39;s code comfortably settled into its new home, just head over to&nbsp;<span style="font-family: Calibri; font-size: 14.6667px; line-height: 20.8px;">[github.com/microsoft/vscode](http://github.com/microsoft/vscode). From there, you can clone it, fork it, submit an issue, submit a PR... or look at what the team is working on and who else is involved. You know... you can do all of the GitHub stuff with it.</span>

So there it is. It&#39;s not only free as in &quot;free beer&quot; now, but also as in &quot;free speech&quot;.

The actual announcement is buried in the [keynote](https://channel9.msdn.com/Events/Visual-Studio/Connect-event-2015/010), so the best way to get the skinny on this announcement, the details, and the implications is to watch the [Visual Studio Code session](https://channel9.msdn.com/Events/Visual-Studio/Connect-event-2015/032)&nbsp;hosted on Connect() Day 2 by [@chrisrisner](http://twitter.com/chrisrisner). The panel shows off Code in serious depth. It&#39;s a must-see session if you&#39;re into this stuff.

One of the more exciting things they showed off is actually the second gigantic announcement regarding Code... the addition of extensions to the product, but that&#39;s a big topic for another day and another blog post.

What exactly does the open sourcing of Code mean for you?&nbsp;<span style="font-size: 11pt; line-height: 1.6em;">As I mentioned, you may or may not be interested in ever even viewing the source code for Code. The real gold in this announcement is the fact that Code now belongs to the community. It&#39;s ours. It&#39;s something that we&#39;re all working on together. That&#39;s no trivial matter. Microsoft may have kicked it off and may be a huge contributor to it here forward, but so are you and I.</span>

<span style="font-family: Calibri; font-size: 14.6667px; line-height: 23.4667px;">So whether you&#39;re going to modify the code base, study the code base, or just take advantage of the warm feeling that open source software gives us, you know now that the best light-weight code editor for Windows, Linux, and Mac, is ready for you.</span>

<span style="font-family: Calibri; font-size: 14.6667px; line-height: 23.4667px;">Let&#39;s have a quick look at the code for&nbsp;Code using Code. ​The official repo is at </span>http://github.com/Microsoft/vscode. So start by cloning that into your local projects folder. My local projects folder is c:\code, so I do this...

&nbsp;

![](http://codefoster.blob.core.windows.net/site/image/b332207857c64570a63287f929439f75/vscodeopen_clone_6.png)

&nbsp;

Then, you launch that project in Code using...

&nbsp;

![](http://codefoster.blob.core.windows.net/site/image/194eb76735394db185bbeecfde42e4ac/vscodeopen_launch_1.png)

&nbsp;

You&#39;ve got it now. So I just added &quot;codefoster&quot; to a readme.md file to simulate a change and then hit CTRL + SHIFT + G to switch to the Git source control section of VS Code, and here&#39;s what I see...

&nbsp;

![](http://codefoster.blob.core.windows.net/site/image/ffea66fb3b8740819602f212395f4370/vscodeopen_changes_6.png)

&nbsp;

&nbsp;

Notice that the changed file is listed on the left and when highlighted the lines that were changed are compared in split panes on the right. Checking this change in would simply involve typing the commit message (above the file list) and then hitting the checkmark.

This interface abstracts away some of the git concepts that tend to intimidate newcomers - things like pushing, pulling, and fetching - with a simpler concept of _synchronizing_&nbsp;which is accomplished via the circle arrow icon.

It&#39;s important to note that I wouldn&#39;t be able to check this change in here because I don&#39;t have direct access to the VS Code repo. Neither do you most likely. The git workflow for submitting changes to a repo that you don&#39;t have direct access to is called a&nbsp;_pull request_. I&#39;ll leave the expansion of this topic to other articles online, but in short it&#39;s done by&nbsp;_forking_&nbsp;the repo, cloning your fork, changing your files, committing and pushing to&nbsp;_your_&nbsp;fork, and then using github.com to submit a pull request. This is you saying to the original repo owner, &quot;Hey, I made some changes that I think benefit this project. They are in my online repository which I forked from yours. I hereby _request_ you _pull _these changes into the main repository.

It&#39;s quite an easy process for the repo owner and I don&#39;t think a repo owner on earth is opposed to people doing work for them by submitting PR&#39;s. :)

<span style="line-height: 23.4667px;">Again, getting involved simply means interacting and collaborating on GitHub. Here&#39;s how...</span>

*   <span style="line-height: 23.4667px;">Check out the list of issues (there are already over 200 of them as I type this) on [microsoft/vscode](http://github.com/microsoft/vscode) repo.</span>
*   <span style="line-height: 23.4667px;">Chime in on the issues by submitting comments.</span>
*   <span style="line-height: 23.4667px;">Create your own issue. [See how](https://help.github.com/articles/creating-an-issue/).</span>
*   Clone the code base using your favorite git tooling or using `git clone https://github.com/microsoft/vscode.git` on your command line. That will allow you to `git pull` anytime you need to get the latest. Having the code means you can browse it whenever you&#39;re wondering how something works. [See how](http://help.github.com/articles/cloning-a-repository/).
*   Fork the code using GitHub if you want to create a copy of the code base in your own GitHub repo. Then you can modify that code base and submit it via a pull request whenever you&#39;re certain you&#39;ve added some value to the project. [See how](http://help.github.com/articles/fork-a-repo/).

And you can chatter about Code as well on Twitter using [@Code](http://twitter.com/code). As to how they got such an awesome handle on Twitter I have no idea.

Also check out my mini-series I&#39;m calling [Tidbits of Code and Node](https://channel9.msdn.com/Search?term=tidbits%20of%20code%20and%20node#ch9Search) on the [Raw Tech blog](https://channel9.msdn.com/Blogs/raw-tech) on [Channel 9](http://channel9.msdn.com)&nbsp;where I&#39;ve been talking a lot about Code (and Node) and plan to do even more now that the dial for its awesome factor was turned up a couple of notches.

In fact, I recorded a Tidbits of Code and Node on this topic precisely where I show you how to find the source code on GitHub, how to clone it, how to fork it, and all that goodness. You can watch it below or find it anytime [on Channel 9](http://channel9.msdn.com/blogs/raw-tech/tidbits07).

Happy coding in Code!

<iframe allowfullscreen="" frameborder="0" height="394" src="https://channel9.msdn.com/Blogs/raw-tech/tidbits07/player" width="700"></iframe></div>
</div>
</div>