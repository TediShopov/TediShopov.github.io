---
layout: default
title: Teodor Shopov
description: Gameplay Systems / Computer Graphics Programmer
---

**Contact Me**
* Email : [teodorshopov01@gmail.com](teodorshopov00@gmail.com)
* Github : [https://github.com/TediShopov](https://github.com/TediShopov)

[More information about me](./about-me.html).

<!--
----- Should I even include this ones ---
# About this document 
**About this document** In this portfolio you can see the projects I am most proud of separated into two categories: **gameplay systems** and **computer graphics**.
%%Each project would deatail my responsibilites and most of the systems I have helped in developing. 
%%Most of the focus would be on explaning the technical and design decisions behind the project's more intricate/interesting systems.
%%Videos and image resource would be provided for all the projects.
-->


<!--
--- Foldable menus might be a good edition --- 
<details>
  <summary><h1>Click me</h1></summary>
  
  ### Heading
  1. Foo
  2. Bar
     * Baz
     * Qux

</details>
-->
<style>
.center {
  margin: auto;
  width: 75%;
}

</style>

# Gameplay System Projects

## Chempunch

Engine: **Unreal Engine**, Duration: **May 2024 To August 2024**, Team-size:  **8**, Role:  **Programmer**
The project became a Dare Academy finalist and was presented at **EGX X MCM 2024 LONDON**.


<div class="center"> 
<iframe width="560" height="315" src="https://www.youtube.com/embed/QnLPjNRj2Zg?si=e1Z3GXbthDfSVBGe" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

<div class="center"> 
<iframe width="560" height="315" src="https://www.youtube.com/embed/68KRv2RYLxA?si=9M80v7d6afHufgTS" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>


#### PITCH 
Chempunch is an intense wave defence action game focussing on reactive but aggressive gameplay in a chempunk world.
Alone but far from helpless, you must protect your slums from an oncoming hoard, using chemicals you route around your body to empower different actions.

#### DEVELOPED SYSTEMS 


* **Enemy AI System**  
  Designed a flexible AI framework utilizing Behavior Trees, Environment Query System and custom utility functions. Some enemies attack the objective, other defend those attacker and third type aggressively collapses on player.
  
* **Congestion-Aware Pathfinding**  
  Augmented Unreal’s built-in pathfinding with congestion tracking diversifying enemy pathing toward objectives. The system was proposed by me to reduce the workload from manually placed path routes by designer.

* **Dynamic Difficulty Scaling**  
  Exposed designer controllable scalars and animation curves to control how __player score__ is accumulated and how it applied to **enemy spawning ratio**, **tier** and **aggressiveness**.

* **Performance Optimization**  
  Used Unreal's Profiler and Memory Insights to identify and resolve critical bottlenecks in code, unoptimized assets and solve bugs. 

[See More --->](./Chempunch.md)



<!--

* Developed with **Unreal Engine** 
* From May 2024 To August 2024.
* Eight-person team
* Dare Finalist project. 
* Presented at EXG X MCM LONDON 2024.

#### Description
A first-person action packed wave-defense experience with some resource management mechanics or 
as the pitch goes:

> "Chempunch is an intense wave defence action game focussing on reactive but aggressive gameplay in a chempunk world.
>  Alone but far from helpless, you must protect your slums from an oncoming hoard, using chemicals you route around your body to empower different actions."

#### Role And Responsiblities
* Developed AI system for the enemies leveraging UE behaviour trees custom AI utility functions 
* Extended UE’s path finding system to accommodate for congestion, diversifying enemy pathing – and saving manual designer labor. 
* Programmed a designer controllable dynamic difficulty system 
* Profiled performance and memory usage, finding critical bottlenecks in code an unoptimized assets. 
-->


## Toot The Lute 
Engine: **Unity**, Duration: **Feb 2025 To May 2025**, Team-size:  **7**, Role:  **Project Lead & Programmer**

> Toot The Lute combines the isometric combat of Hades with the rhythm game system inpisred by Bullets Per Minute (BPM). 
> Click on the beats to performs dashes, attacks and accumulate combos. 

[See More --->](./TootTheLute.md)


## Ratking 
Engine: **Unity**, Duration: **Feb 2023 To May 2023**, Team-size:  **7**, Role:  **Programmer**
The game was created in accordance to brief provided by a Modern Wolf.

> Ratking is a stealth-platformer game in which you play as a rat trying to infiltrate a mansion of cats.


* **Navigation Graph Builder**
  Built a graph-based navigation system for 2D side-scrolling platforming, capable of handling rigid platforms, one-way platforms, 30° and 45° slopes, and ladders.
  Used by enemy patrol AI to traverse the level. Includes a custom in-editor visualization tool to preview nodes and debug graph generation issues.

* **Noise-based distraction/sensory system**  
  Designed and implemented a Mark of the Ninja–inspired noise system.
  Most in-game objects generate noise based on material type, collision force, and player interaction.
  Guards respond to sound sources, allowing the player to mislead them by throwing noisy objects. Conversely, running or dropping items carelessly can attract unwanted attention.

* **Projectile Trajectory Ghost**
Developed a fully simulated trajectory preview system for thrown objects.
Unlike standard parabolic previews, this system accounts for physics-based collisions and bounces, allowing players to plan noise-based distractions with high accuracy.

* **Classic Stealth Game State Machine**
Implemented a Patrol, Search and Chase state machine for the enemy guards. 
Additionally, once player is seen his position is propagated to nearby guards.




# Computer Grpahics

## Solving surfel over-spawning in surfel-based global illumination
Engine: **DX12/MiniEngine**, Duration: **June 2023 To Current**, Team-size:  **1**, Role:  **Graphics Programmer**

## Navigatable seascape with procedural terrain and destruction
* Developed with **Unreal Engine** 
* From May 2024 To August 2024.
#### Description

#### Role And Responsiblities

