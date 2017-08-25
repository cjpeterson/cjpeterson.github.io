---
layout: post
title: "The Problem With SaltyBet"
date: 2017-08-24 11:24 -0600
categories: other
---

In mid-2013, a twitch user by the name of Salty came up with an idea for a stream where viewers could bet meaningless virtual currency on fights between computer players. The fights would take place in MUGEN, an open-source fighting game engine. Thus, SaltyBet was born.

<a href="{{site.url}}/assets/images/Salty/SaltyBet Main.png"><img class="centerimg" src="{{site.url}}/assets/images/Salty/SaltyBet Main.png"></a>
<p class="caption">The SaltyBet website</p>

### The Gameplay

The concept is incredibly fun at first blush. You're given an initial pot of money, and you can never go below 10 dollars (cause it's all in fun). Every round, you are shown the two characters who will be fighting each other (chosen from a pool of thousands of custom characters), and you can bet any amount of your money on one of the two characters. They fight it out in a best of three, and if your character wins, you get money, based on the odds. If you lose, you lose your money.

The characters in the game are all made by the MUGEN community, and they come from everywhere. They are everyone from Goku to Tiger Woods. From Mario to Ultraman. The betting is almost an excuse to watch various characters from your childhood battle it out in an arena.

The characters range greatly in power level, with some having ridiculous health and abilities, while others are more sensible. This meant that initially, most matches had a clear winner from the start. The stream has been refined over the years, however, and the fighters are stratified into a few classes, roughly according to their power level. This makes it a little tougher to predict who will win.

### The Betting

Your winnings are based on the odds of the match, which are hidden until betting is over and the fight starts. This makes it different from traditional odds gambling, which makes sense. In traditional odds gambling, the bookie is supposed to win. At SaltyBet, good bettors are supposed to win. That's what makes it a game people are going to come back to.

Well, not only are the odds hidden until the betting is frozen, they aren't even calculated until then. The odds are merely a reflection of the money pools. If a total of $1M was bet on Fighter A, and $2M was bet on Fighter B, the odds are set at 1:2. If you bet on Fighter A, and he wins, you'll get back the money you placed + itself + itself.

### Strategizing

If you play SaltyBet, you'll quickly find yourself losing all your money. The game may not have been designed as a moral lesson against gambling, but it sure feels like one. Once you place a bet, you get to see the odds and find out if you messed up big time. If you bet $100 and the odds are 1:100 against, you can kiss that money goodbye. Every once in a while there's an upset, which is far and away the most exciting aspect of the game, but those are necessarily rare, happening only a few times a day.

<a href="{{ site.url }}/assets/images/Salty/Salty Clutch.webm"><video class="centerimg" width="640" height="360" autoplay loop><source src="{{ site.url }}/assets/images/Salty/Salty Clutch.webm" type="video/webm"></video></a>
<p class="caption">A minor upset</p>

So, placing a large bet is always risky, if you don't know the fighters. On the other hand, placing a small bet isn't fun. If you bet $100 and the odds are 100:1 for, you can expect to win a shiny new dollar. This is actually the least amount of money you can win, and you would've gotten the same return with less risk by betting $1. Betting at SaltyBet always feels high risk, low reward. This isn't particularly fun. A friend I spoke to chalked this up to the failings of gambling (moral lesson successful), but that doesn't seem right to me. This isn't gambling, it's a game. You should be rewarded for skill. So, let's strategize.

While I was taking an online data analysis course, I was taking a break watching SaltyBet, and thought about how I might go about predicting the winner of a match. You would need a dataset, first off, which I happened to find online. There are many people who've turned to data analysis and botting to beat the Bet, and one of them very generously provided a dataset for it.

The simplest thing to do would be to consider total wins v total losses to get a proportion. Then, with two fighters, you have two proportions. The larger one has a higher win rate, so you should bet on him. This was very straightforward. The hardest part was creating an interface so I could look up two characters by typing their exact names within 30 seconds (the betting time). So, did it work?

To my mind, somewhat. It would've taken way too long to do a proper analysis on the success of the strategy, but it felt like I was winning more often.

### The Problem

I wanted to know how well you could do by accurately measuring the odds. For each given matchup, it can be boiled down to a probability. If the same matchup were played thousands of times, Fighter A would win P% of the time, and Fighter B would win (100-P)% of the time. Fighter A wins with probability p, Fighter B with probability (1-p). Supposing I know p, what's my edge over someone who is betting randomly?

Well, suppose the odds are set exactly like p: e.g. if p is .9, then the odds are set at 9:1. Fighter A will probably win. If I bet $100 on Fighter A, I have a 90% chance to gain $11 and a 10% chance to lose $100. The expected return is 0. Contrarily, if I bet $100 on Fighter B, I have a 10% chance to gain $900 and a 90% chance to lose $100. My expected return is, again, 0.

This principle holds true always: if the gambling odds are the same as the true odds, the expected return is 0, no matter who you bet on<sup>1</sup>. It was my assumption that betting on the more favored fighter was always to your advantage, but I didn't stop to analyze the strategy.

A further analysis will show that if the gambling odds are not set at the true odds, it is always to your advantage to bet on the underestimated fighter. If the true odds are 10:1, and the gambling odds are 20:1, you'll want to bet on Fighter B. But of course, you don't know the gambling odds until the betting is over. **The only way to gain a strategical edge is to estimate the crowd's expectation of the fight.**

This is not a fun mechanic. You have to acquire a lot of domain knowledge to estimate the outcome of the fight, and then the difficulty and skill of that is completely overshadowed by your knowledge of who is watching and what they know about the game. I want to learn a lot about the fighters. I think that's where the skill should be. I don't want to learn about JaffaCakes69 and how he watches 12 hours a day, but isn't botting.

Now, you would do well to assume that most players have little understanding of the fighters, and that the betting odds will always tend closer to 1:1 than the true odds, meaning that it is typically to your advantage to bet on the better fighter, but all of this is haunted by your understanding that if everyone executed basic strategy, the betting odds would be the true odds, and it wouldn't matter who you bet on.

The problem with SaltyBet is that it seems like a game where you win by understanding the fighters, but it's actually a game where you win by understanding a crowd of your peers who have a strong incentive to lie to you.

### A Solution

When designing games, you typically want to look at how you wish the game to proceed, and then tweak the mechanics to make that happen. I think the game should proceed with players rapidly gaining money and then losing it all. They should have longer winning streaks by having better domain knowledge, and there should be the occasional upset that wins them tons of money.

The best players will always follow optimal strategy to a T, even when this strategy isn't fun. In the case of a simple system like ours, the optimal strategy is to use the Kelly Criterion. The Kelly Criterion defines what percentage of your bank you should wager, and the formula looks like this:

<img class="centerimg" src="{{site.url}}/assets/images/Salty/Kelly.png">

Here, p is the probability of winning, b is the multiple of your bet you will receive on top of your wager if you win, and f is the proportion of your money you should bet. A cursory analysis will show that as you raise either b or p, you raise f, and if you lower b or p, you lower f. f can be negative, which indicates that you shouldn't place a bet.

Now, with our particular setup, b is symmetric. This wouldn't happen with a real bookie, unless he was taking a flat percentage of the profits. Given this, we can show that if a particular bet would give us a negative f, we should place the opposite bet with a bankbook proportion of -bf. What this means is that if a bet is bad, the opposite bet is good. The only instance where both bets are bad are when f is zero (as is the case when the odds match the probability).

This is bad news. We want to encourage people to bet, but we also want to ensure that people lose now and again. Here, either there is a good bet, or they will abstain.

My proposal is that the player be forced to place larger and larger bets, depending on their bankroll. If you have $10 in the bank, you can bet any amount. If you have $1000, you have to bet at least 5% of your money. If you have $1,000,000, you've gotta bet at least 50%.

This should drastically change the nature of the game. At lower dollar amounts, people would place smart bets to build up their bankroll, and at larger amounts, people would be forced to choose between freezing their assets and gambling for high scores.

The beauty of this is that you're now free to adjust the odds to favor big wins. On a 90% match, you can set the odds at 1:1, and most people would double their bet most of the time. On a 99% match, you can set the odds at 1000:1, and most people would abstain, but a few people would take the risk of reaping the reward from an upset. Big wins, big losses, and the only way of ensuring a long winning streak is knowing the fighters.

I'd play it.

* * *
1. The only edge you can gain is to play the round-off error by betting $1 on the more favored fighter.
