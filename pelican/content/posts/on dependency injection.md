Title: on dependency injection
Date: 2025-01-21
Category: posts



An outage broke out in production recently. The team scrambled for almost a week to douse the fires. Our systems use object storage to store data. The object storage access is secured by secrets. These secrets have expiry dates. The secrets were packaged along with the code.

A senior in the team made an observation, “This is classic non-following of dependency injection principle…”. I agreed with that assertion. But nowadays, I tend to double-check my understanding of words, especially if they are idiomatic with capital letters. So I thought I would examine the claim a bit. What is dependency injection, really? I vaguely understood it as externalizing configurations. An LLM pointed the origins to a 2004 Martin Fowler blog [post](https://martinfowler.com/articles/injection.html). 

 *“Underlying these containers is a common pattern to how they perform the wiring, a concept they refer under the very generic name of “Inversion of Control”. In this article I dig into how this pattern works, under the more specific name of “Dependency Injection”, and contrast it with the Service Locator alternative.”*

Note, how skeptical Martin is, about the generic nature of the phrase “Inversion of Control”. I wanted to enter the rabbit hole of finding out its origins too. The LLM revealed the source to be a thesis published in 1996 by Michael Mattson. 

*“The major difference between an object-oriented framework and a class library is that the framework calls the application code. Normally the application code calls the class library. This inversion of control is sometimes named the Hollywood principle, “Do not call us, we call You.”*

Note, Michael is not trying to formalize the silly phrase “Hollywood principle” into something more professional and white-collar like “Inversion of Control”. He did not capitalize the phrase.   
It’s likely, from hereon someone likened the formal nature of this term and appropriated it as the name of a hot trend. 

Martin Fowler traced the etymology further back in time in 1988\. An article titled, Designing Reusable Classes in the [Journal of Object-Oriented Programming](http://www.sigs.com/html/publications.shtml#joop) mentions the phrase.  
Here as well, it is not used as a proper noun.

*“One important characteristic of a framework is that the methods defined by the user to tailor the framework will often be called from within the framework itself, rather than from the user's application code. The framework often plays the role of the main program in coordinating and sequencing application activity. This inversion of control gives frameworks the power to serve as extensible skeletons. The methods supplied by the user tailor the generic algorithms defined in the framework for a particular application.”*

Anyway, circling back to what is Dependency Injection? I will use the acronym DI for brevity, Martin in his 2004 blog post, adopted this phrase to be more specific about the popular Inversion of Control patterns. 

*“There are three main styles of dependency injection. The names I'm using for them are Constructor Injection, Setter Injection, and Interface Injection.”*

He leaves no ambiguity in defining that DI is concerned with fulfilling objects’ needs for other objects, by having a separate set of assembler objects aware of these needs. These needs can be put together in configuration files for sufficiently large projects.

Having a secret in a properties file or an external store is an instance of another pattern, sometimes called, Externalising Configurations, more popularly covered in [The Twelve-Factor App](https://12factor.net/).

In software, practitioners jump on the hype wagon and paste important-sounding names to sometimes useful patterns. Over time, I have learned to peel off the layers and never take my understanding of these concepts for granted. Quoting Phil Karton,

*“There are only two hard things in Computer Science: cache invalidation and naming things.”*  
