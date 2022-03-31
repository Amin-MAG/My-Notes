# Code Review

# SOLID Principles

As with all the other areas we’ve covered, not all teams will prioritize this as the highest value area to check, but if you are trying to follow SOLID Principles, or trying to move your code in that direction, here are some pointers that might help.

## SRP - Single Responsibility

There should never be more than one reason for a class to change.

> This was a summarization of `What to Look for in a Code Review`
> 

# Security

How much work you do building a secure, robust system is like anything else on your project. (It depends)

When reviewing code, at the very least you want to check if any new dependencies (e.g. third-party libraries) have been introduced.  If you aren’t already automating the check for vulnerabilities,  you should check for known issues in newly-introduced libraries.
You should also try to minimize the number of versions of each library – not always possible if other dependencies are pulling in additional transitive dependencies. But one of the simplest ways to minimize your exposure to security problems in other people’s code (via libraries or services) is to
•   Use a  few sources as possible  and understand how trustworthy they are
•   Use the  highest  quality library  you can
•   Track what you use and where,  so if new vulnerabilities do become apparent,  you can check your exposure.
This means:
•   Understanding  your  sources  (e.g.  maven  central  or  your  own  repo  vs  arbitrarily  downloaded jar  files)
•   Trying  not  to use 5  different versions  of  3  different logging frameworks (for example)
•   Being  able  to  view your dependency  tree, even if  it’s  simply through Gradle/Maven

# References