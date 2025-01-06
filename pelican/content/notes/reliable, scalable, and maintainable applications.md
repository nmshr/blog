Title: reliable, maintainable, and scalable applications
Date: 2024-12-03
Category: notes
Status: hidden



## Introduction

I had always wanted to read Martin Kleppmann’s Designing Data-Intensive Applications, or DDIA as known among developers. I tried to read it many times but never really read it cover to cover. In July 2024, Alex Petrov’s book club voted to pick it as their next reading. Phil Eaton who himself runs a popular book club, which I tried to join but failed, tweeted about it. I could not think of a better way of actually getting to read the book. I also decided to jump-start my blog by writing about the experience. The book club meets weekly to discuss one chapter each week. I thought I’d participate and summarise the chapter and the book club experience in my blog posts. So, here it goes.


### Chapter 1.

The first chapter sets the tone of the book and gives a peek into what follows, without being too technical. It begins by saying that most applications are data intensive. This is fairly true, any production-grade application today needs to span across multiple computers to serve its purpose. Martin describes how applications make use of databases, caches, search indexes, stream, and batch processing constructs. Developers are forced to make many choices between these systems to solve the specialized use cases of their domains. To navigate these choices, the author sets out to establish key important qualities according to him. These values are reliability, maintainability, and scalability.

#### Reliability

Reliability is described as the quality where the application must perform predictably within an expected degree of performance even when the conditions are not perfect. Faults and failures are described as inevitable conditions. Failure is defined as the event in which the system halts\ completely and stops working for the user. Fault is when a component deviates from the spec. A system is considered reliable when it can tolerate faults to a sufficient extent. 

Faults can occur due to hardware malfunctions, for example, disk failures which happen often.
Another example of faults, the book club came up with is CPU heating problems. 
Software faults are much more common and much harder to find out, this is known to every developer I believe. Here the book club could discuss several incidents caused due to a logical error, the most widespread one has been recently the Crowdstrike incident[1]. Software faults could be due to logical errors, or a program using too many resources in the machine making the overall system function in a degraded manner. Cascading faults cause a chain of events where one fault leads to many other faults. I have personally experienced this a lot in ETL workloads, where one bad event triggers a series of unwanted consequences.

Another source of faults and failures is human errors, technically even bugs are human errors. Apart from just bugs, oftentimes there are lapses in judgment. Who hasn't heard an incident report that involved an innocent accident where things were deleted or shut down unknowingly? This incident[2] came up in the book club discussions which involved an unsuspecting developer shutting an entire data center down.

The author then provides a list of tactics that are supposed to make applications reliable.
These involve finding the right abstractions and interfaces to stop the users from shooting themselves in the foot. Isolating production environments, with different stage setups in place. I have an example to share here from my workplace, where someone published crappy test messages like “hello?” to a production topic causing outages that were complex to recover from. Telemetry, having good management practices, and allowing for easy recovery with checks and balances like CI/CD pipelines are other such recommendations.

The author makes a great point about reliability not only being a requirement for mission-critical systems like a nuclear launcher, a simple tool like an online journal which a young adolescent might be using to note down their thoughts, is expected to not break because for them the journal might be their life’s work. If this example is not relatable for you, you are free to make up your own.

#### Scalability

The chapter then discusses another key tenet of data systems, scalability. It is defined to be a system’s ability to cope with increased load. Load is measured by requests per unit of time, to which an application is subjected. This could be due to an increase in the number of users or an increase in the volume of data being processed at any given time. The numbers which describe load are called load parameters and they differ according to the application. It can range from the number of concurrent users to the bit rate on a cache. Making an application scalable, involves making the right architectural choice, the author explains this by giving the Twitter example.

Load parameters work in tandem with a measure of performance, meaning that any change in load parameters is usually reflected in the application’s performance, which is measured by metrics like throughput or response time. Latency and response time are similar sounding metrics but according to Martin, latency is the portion of the response time where the request is just waiting in a queue.

A metric that has become important over the years is tail latency. The term tail latency can be understood simply from the distribution of response times v/s number of requests. Tail latencies are important as they have a compounding effect and they usually are caused by requests which add a lot of value. For example, the query on an OLAP spans across multiple years to populate the landing page of a dashboard.

Vertical and horizontal scaling are the two most classic ways of handling scale. Horizontal scaling gained a lot of popularity considered to be the cheaper alternative. In practice, a mix of both approaches work together. A smaller team would want to host their application on a lesser number of instances even if it means provisioning larger machines.

#### Maintainability

The next concern addressed is maintainability. This is commonly understood as how easy is to keep the lights on. This is further broken into three qualities of a maintainable system, operability, evolvability, and simplicity. Each of them is pretty self-explanatory. Most experienced developers would appreciate this. It’s not just good to have quality. It leads to make-or-break situations in software systems.

This chapter lays out the goals a data-intensive application sets out to achieve. The next chapters will cover how applications achieve them.

#### References

[1] [https://www.crowdstrike.com/blog/falcon-content-update-preliminary-post-incident-report/](tab:https://www.crowdstrike.com/blog/falcon-content-update-preliminary-post-incident-report/)
 
[2] [https://www.youtube.com/watch?v=sdDEwqUnOfs](tab:https://www.youtube.com/watch?v=sdDEwqUnOfs)

