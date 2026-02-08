---
layout: post
title:  "Exploring Unreal Engine 5"
date:   2025-12-10
categories: unreal 
lang: en
---

I've wanted to learn Unreal Engine 5 for quite a while and a few months back I decided to jump in by making a game or something resembles a game. I gave myself a list of things to do :

1. Spawn enemies
2. Player shoots projectile
3. Damage and health system + simple UI
4. Special effects (Niagara system)
5. Enemies AI

## Enemies

To spawn enemies, I didn't do much. I created a blueprint that spawns in an Actor blueprint that represents an enemy.

![](/assets/unreal/2025/12/spawner_blueprint.png)

To represent an enemy is this geometric guy, my attempt at modeling in Blender. It took far too long to create but as someone who had never used Blender, except the classic donut tutorial, I'm pretty proud of myself.

I didn't implement any AI because when I looked into blackboards and behaviour trees, it looked heavy and daunting.

<img src="/assets/unreal/2025/12/robot.png" width="300">


## Aiming and shooting

Most of my time on the project was spent on this aspect of the game. My first attempt was wonky. I had to stop moving to shoot or else the projectiles would shoot in the wrong direction. The camera didn't really aim. Nothing really worked.

<video controls src="/assets/unreal/2025/12/first_aiming.mp4"></video>

Then I improved the aiming for when the player is standing still. I took inspiration from the games Control and Rise of Rain 2 for the aiming behavior. When the player turns the camera in a direction and shoots, the character turns toward that direction and shoots. When the action of shooting is done, the orientation of the character is locked to the direction of the camera for an amount of time, then the camera goes free. I also added a small cursor, but the bullet trajectory doesn't align with it.

<video controls src="/assets/unreal/2025/12/second_aiming.mp4"></video>

I struggled trying to understand how timers and all the different versions worked in blueprints. I started with the "Set Timer by Event". I spent a lot of time debugging why my custom event was being fired every frame when connected to the completed state of my mouse click input action. I saw some posts on the UE forums that used the "Set Timer by Function Name" and everything worked perfectly. Well almost. I had to plug the "Sequence" node on the Tick event to update the orientation of the character in sequence of what triggered it. Shooting or mouse movement.

![](/assets/unreal/2025/12/aim_blueprint.png)


## Special Effects

In my research on how to fix the alignment between the projectile and the reticle, I found out that most shooters don't shoot any projectile out of the gun, but instead do some ray casting and VFX to give the feeling of shooting the gun. So I tried doing that.

The ray casting was fairly easy, just shoot a ray from the center of the camera and check if it hit anything. The most complex part of all this was the VFX.

I wanted to create my own effects to learn how they were made and learn the basics of the Niagara particle system. I found a [YouTube tutorial](https://www.youtube.com/watch?v=SGoNF1UTD3I) on how to make from scratch a gun muzzle flash. I had to pull out Blender and GIMP. For someone that is still not that comfortable with that software, it took a while to create this simple effect. With what I learned in the tutorial I was also able to create some kind of bullet hit effect.

<video controls src="/assets/unreal/2025/12/sfx.mp4"></video>


## Damage and Health


For the health part of the project I created a simple Health component in C++ that contains 3 values : the base health, the armor and the energy shield. All those values are updated in a method called `TakeDamage`. That method is called when the built-in `AnyDamage` Event is triggered. Then I update the UI component (The UI update could've been in its own method here). The UI component is simply three overlaid progress bars.

![](/assets/unreal/2025/12/health_blueprint.png)

![](/assets/unreal/2025/12/health_bar.png)

The `AnyDamage` Event gets triggered when I call the Actor class method `TakeDamage` from my C++ component `WeaponSystem`. This component does the line trace and if it detects a hit on an Actor it calls `AActor.TakeDamage`.

![](/assets/unreal/2025/12/damage_code.png)


<video controls src="/assets/unreal/2025/12/health.mp4"></video>

## Conclusion

This project was a great first dive into Unreal Engine 5. I got to touch on a bit of everything — blueprints, C++, Niagara, Blender, and even some UI work. The biggest takeaway is that things rarely work on the first try, and that's fine — most of the learning happened while debugging.