---
layout: default
title: Chempunch
description: A first-person wave-defense action game.
permalink: /Chempunch.html
---

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
A first-person action packed wave-defense experience with some resource management mechanics or 
as the pitch goes:

> "Chempunch is an intense wave defence action game focussing on reactive but aggressive gameplay in a chempunk world.
>  Alone but far from helpless, you must protect your slums from an oncoming hoard, using chemicals you route around your body to empower different actions."

## Enemy Horde AI System
The AI was originally designed for a mode where enemies rushed an objective and the player intercepted them. This later shifted to wave defense, but the AI kept its flexibility.

#### Goal Prioritization
Enemies choose between the player and the objective based on navmesh distance, weighted by configurable scalars. To avoid erratic switching when scores were close, I stabilized decisions by adjusting how often priorities were updated in the behavior tree.

#### Strafing Behavior
To support strafing, I separated movement and attack goals. By tweaking their weights, enemies could act aggressively, rush the objective, or strafe while firing—creating variety in enemy types.

#### Player Awareness
Enemies use sight (via Unreal’s perception system) and sound (e.g. gunfire, ally death) to track the player. Player location is shared between nearby enemies and forgotten after a short delay.

#### Tactical Ranged Positioning
Ranged enemies keep distance by stepping backward while maintaining line of sight. This is handled through Unreal’s Environment Query System (EQS).

## Congestion-Aware Pathfinding
I extended Unreal's pathfinding with a congestion map that tracks navmesh traffic. Enemies avoid crowded paths, diversifyin their routes to the objective.
This system was developed to alleviate some work of the level designers.

An extra data structure is introduced called congestion map - couples each navigation node reference with a certain unsigned integer signifying congestion.
Each X Frame the congestion system is updated by queriying all enemies and the getting the current node reference from the path following component. Once acquired,
the congestion score of that nav node is incremented by 

Utilized by **FCustomRecastNavQueryFilter** which is used by the **CongestionNavMesh** extension of the navigation system.

<!--
#### Role And Responsiblities
* Developed AI system for the enemies leveraging UE behaviour trees custom AI utility functions 
* Encoded the wave system of the game
* Programmed a designer controllable dynamic difficulty system 
* Profiled performance and memory usage, finding critical bottlenecks in code an unoptimized assets. 
* Setup WWise integration on multiple machine in different development environments



## Enemy Horder AI System

Note that most of the AI decision made were taoiled to a game in which the player had to intercept
the enemy horder on their path toward the objective as this was the original design idea.
Which later got changed to a simpler wave defense due to time constraints.

#### Prioritizing between objective and player
The final system calculates the distance to both goal from the NavSystem.
The distance is divided by scalar weight to result to a score for each of the goals. (Higher Score Gets Picked).

A common problem with a problem definitions such as this is: there is usually a distance to both goals such that 
the score as so close to each other that any small changes tip the scale to one or the other. 
This leads to the enemies looking confused and uncertain. 
There are many ways to solve this problem:
*   A scaler change of goal cost.
*   Different weight,curves for when an goal is maintained and when it is not
*   Modifying the tick time of the custom service in the Behaviour Tree.

The simplest and most practical for the purposes of the project was the last one. So I went with it.


#### Enemy Strafing - a desired feature
One of the features that deisgner wanted to have is Enemy Strafing while walking toward the objective.
This was to show that enemies have more important things to do than kill to player and enforce the game as wave defense game.

The previous goal prioritization was reframed a Goal To Move To, but not necessarily Goal . 
The previosu system was separated to Goal To Move and Goal To Attack. The newly introduced 
Goal to Attack however used Euclidean distance to the attacked objective (simpler and has more physical sense).
Just by modifyign the weight some distincty enemy types emerges from this system : 
balacned set of weights would have the enemies prioritizing what is closest. 
Higher weight on just objective related would result in the enemies rushing the objecitve.
Lower weight on just objective related would result in the enemies beign extra aggresive to player.
Higher on go to objectve and higher on attack player would result in strafing.

Even in the Rush To Objective type if player is easy to hit and close the would it, avoiding the enemies to feel one-dimensional.

#### Handling Visiblity and Player Awareness
The above paragraph detail how the systems would act if both the players and objective position are known, however, 
enemy agents do not have completely information and that is rarely the case.
Two simple strategies are used:
*   Visbility: Utilizing Unreal's sensory system when the player is seen the player pawn's position in assigend to the Black Board(BB).
*   Sound: When an enemy is killed nearby that produces a *sound* or a sensory sphere of influence immediately notifying enemies in it about the player's position.
Additionally, when an enemy shoots at the player a smaller SOI is generated to notify nearby enemies that something is wrong.

Knowledge of the plaer is lost only after a specfici time.

#### Tactical Positioning
When attacking ranged enemies would attempt to maintain a desired range from the player.
If player is closer the would walk backward while attacking the player, they move backwards
 while also ensuring that the position they go to has visibility of the player.
 This is done by using the Environment Query System.

#### Custom Pathfiding by extending the UE's pathfinding system to include congestion
-->

    void AAICoordinatorActor::UpdateEnemyPerNode(TMap<int64, int>& NodeEnemyMap) const
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
                auto NavMesh = Cast<ARecastNavMesh>(NavSys->GetMainNavData());
                float NavPolySize = 
                    CalculatePolyAreaSize(NavNodeEnemyCountPair.Key, NavMesh);

                //Current congestion algorithm interpolates the new estimated area value
                //in the range on [0, 3*polygonArea]

                float estimatedFreeArea = NavPolySize - enemyCount * 
                    this->CongestionMapParams.CongestionWeightPerEnemy;


                float congestionValue = 1 -
                    UKismetMathLibrary::NormalizeToRange(estimatedFreeArea, 
                        0, NavPolySize);
                CongestionMap.Add(NavNodeEnemyCountPair.Key, congestionValue);
            }
        }

    }


## Wave System
Implemented the full wave logic, including timers, active spawners, door control, and randomized enemy distribution. All parameters are exposed in the Unreal Editor, allowing designers to easily configure and test wave setups without code changes.



## Dynamic Difficulty
Developed a designer-friendly system that tracks a score which increases when enemies are defeated and gradually decays over time. This score feeds into custom editor curves to dynamically scale enemy cap and spawn rate, adapting the game’s intensity in real time.

[back](./)
