---
title: Visual Studio Code
tags: []
date: 2016-10-02 16:03:46
---

If you haven&#39;t watched the first day&#39;s keynote from Microsoft&#39;s Build 2015 conference, [go do that now](http://buildwindows.com/). It has a few wow moments such as Android and iOS code for building Windows 10 apps, elastic pools for Azure SQL databases, and the amazing HoloLens robot that was a hologram and a real robot in one - amazing!

One of the pieces of the conference that&#39;s going to affect my every day, though is Visual Studio&#39;s new, free, light edition called simply Code. Code is what I&#39;ve been looking for in a code editor - in a text editor even. It will replace a few other apps in my MRU list - Visual Studio Community, Notepad++, and Atom to name a few.

**Console.** code.exe is in your path after your basic install, so from your shell, you can type `code` to run the app from scratch. You can also type `code myfile.txt` and launch into the editing of your file or `code mydirectory` to open it with the explorer pane&#39;s context already set to your directory. So my new favorite command is going to be `code .` for opening the current directory in Code. I was looking for some PowerShell magic to make that possible with VS Community, but now I no longer have the need.

**Speed.** It takes about 3 seconds to launch code.exe cold, and iut doesn&#39;t appear to take any extra milliseconds to load either a file or a directory.

**Essentials.** Code is just the essentials. It&#39;s basically a new perspective on authoring code that hopefully complements Visual Studio. If I&#39;m an enterprise developer with a massive code base and a lot of static analysis, workflow automation, and other tooling built in to my development environment, then I can see running the full editions of Visual Studio. There are a lot of support features, it&#39;s plug-in capable, it has excellent GUI Azure tooling. But if I&#39;m working on what I might call more of a scrappy project - a website, a Node.js app, a sample, or whatever - Code is all I need. For some people, I suspect Code will cover 10% of the use cases, but for me, it&#39;s more like 90%. And actually, this is only the beginning. Check out what it says in the official docs: &quot;In future previews, as we continue to evolve and refine this architecture, Visual Studio Code will include a public extensibility model that lets developers build and use plug-ins, and richly customize their edit-build-debug experience.&quot;

**Languages.** There are a ton of languages supported out of the box.&nbsp;Code recognizes Batch, C#, C++, CSS, Clojure, CoffeeScript, Dockerfile, F#, Go, HTML, Handlebars, Ini, JSON, Jade, Java, JavaScript, Less, Lua, Makefile, Markdown, Objective-C, PHP, Perl, Perl 6, Plain text, PowerShell, Python, R, Razor, Ruby, SQL, Sass, Shell Script (Bash), TypeScript, Visual Basic, XML, and YAML. If that doesn&#39;t cover what you&#39;re writing, I&#39;d be really interested to know what you&#39;re writing!

**Commands.** Hit CTRL + SHIFT + P to open the Command Palette and do most anything you want.

**Autosave.**&nbsp;Code can be configured to autosave your file as you change it like Atom does.&nbsp;Keep in mind, of course, that if you&#39;re&nbsp;using file watchers like gulp&#39;s .watch() method, this&nbsp;is going to&nbsp;trigger every time you type a character in your code.&nbsp;Autosave is off by default. To turn it on, hit&nbsp;CTRL + SHIFT + P and type auto.

**Search.** You can CTRL + SHIFT + F search over all files in the open folder and it supports regex. It ignores certain folders like node_modules by default since that&#39;s the right thing to do.

There&#39;s a ton more in Code, but that&#39;s all I&#39;m mentioning for now.

I hope you like your new home for code as much as I do so far.

&nbsp;