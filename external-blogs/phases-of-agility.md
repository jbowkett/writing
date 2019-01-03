# The Evolution of Agile
As part of Excelian's delivery services team, we speak a lot about
continuous delivery, devops and its associated tooling.  Continuous delivery
is surely the pinnacle of today's agile team.  For many people being "agile" means
using "scrum".  However, I favour a more pragmatic approach.  After all, the 
agile manifesto values "individuals and interactions over processes and tools"*.
The blog post that immediately springs to mind whenever I think about the 
nature of agile is Dave Thomas' excellent post from 2014,
[Agile is dead, long live agility](https://pragdave.me/blog/2014/03/04/time-to-kill-agile.html)

_So what does it mean to be "agile" today?_

_Why have I seen so many different flavours of agile with varying degrees of 
success?_

I believe there are very different stages - albeit subjective - of how agile an 
organisation or software team is.

## Stage 0 - primordial soup - pre-agile
Not agile at all, possibly even actively hostile to it

## Stage 1 - hope springs eternal
Here we may be organising work into sprints, but possibly a "design sprint", 
followed by an "implementation sprint" then a "test sprint" and so on.  Alternatively 
what may actually be happening here is "mini waterfall".  In this stage people
are dividing up the project, but not adhering to any of the agile manifesto 
philosophies. 

## Stage 2 - maturation
The next stage of agile adoption is maturation.  Here the team may well 
be dividing up the work into stories, organising the work into sprints.  
There may also be the beginnings of automated testing, with some of the agile 
ceremonies observed (morning standups, possibly the odd retrospectve).

## Stage 3 - civilisation
Starting to add structure (in an agile way?!), to the agile project.  Automated 
test coverage is increasing, the agile ceremonies are in place (and 
working).  A backlog may be present (but perhaps it is a little neglected).  
Planning boards are clear and visible to everyone.

## Stage 4 - metrics & tooling
Building on good foundations, in this stage, teams are starting to 
collect statistics on their work - how fast are the team turning 
story points into code? How accurate were the original estimates? Are there 
differentials in different teams' work rates? How long does something sit in 
the ready queue/backlog? 

These metrics allow the team to create tight feedback loops and identify 
bottlenecks in their processes.  Tooling will be especially important here, 
nothing should get in the way of the developers cutting code (especially not 
"management overhead")

## Stage 5 - Continuous delivery
Once the bug for tooling has taken hold, it paves the way for continuous 
delivery.  Continuous delivery teams have a script to do everything - their 
environments are provisioned and torn down automatically, their builds and 
releases make their way through the pipeline automatically and ultimately 
their releases (which in advanced teams happen multiple times per day) and 
rollbacks are fully automated with little or no intervention.  

I've heard it said that highly regulated industries such as finance can't 
possibly allow automated releases.  This seems to be wholly at odds with 
their need for stability and reliability.  A script is more repeatable,
reliable and auditable than manual steps written in a document carried out by
a separate support team.

I wonder if phase 6 is actually #NoEstimates I'm sure Woody Zuill and 
Vasco Duarte would call this phase "enlightenment" :-) however I'm not convinced 
every software undertaking (I shy away from calling them projects in this 
instance as No Estimates goes hand-in-hand with "no projects"), is suited to 
sizing its stories identically, as some work items are atomic and need to be 
treated as binary - for instance, either the hardware is provisioned or it is 
not - there are no subunits.

For me, highly agile organisations are characterised by one thing - _change_.
And more specifically their attitude to change - they embrace and actively
encourage it.  This allows them to maintain their "agility" and therefore
shorten their time from code to delivering business value and this is all
underpinned by tooling and metrics that gives them insight into where they
can improve and further enhance their process and agility.  This creates _a 
virtuous spiral of improvement_ - the faster that team delivers, the faster 
they find out what's holding them back and the faster they can change 
something and so the faster they can deliver and so on.

At the end of the day, we don't need a model to tell us how agile we are, but
hopefully the stages above can help identify where you are and point the way
to next steps on the path to continuous delivery. 

Ultimately, agility is based on one important metric - how happy our 
stakeholders are that the right software is getting delivered at the right time.



* the software craftsmanship manifesto talks of "steadily adding value", but 
this is a whole different blog post!....