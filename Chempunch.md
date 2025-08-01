---
layout: default
title: Chempunch
description: A first-person wave-defence action game.
permalink: /Chempunch.html
---

<link rel="stylesheet" href="site.css">

<script>
  document.addEventListener("DOMContentLoaded", () => {
    document.querySelectorAll('.expand-toggle').forEach(button => {
      button.addEventListener('click', () => {
        const expandable = button.closest('.expandable');
        expandable.classList.toggle('open');

        const arrow = button.querySelector('.arrow');
        if (arrow) {
          arrow.textContent = expandable.classList.contains('open') ? '▴' : '▾';
        }
      });
    });
  });

</script>

<script src="https://unpkg.com/lucide@latest"></script>
<script>
  document.addEventListener("DOMContentLoaded", () => {
    lucide.createIcons();
  });
</script>
<p>
  <svg data-lucide="cpu" class="icon"></svg>  Unreal Engine,WWise  
  <svg data-lucide="calendar" class="icon"></svg>  May 2024 – August 2024  
  <svg data-lucide="users" class="icon"></svg>  8  
  <svg data-lucide="user-cog" class="icon"></svg>  Generalist & AI Programmer

  <br>
  <svg data-lucide="award" class="icon"></svg> Dare Academy Finalist  
  <svg data-lucide="map-pin" class="icon"></svg> Presented at <strong>EGX x MCM 2024 London</strong>
</p>


#### Description


<!--
A first-person action-packed wave-defence experience with some resource management mechanics or 
as described in the game's pitch:

> "Chempunch is an intense wave defence action game focusing on reactive but aggressive gameplay in a chempunk world.
>  Alone but far from helpless, you must protect your slums from an oncoming horde, using chemicals you route around your body to empower different actions."
-->




<div class="project-showcase">
  <div class="media">
    <!-- Replace with <video> if needed -->
<iframe   src="https://www.youtube.com/embed/68KRv2RYLxA?si=dg78yrvzcB-sfkJV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>
</div>

<div class="project-showcase">
  <div class="media">

<p>
A first-person action-packed wave-defence experience with some resource management mechanics or 
as described in the game's pitch:
</p>
  <blockquote>
<p>
"Chempunch is an intense wave defence action game focusing on reactive but aggressive gameplay in a chempunk world.
Alone but far from helpless, you must protect your slums from an oncoming horde, using chemicals you route around your body to empower different actions."
</p>
  </blockquote>
</div>
  <div class="contributions">

  <img src="./ChempunchHand.png">
</div>
</div>


  <picture>
  <source type = "image/webp" srcset="./ChemPunch-Team-Work.webp"/>
    <img src="./ChemPunch-Team-Work.webp" alt="Team Work illustration" />


  </picture>



## Enemy Horde AI System
The AI was originally designed for a mode where enemies rushed an objective and the player intercepted them. This later shifted to wave defence, but the AI kept its flexibility.

#### Goal Prioritization
Enemies choose between the player and the objective based on navmesh distance, weighted by configurable scalars. To avoid erratic switching when scores were close, I stabilized decisions by adjusting how often priorities were updated in the behavior tree.

#### Strafing Behavior
To support strafing, I separated movement and attack goals. By tweaking their weights, enemies could act aggressively, rush the objective, or strafe while firing—creating variety in enemy types.

#### Player Awareness
Enemies use sight (via Unreal’s perception system) and sound (e.g. gunfire, ally deaths) to detect and track player. Player location is individually tracked by enemies and forgotten after a short delay.

#### Tactical Ranged Positioning
Ranged enemies maintain their distance by stepping backward while maintaining line of sight. This is handled through Unreal’s Environment Query System (EQS).

## Congestion-Aware Pathfinding
To reduce bottlenecks in enemy movement and ease the burden on level designers, I extended Unreal’s default pathfinding system with a congestion-aware routing mechanism.
A custom data structure, the **Congestion Map**, tracks traffic density across navmesh nodes by associating each node with a congestion score (an unsigned integer). Every few frames, the system:

*   Queries all active enemies
*   Retrieves their current navmesh node via the path-following component
*   Increments that node’s congestion score

This data is then fed into a custom **FCustomRecastNavQueryFilter**, which biases pathfinding away from highly congested areas. As a result, enemies naturally spread out and avoid forming predictable chokepoints, making encounters feel more dynamic and organic.


<div class="expandable">
<button class="expand-toggle ">Updating Congestion Map Code Snippet ▾</button>
<div class="expand-content">

<div class="highlight">
<pre class="highlight">

<code>
void AAICoordinatorActor::UpdateEnemyPerNode(TMap&lt;int64, int&gt;&amp; NodeEnemyMap) const
{
    //Initialize the enemy per node map
    NodeEnemyMap.Empty();
    for (AHordeEnemyAI* EnemyAI : HordeEnemyAIs)
    {
        NavNodeRef EnemyCurrentNatNode = GetNavNodeRef(EnemyAI);

        if(EnemyCurrentNatNode == 0)
            continue;
        //Add or increment the enemy map
        if (NodeEnemyMap.Contains(EnemyCurrentNatNode) )
            NodeEnemyMap[EnemyCurrentNatNode]++;
        else
            NodeEnemyMap.Add(EnemyCurrentNatNode, 1);


    }
}
void AAICoordinatorActor::UpdateAllCongestion()
{
    CongestionMap.Reset();


    //Update enemy count per node
    UpdateEnemyPerNode(EnemyPerNode);


    for (auto NavNodeEnemyCountPair : EnemyPerNode)
    {
        //Get the congestion value for the node
        int enemyCount = NavNodeEnemyCountPair.Value;

        UNavigationSystemV1* NavSys = UNavigationSystemV1::GetCurrent(GetWorld());
        if (ensure(NavSys))
        {
            auto NavMesh = Cast&lt;ARecastNavMesh&gt;(NavSys-&gt;GetMainNavData());
            float NavPolySize = 
                CalculatePolyAreaSize(NavNodeEnemyCountPair.Key, NavMesh);

            //Current congestion algorithm interpolates the new estimated area value
            //in the range on [0, 3*polygonArea]

            float estimatedFreeArea = NavPolySize - enemyCount * 
                this-&gt;CongestionMapParams.CongestionWeightPerEnemy;


            float congestionValue = 1 -
                UKismetMathLibrary::NormalizeToRange(estimatedFreeArea, 
                    0, NavPolySize);
            CongestionMap.Add(NavNodeEnemyCountPair.Key, congestionValue);
        }
    }
}
</code>
</pre>
</div>
</div>
</div>




## Wave System
Implemented the full wave logic, including timers, active spawners, door control, and randomised enemy distribution. All parameters are exposed in the Unreal Editor, allowing designers to easily configure and test wave setups without code changes.



## Dynamic Difficulty
Developed a designer-friendly system that tracks a score, which increases when enemies are defeated and gradually decays over time. This score feeds into custom editor curves to dynamically scale enemy cap and spawn rate, adapting the game’s intensity in real time.

[back](./)
