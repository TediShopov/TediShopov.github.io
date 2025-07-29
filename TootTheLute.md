---
layout: default
title: Toot The Lute
description: An action rhythm battler combining Hades-style action gameplay with the rhythm system of games like Beats Pers Minute (BPM).

permalink: /TootTheLute.html
---

<link rel="stylesheet" href="site.css">

<script src="https://unpkg.com/lucide@latest"></script>
<script>
  document.addEventListener("DOMContentLoaded", () => {
    lucide.createIcons();
  });
</script>

<p>
  <svg data-lucide="cpu" class="icon"></svg>  Unity, WWise
  <svg data-lucide="calendar" class="icon"></svg>  Feb 2025 To May 2025
  <svg data-lucide="users" class="icon"></svg>  7
  <svg data-lucide="user-cog" class="icon"></svg>  Project Lead & Programmer

  <br>
  <svg data-lucide="gamepad" class="icon"></svg>
    <a href="https://somethingsmthn.itch.io/toot-the-lute" target="_blank">
      Toot the Lute on Itch.io
    </a>
</p>

#### Role And Responsiblities
Led a team of junior developers on their first project. Established scope, production workflow, and delivery schedule.  

*   Developed core **rhythm input system**, including **beatmap streaming**, synchronization to Wwise audio, and **dynamic callibration**.
*   Implemented combat mechanics: **rhythm-synced attacks**, **dashes**, and **interactable environemnt objects**.
*   Designed modular **boid-based AI** for enemies and boss.
*   Created visual polish **custom shaders**, **keyframe effects**, and **tweening**.
*   Integrated **dynamic transparency**, room/door logic, tutorial triggers, and environment interactivity.
*   Defined and enforced project scope, managed Git version control, integrated WWise.



## Rhythm System
At the core of Toot the Lute is a custom-built rhythm system that synchronizes fast-paced action gameplay with beat-based timing mechanics. Drawing inspiration from games like BPM and Crypt of the NecroDancer, this system transforms combat into a musical challenge, rewarding precision and timing.
The heartbeat of the game. All gameplay events are synced to beatmaps parsed from Wwise audio timestamps.

#### Beatmap
The representation of a beatmap is completely decoupled from the acutal soundtrack and
 strictly defined by repeating BeatmapSegments structs which are infinetely looping.
The BeatmapSegment struct is a list of Beats, each containing duration in count of beats, if the is a pause or not and the time to be repeated. 
The BeatmapSegment itself can be repeated many times. This created a fully customizable beatmap.
However, for the purposes of the game only a simple-most repeating beat is used and just the BPM is modified when changing tracks.

#### Beat Detection and Input Grading
The system detects beats in real time by parsing track BPM and aligning gameplay logic to musical timing windows.
When player input is detected, the system checks player input against the current beat window and determines if the input should be accepted or rejected.
This ties back to player attacks, dash and combo systems.

Additionally, the beats themselves have an event which is invoked once the beat is passed no matter successful or not. 
This allows environment object to have their visual, cooldown and effects all ties to the beat.

#### Visual Beat Track
To help players stay in rhythm, I implemented a visual beat track that displays incoming beats as markers approaching a central strike zone.
The track dynamically streams from the beatmap ensuring there are always enough beats to present on the screen.

Additonally, how fast beats are perceived to approach or how zoomed in the track is controlled by a **human-centered designed property** named
*VisibleBeatsAtOnce*. The bottom picture shows the effect on changing that property

 <img src="./TootTheLute_HumanCenteredDesign.png" alt="Toot The Lute Human Cetnered Design">




#### Technical Highlights

*   Synchronized Wwise playback via ElapsedTime instead of event callbacks to ensure frame-perfect input timing.
*   Efficient beatmap streaming with dynamic object pooling.
*   Modular, serialized beat segments allow infinite track looping.
*   Unity editor tools for beat visualization, boid debugging, and room/door control.


## Combat System
All actions must be timed to beats.
Attacks: Trigger visual and physical effects (colliders, particles, shaders).
Dashes: Invulnerability movement synced to rhythm.
Special Attacks: Reward perfect rhythm streaks with amplified damage and visuals.
Interactables: In-world objects (trees, bushes) that sync to beat and can be used in combat.

## AI Systems

Simple but expressive systems based on modular boid logic.
Includes: **Seek**, **Pursue**, **Flee**, **Separation** and **Custom Obstacle Avoidance** behaviours.
Integrated beat-timed attack behavior using Wwise beat events.


<div class="project-showcase">
  <div class="media">

<iframe width="560" height="315" src="https://www.youtube.com/embed/ozx9z03v268?si=hbtAgIE_ZfT9o-li" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

  </div>
  <div class="media">
<iframe width="560" height="315" src="https://www.youtube.com/embed/FBxBe7EE92Y?si=aj9wHN5kgL68q_pQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
  </div>
</div>

## Dynamic Transparency

Applies transparency to foreground objects blocking player view.
Based on distance and render layer.
Configurable range per object.
 <img src="./TootTheLute_DynamicTransparency.gif" alt="Toot The Lute Human Cetnered Design">


## Visual Polish
*   Shader graph was utilized to create the bottom layer (Shockwave) of the player attack effect.
*   Particle systems added game juice on plaers attacks and served as feedback on enemies hit/death.
*   "Faking" the perception on the enemy split was achieve by tweening blob and shadow size.


<div class="project-showcase">
  <div class="media">

 <img src="./TootTheLute_EnemySplit.gif" alt="Toot The Lute Human Cetnered Design">
  </div>
  <div class="media">
 <img src="./TootTheLute_PlayerAttack.gif" alt="Toot The Lute Human Cetnered Design">
  </div>
</div>



[back](./)
