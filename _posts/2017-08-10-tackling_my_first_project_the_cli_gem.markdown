---
layout: post
title:  "Tackling My First Project: The CLI Gem"
date:   2017-08-10 20:56:48 +0000
---


For my CLI Gem project I decided to work with the FIFA world rankings. While coming up with the idea for my project came rather easily, actually starting my first project was daunting, I definitely procrastinated some and wrote a ton of different lists/outlines to help organize my thoughts. Eventually I realized that I needed to stop dipping my toe in the water and just dive in. Watching Avi’s walkthrough video was super helpful because it provided a roadmap for the project - where (and how) to start, important considerations, steps to take, and reminded me that it’s okay not to know everything.

#### Making a plan 
To start, I spent some time with my chosen websites (all from Wikipedia) and the Chrome Inspect tool. Early on, I needed to ensure the feasibility of scraping the sites and that it offered enough depth of information to make it interesting. Next, I focused on the basics of the user experience, of which the first pass looked something like this: 

```
- User starts program
- Greet user and ask if they want to view the Men's or Women's rankings
- Show the list of the 20 teams and basic stats: rank, number of points, change in position (up/down/none)
     Rank - Team - Points - Change
     1. - Germany - 1609 - Up
     2. - Brazil - 1603 - Down
- Ask the user which team they would like to learn more about.
- Receive user input and display info about team. Team info available: Description, Association, Nicknames, Confederation, Head Coach, Captain, Most caps, Top scorer, FIFA Code
```

#### Putting the plan into action
Taking the user experience that I had designed, I set out to actually start writing Ruby. In order to stay focused and not get bogged down by complexity, I stubbed out my interface using fake data  (aka hard coded data). What I learned from this process is that the key here was just to get something down on paper. Simply start by writing the code that achieved the end goal, even if it wasn’t the prettiest. It is easy to get hung up on whether or not it was the best code you have ever written. Thinking back on it, I’m glad I pushed myself to work in this manner, at least at the beginning of my project, because it allowed me to make forward progress and I wound up refactoring a lot of my initial code. 

#### Scraping 
After putting together my program, the next step involved moving from the hard coded data to scraping data from the Wikipedia pages. To avoid web-crawling issues, I saved down the HTML file for each page I needed and used those to test and determine which selectors I needed. I can honestly say that scraping was the most taxing part of my project. The pages I used from Wikipedia are not standardized across the board. Some team pages would work perfectly and others would return errors even though it appeared they all used the same selectors. The one issue  that really had me scratching my head was when I would use the appropriate selectors but get back `nil` or `NoMethodError` for data that I could see in the HTML file. The solution I devised was to add conditionals to my Scraper class methods in order to properly handle situations where the data differed slightly. After successfully scraped the data from the saved HTML fixtures, I switched to scraping from the live web pages. My biggest takeaway here was that within `<table>` elements, browsers sometimes insert a `<tbody>` element, but it does not always exist in the actual source<sup>[1](http://ruby.bastardsbook.com/chapters/html-parsing/)<sup>. 

#### Final Thoughts
I found that as I moved through my project, I worked in somewhat of an iterative process that went something like this: 
Write out what I want in plain English
Begin to translate that into my program using pseudo code and fake data
Slowly start replacing pseudo code with Ruby and fake data with scraped data
Test, break and make things prettier
Fix what’s broken
It sounds odd, but attempting to tweak things, breaking them and then fixing it again proved more satisfying than frustrating. In the end, when it all comes together, it is very rewarding. 

*A note about the scope: The original scope for my project sought to provide the user with the top 20 Men’s and Women’s FIFA teams. The user would select one of two different options (M or W) and then learn about more about a specific team within that list. I worked through a lot of my program before deciding to narrow the scope to just the Women’s teams. 

The source code is available [here](https://github.com/sydra08/fifa_rankings).



