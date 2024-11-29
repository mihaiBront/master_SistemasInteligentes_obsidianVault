---
Subject: Computational Intelligence
Teacher: "@LuisAmableGarcíaFernández"
Topic: 
date: 
tags:
  - SJK004-ComputationalIntelligence
---
# Mechanisms (protocols) and Strategies

- The **negotiation process** is driven by a **protocol** (also called ***mechanism***) that defines the interaction rules among the agents. 
- Mechanism design is a **mathematical field** that design ***protocols showing certain desirable properties***. For example, incentivizing agents to participate in the protocol.

## Criteria for mechanism design

- **Convergence** (it reaches a finish)
  
- **Successful agreement** (not only it reaches a finish, but an agreement).
  
- **Social welfare**: ***maximizing the cumulative addition*** of utilities for all the agents involved. 
  -> It assures the sum of all benefits is maximum 
  
- **Pareto efficiency**: x is P.E. if ***there is not a x’ such that x’ is best that x for an agent and for the rest of agents x’ is greater or equal than x*** 
  In other words; P.E: implies that there is not possible to give an agent a better profit without give some other agents a worst profit) 
  -> The best solution for everyone; (e.g., if there is a start that returns $[5,3]$, there is no other strategy that would benefit both; if there were strategies $[6,2]$ or $[4,4]$ but both lose something to the $[5,3]$, so would NOT BE PARETO EFFICIENT)
  
- **Individual Rationality**.
   
- **Stability**: -> Finding the best path for everyone 
	- ***Dominant Strategie***: i chooses strategie si if it is the best for him independently of the strategies played by other agents. 
	- ***Nash equilibrium***: $s_A^* = (s_1^* ,…,s_n^* )$ is in N.e. if ∀i, s i * is the best strategy given that the rest of agents use the strategies $s_{A-i}^* = (s_1^* ,… s_{i-1}^*,s_{i+1}^*,..,s_n^* )$. 
	  
- **Computational efficiency**: The execution cost of the protocol is low, there are more available time for deciding which strategy to use. 

- **Distribution** -> ***Cost of anarchy***

>[!example] Cases with the *prisoner's dilema*
>
>![[Pasted image 20241128153905.png]]
>
>a) **Social Wellfare** -> CC
>
>b) **Pareto Efficient** -> There is no pareto-efficient
>
>c) **Dominant** and **NE** -> DD
>
>d) **Calculate mixed Nash Eq**:
>
>#remember *The mixed equilibrium exists when no Pure equilibrium exists* 
>
>$p: prob_i \space plays \space defect$
>$1 - p: prob_i \space plays \space coop$
>
>The probability J does not care what I does:
>
>$$2p + 4(1-p) - p - 3(1-p) = 0$$
>$$p + (1-p) = 0$$
>$$1 = 0 \ne true=>indetermination$$
>
>***DD is the pure, I didn't realize before***

>[!question] Let's assume that two persons has to split a given amount of money. Which solutions will be Pareto-efficiency?
>
>50/50, right? -> What about 49/51, does it exist any other combination that is better for both? NO, all solutions are pareto.


# The concept of voting

## Objectives and considerations

- **Maximize Social Welfare**: all agents provide a vote based on their preferences to a mechanism (which serves as both the ballot box and the ballot count), and the result is the agreed-upon solution for all agents.
- This is not an easy problem to solve as it can seems. Moreover, agents can ***not to be truthful*** when they send their votes … 

## Truthful agents

Each agent has preferences about what it is voting. So, it set a transitive and anti-symmetric order $≻_i$ among these preferences (O). Then, it send a vote (according to that order) to the mechanism. The mechanism receive all votes and calculate the overall solution $≻^*$, called the social preference relationship.

How ≻ * has to be for the overall procedure to be considered “fair”? 
- $≻^*$ **must exist for all** the **preferences** of the agents 
- $≻^*$ must be **defined for each pair** $o,o’$ from $O$ -> Transitive 
- $≻^*$ must be **antisymmetric** and transitive in $O$ 
- $≻^*$ **must be pareto** efficient: if $o ≻_io’$ then $o ≻*o’$ 
- $≻^*$ must be **independent of irrelevant alternatives**. ( #tbd )
- $≻^*$ no agent can be a ***dictator***

>[!fail] Is this possible to design? -> NO!
>**Arrow’s impossibility theorem** There is no mechanism that can be designed to fulfill all previously exposed 6 conditions
>Therefore any of these conditions must be relaxed

### Popular voting strategies
#### Binary Protocol

>[!example] 
>- 35% agents C ≻ D ≻ B ≻ A 
>- 33% agents A > C ≻ D ≻ B 
>- 32% agents B ≻ A ≻ C ≻ D
>
>The results are not determined, the results totally depends on the order of the binarizations:
>
>(D,B) → (D,A) → (A,C) → A wins 
>(C,A) → (D,A) → (A,B) → B wins 
>(A,B) → (B,C) → (C,D) → C wins
>(C,A) → (A,B) → (B,D) → D wins, **but observe that all agents prefer C to D**
>
>This is an illustrative, extreme case, to exemplify what can happen


>[!faq] **Condorcet paradox**
>![[Pasted image 20241128160051.png]]
>Every possible winner has a majority against

#### Borda Protocol:

Give points to each preference by its position in the order of preferences. 
-> **Problem**: the outcome can change by introducing irrelevant alternatives

![[Pasted image 20241128160508.png]]
## Untruthful agents

>[!example] Vote amongst neighbours if we have to paint the front of a building
>- If the cost to paint it is **splitted among all the neighbours**. I***s it fair for the neighbours that do not want to paint it to paid too***? 
>- If the cost for painting is **splitted among all the neighbours that want to paint it**, and one neighbour $i$ , who wants to be painted, ***knows that there is a majority of neighbours that want to paint it, what should agent $i$ vote*** (acting as a self-interested one)?
>
>If a rational agent can get benefit sending an untruthful vote, it will do it

How do we  deal with these situations? -> Introducing incentives in the protocol to make agents act truthfully

Formally: A mechanism design problem has a set of involved agents with the following properties: 
- Each agent i has a private type θi ∈ Θ (what he actually thinks) 
- Mechanism $g$: $S_1 · S_2 · .. · S_A → O$ (where $O$ must fulfill the *Social Election Function*)
- Each agent i gets a value $v_i(o, θ:i)$ 
- A Social election function describes the outcome that the mechanism wants to be reached $f: Θ → O$
