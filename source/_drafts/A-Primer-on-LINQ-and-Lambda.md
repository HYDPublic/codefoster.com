---
title: A Primer on LINQ and Lambda
tags: []
date: 
---

# Introduction

I still remember the day well. I had heard a ton of buzz about LINQ statements and lambda expressions, so I knew they were pretty big deals, but I didn&rsquo;t have the concept. I didn&rsquo;t know why they were important or when. In short, I didn&rsquo;t know what problem they solved.

I looked all over the internet for a good description of the why/when...; for the problem statement...; for the concept, but I didn&rsquo;t really find it. I found enough information to extrapolate the concept in rather short order, but I&rsquo;d like to write a primer on the composite topic of LINQ and lambdas because it&rsquo;s what I would have like to have seen.

This is just a primer and not an exhaustive introduction. There&rsquo;s plenty of information on the web and I&rsquo;ll include some links to get you started digging further, but this is just a brief attempt to relay the concept and get you started.

Here goes.

# **LINQ**

## The problem statement

_LINQ exists because we do a lot with lists of data in a huge variety of forms._

LINQ is essentially a collection of extension methods that act on lists. It&rsquo;s that simple. These methods filter lists, sort lists, and in many other ways they manipulate the individual items in a list. The cool thing is that any list (any list that is IEnumerable technically) can be &ldquo;LINQed&rdquo;. Furthermore, the extension methods can be overloaded to work not only on local collections such as lists and arrays, but they can also act on an endless number of more conceptual collections. So anyone can create a LINQ provider and allow developers to interact with their collections through this common set of methods. The Wikipedia page on LINQ includes a good list of providers. Some I find interesting are: LINQ to Twitter, LINQ to Wikipedia, and LINQ to CSV.

The best place to go to learn about LINQ is [101 LINQ Samples](http://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b). But know this...; when they created LINQ, they created a whole new way of writing the code. It&rsquo;s called the LINQ query syntax and it feels a little like you&rsquo;re writing SQL. But everything you write in LINQ can either be written using this LINQ query syntax <span style="text-decoration: underline;">or</span> it can be written using extension methods. The 101 LINQ Samples writes everything in LINQ query syntax, but I all but never use this syntax anymore. I strongly prefer the extension method syntax myself.

So these are equivalent statements that filter a list of states for the ones that start with the letter &lsquo;a&rsquo;:

**LINQ Query Syntax:**

<pre class="brush: c-sharp; toolbar: false">
var statesThatStartWithA = from s in States where s.Name.StartsWith('a');</pre>

**LINQ Extension Method Syntax:**

<pre class="brush: c-sharp; toolbar: false">
var statesThatStartWithA = States.Where(s => s.Name.StartsWith('a')); </pre>

# Lambda Expressions

## The problem statement

_Lambdas exist because delegates can be cumbersome._

Dynamic languages like JavaScript make handling and passing functions a beautiful thing. A function can be passed around just like a variable. The same can be done in C# using delegates, but it&rsquo;s a pain in the butt. You have to declare the delegate and whoever uses it needs to have visibility to this definition.

Lambda expressions are simply anonymous functions. They&rsquo;re just a really shorthand way of writing a function without even giving it a name.

Notice in the code above where we filter out the states that begin with &lsquo;a&rsquo; using the extension method syntax that the Where method takes a single parameter and that parameter is a lambda expression...;

<pre class="brush: c-sharp; toolbar: false">
s => s.Name.StartsWith('a')</pre>

This is awesome because functions are powerful. In this case we use s => which means that our function takes in a single parameter (which we know is going to be the type of whatever item types exists in our list...; in this case States) and it returns a Boolean value of true if the state starts with an &lsquo;a&rsquo;. If we did this (which would be silly of course)...;

<pre class="brush: c-sharp; toolbar: false">
var states = States.Where(s => true); </pre>

...;then we would get all states. They would not be filtered at all.

Notice also that our function doesn&rsquo;t have any mustaches around it, doesn&rsquo;t have a semicolon, and the &ldquo;return&rdquo; keyword is omitted. It doesn&rsquo;t say s => { return s.Name.StartsWith(&lsquo;a&rsquo;); } . This is another feature of lambda expressions. If you&rsquo;re entire function is a single statement and evaluates to the return type that the lambda expression is expected to return, then the typical adornments can be omitted and it works. Know, however, that you are welcome to add the adornments and thus grant yourself the ability to get all sorts of fancy with your function. You can, for instance, do a few steps imperatively before you return a value. Let&rsquo;s look at what that might look like although the following example is again rather silly except that it illustrates my point...;

<pre class="brush: c-sharp; toolbar: false">
var statesThatStartWithA = States.Where(s => {
	char letter = 'a';
	bool result = s.Name.StartsWith(letter);
	return result;
});</pre>

Lambda expressions work great with LINQ statements, but their usefulness by no means ends there. Remember, it&rsquo;s just a shorthand way of writing a function, so you will find them all over the place.

That probably plenty of depth for a primer, but dig into more depth in the links I have included below.

# Resources

[101 LINQ Samples](http://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)

[MSDN: Introduction to LINQ](http://msdn.microsoft.com/en-us/library/bb397897.aspx)

[Wikipedia article on LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query)

[MSDN: Lambda Expressions (C# Programming Guide)](http://msdn.microsoft.com/en-us/library/bb397687.aspx)

[Wikipedia article on anonymous functions](http://en.wikipedia.org/wiki/Anonymous_function)

[Practical Introduction to Lambda Expressions](http://www.rvenables.com/2009/03/practical-introduction-to-lambda-expressions/)