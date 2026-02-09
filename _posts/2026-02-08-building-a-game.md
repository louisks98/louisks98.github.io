---
layout: post
title:  "Building a game"
date:   2026-02-08
categories: game dev
lang: en
---

At the end of last year I lost my job so I found myself with a lot of free time and with the start of the new year it's the perfect time to start a new project. One of my favorite genres of games are Rogue-like. Hades and Hades 2 are easily part of my top 10. I like the infinite replayability and the multitude of builds that you can make. In a more general way, Sci-fi is also a genre that I have great interest in. I'm a fan of Star Wars since I was a child. I loved the anime Space Opera series "Legend Of The Galactic Heroes" and 3 years ago I started watching the "Gundam" franchise. One of the common points of all of these are the flashy space battles. So I had this idea of making a rogue-like game themed around those space battles.

So I started building. Opened Unreal Engine 5, created a new project, added a player controller and a star fighter model. Then I hit the first hurdle.

The problem was what kind of controls did I want for this game. I had never played a game where you fly a plane/space-ship. I looked for examples. Battlefield 6, GTA 5 and Ace Combat 4. I played through the tutorial and the first few levels of Ace Combat 4. I didn't like how the controls felt. They were too stiff. I looked at gameplay for jet flight in Battlefield 6 and GTA 5 and found that those were not really what I wanted. They were in a way too complicated, too many inputs. I wanted a control scheme that's more like a 3rd person shooter. I asked Claude Code to brainstorm some ideas and it came up with a system where the fighter goes toward where the cursor points. I added w/s for throttle controls and a/d to increase the turning speed. It's not perfect and it's probably subject to change but it's enough to start with.

<video controls src="/assets/game_dev/demo1.mp4"></video>

Then for the guns, missiles, health I reused the components I created in a previous [UE5 project]({% post_url 2025-12-16-exploring-unreal-engine %}). I did have a small problem with the missiles. I was using `UGameplayStatics::ApplyRadialDamageWithFalloff()` to create a blast area of damage but my enemies would never get damaged. I found out online that there was a 'bug' in how the hit scanning was calculated for radial damage. If the impact point is at floor level or under, the hit isn't registered for actors inside the radius. A simple fix to this was to add a small offset to the height of the impact point.

<video controls src="/assets/game_dev/demo2.mp4"></video>

And this is the current state of the game. Stay tuned for other updates.