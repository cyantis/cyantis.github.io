---
layout: post
title:      "Invisibility: an Adventure in Scraping, Regex, & Everything In Between"
date:       2019-04-13 18:27:45 +0000
permalink:  invisibility_an_adventure_in_scraping_regex_and_everything_in_between
---


I just wrapped up my first CLI project, which was equal parts challenging and engaging. [Best Music](https://github.com/cyantis/best_music) is a gem that scrapes [Pitchfork.com](https://pitchfork.com/reviews/best/albums/) and returns the previous 12 weeks’ worth of “Best New Albums.” You can browse by genre or score as well as generate a chronological list of albums. It was a ton of fun to build, and I actually intend on firing-up the program weekly to check out new music without the hassle of having to click through to each album review page for scores and descriptions.

While building my program, I encountered some of the usual pain points: narrowing the project scope when you start coding and realize you were way too ambitious with your initial idea; all the little things that took _forever_ to figure out but mere seconds to actually fix; the the brainpower, sheer will, and cups of coffee consumed while figuring out what “has many” and what “belongs to” what. But rather than writing at length about those, I want to highlight one aspect of my program that unfolded rather mysteriously. What follows is a tale of regex, scraping, invisible characters, and more!

Using [Nokigiri](https://nokogiri.org/) to scrape the Pitchfork.com data took some time, but, all-in-all, it was a fairly straightforward process: after a decent stint of trial and error with [Pry](http://pryrepl.org/), I had the necessary data stored away in my program's class structure. The thing is, the data was kind of dirty, and, while this didn’t break my program, I wasn’t about to let these details sully my hard work.

This visual mess stemmed from characters returned in the Nokogiri scrape via the `text` method, and, for the most part, I took care of them with `gsub`. But one case was downright baffling: the character `â` was showing up in place of apostrophes as well as em dashes. I first noticed this when I wrote a `gsub` to replace `â` with `’`. That worked for any possessive in my text, but it looked rather silly when the words before and after an em dash were suddenly contracted into a meaningless new term. And here my journey into _invisibilia_ began…

Since a simple find and replace wouldn’t fix this issue, my first thought was that an apostrophe will almost always be followed by an `s` and then a space. So, I tried using [regex](https://en.wikipedia.org/wiki/Regular_expression) to match that three character pattern (`âs `), but no luck. I then went down the rabbit hole of regex _lookbehind/lookahead_ functionality as well as a range of conditionals, but none of this helped fix the issue. As it began to dawn on me that this was likely going to be a matter of sheer brute force trial and error, I figured that, rather than hanging out in Pry and continuously executing my program, an online [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) like [Rubular](https://rubular.com/) would be easier to experiment with.

It was when I posted some sample return text into Rubular, that I realized the `â` character had an accomplice! It was followed by the `€` character as well as what looked like additional white space. For example:

_On his first ever collection of original solo `songsâ€ `   a companion piece to his recent `memoirâ€ `  the Wilco leader uncovers himself like never before._

Since the `€` and additional whitespace didn’t display in my terminal, I’d had no idea they were even there.

I messed around in Rubular until I got the pattern to match which, oddly, required using the regex `\S` (_non_-whitespace) to match what sure looked to me like whitespace. I then went back to Atom to apply this to my code, but quickly noticed that the  `€` character was missing in my `.rb` file, even though I had pasted it there. Stranger still was that there _was_ a character there, because, as I arrowed forward or backward where the character should be, the cursor paused and the character count in Atom changed. Cue: [twilight zone music](https://www.youtube.com/watch?v=XVSRm80WzZk).

I went ahead and ran my program and, lo and behold, it worked as envisioned. Still weirded out by the invisible `€`, I removed the character from my source code, ran my program again, aaaand...it no longer matched! By this point, I’d had enough of this madness, so I pasted the `€` back in and left it. Here’s my `gsub` chain for this particular operation:

`.gsub(/â(?=\Ss)/, "'").gsub("â", "--").chomp`

(In Atom the `€` after each `â` doesn't display in my `.rb` file.)

So, there ends--happily, if a bit confusedly--my first real foray into the weeds of scraping, regex, and invisible characters. I’d love to hear from those of you who have far more experience with this kind of stuff, as I still have a range of questions. What causes this? Are there best practices for cleaning-up these types of issues? Is the `` character actually an invisible character or just something certain programs can/can’t display?

Now, off to hoist a celebratory pint with [Rod Serling](https://en.wikipedia.org/wiki/Rod_Serling).
