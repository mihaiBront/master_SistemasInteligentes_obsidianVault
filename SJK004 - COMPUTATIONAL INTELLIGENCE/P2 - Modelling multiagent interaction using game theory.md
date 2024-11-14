
# 2.1 Rational Agent

An agent is rational when, choosing among their preferences, it chooses based on its acceptable preference order: 
- An order that is complete (all has relation to all) and transitive (if A is preferred to B and B is preferred to C; A is preferred to C). 
- Which one is selected is done by the utility function.

>[!example] Example: Beating eggs
>The agent has already beaten 5 and has 1 left, but there is a risk that it is bad.
>What actions, potential scenarios and preferences exist in this example?
>![[Pasted image 20241107152818.png]]
>How to sort these preferences? By using the utility values of each preference:
>
>$$C_{11} \ge C_{21}\ge C_{31} \approx C_{32}\ge C_{22}\ge C_{12}$$
>
>$$C_{11} \ge C_{31}\approx C_{32} \ge C_{21}\ge C_{212}\ge C_{22}$$
>
>In which of the two states is the egg?

>[!example] The tproblem of the corrupt politician 
>What if the agent isn't sure what the current state of the world is? The EU (expected utility) can be calculated: EU. (“A” denotes the action selected by the agent)
>$$UE(A) = \sum p(S)U[C(S,A)]$$
>$$sujeto_a\sum p(S) = 1$$
> ![[Pasted image 20241107153424.png]]
> Suppose that: 
> $U(C3 ) = 1$ 
> $U(C 2 ) = 0.6$
> $U(C 1 ) = 0$ (these values provide a preference order) $p(S 1 ) = 0.2$
> $p(S 2 ) = 0.8$
> 
> ***A. What action is the corrupt politician to take?***
> $UE(A1) = 0.2 * 0 + 0.8*1 = 0.8$
> $UE(A2) = 0.2 * 0.6 + 0.8*0.6 = 0.6$
> $UE(A3) = 0.2 * 0 + 0.8*1 = 0$
> 
> ***B. Assuming that the probability of the existence of evidence is unknown, with what probability is A2 (confess part) better than A1 (deny everything)?***
>$UE(A1) = p * 0 + (1-p)*1 = 0.8$
> $UE(A2) = p * 0.6 + (1-p)*0.6 = 0.6$
> $UE(A3) = p * 0 + (1-p)*1 = 0$
> 
> Resolve the equation system and obtain P:
> $1-p \le 0.6$
> $p \ge 0.4$
> 
> That means: "*When the probability of evidence ($p$) is greater or equal than $0.4$, the politician should confess*"
>
>***Note that the values of $U(C_i)$ have been given in the problem, but how do we determine that??? :LiArrowBigDownDash:***

# 2.2 Von Neumann-Morgenstern Utility

Evaluate the intermediate consequences (Cs) based on the best and worst available consequences acting in a lottery. 

Lotteries are defined (consequences tied with probabilities).
$$
L = (p_1C_1, p_2C_2, ...,p_nC_n), \space sujeto \space a \sum p_i = 1
$$
The player's indifference is sought between opting for an intermediate consequence and the probability of playing a lottery in which only extreme consequences can come out. 

For example, with the problem of the corrupt politician: 
- C2 is not indifferent to (0.4C3, 0.6C1) (it prefers C2 to that lottery)
- C2 is indifferent to (0.6C3, 0.4C1) 

The best and worst consequence utilities are normalized to 1 and 0. 
- U(C3) = 1 
- U(C1) = 0 

The utilities of intermediate consequences are calculated using the expected utility of the lottery that makes them indifferent: 
$U(C_2) = UE(L(0.6C_3 , 0.4C_1 )) = 0.6*1 + 0.4*0 = 0.6$

The ***Von Neumann-Morgenstern*** utility function determines **which Lottery is the most suitable for each possible action**: choosing the lottery associated with the action that is most likely to result in the best possible consequence.

>[!example] 
>***Given two actions A1 and A2 with, respectively, lotteries L1 and L2. Suppose C1 be the best consequence, C4 the worst consequence, and it is known that the intermediate options show the indifferences indicated below. Which action will choose the agent: A1 or A2?***
>
> ![[Pasted image 20241107154827.png]]
> 
> ***A. Calculate L2 according to the previous pattern. What action does the agent chose?***
> 
> $L2 = 0.2C_1, 0.3[0.7C_1, 0.3C_4], 0.3[0.4C_1, 0.6C_4], 0.2C_4$
> $L2 = 0.2C_1, 0.21C_1, 0.09C_4, 0.12C_1, 0.18C_4, 0.2C_4$
> $L2 = 0.53C_1 , 0.47c_4$//
>
>Calculate equivalent utilities (assuming $U(C_1) = 1;\space U(C_4)=0$)
> $UE(L_1) = 0.55U(C_1) + 0.45U(C_4) = 0.55$
> $UE(L_2) = 0.53U(C_1) + 0.47U(C_4) = 0.53$
>
> $UE(L_1) > UE(L_2)$
> 
> ***Yet we reached the same kind of problem; lotteries have been defined by the problem***

# 2.3 Attitude to risk

Risk is a key to measure the intensity of the agent's preferences.

It is the agent indifferent between: 
1) play a lottery (obtain the expected utility of the lottery: $UE(L) = ⨊pU(Ci))$; either 
2) obtain the expected value of the lottery $E(L) = ⨊pCi)$?

There are three possibilities: 
- **Risk neutral agent**. It is indifferent to himself. 
- **Risk prone agent**. He prefers to play the lottery. 
- **Risk averse agent**. Prefers the expected value of the lottery.

Suppose a linear utility function: $U(x) = 4x + 2$
The agent is offered the lottery $L = [4/5 1 euro, 1/5 16 euro]$

The expected value of the lottery $L$ is $E(L) = 4/5*1 + 1/5*16 = 4 euros$ 
$U(E(L)) = U(4) = 4*4 + 2 = 18$ 
$UE(L) = 4/5*U(1) + 1/5*U(16) =$ 
  $= 4/5*6 + 1/5*66 = (66+24)/5 = 90/5 = 18$ 
$U(E(L)) = UE(L)$ The agent is risk neutral

Assume a non-linear utility function $U(x) = 4\sqrt x$
$U(E(L)) = U(4/5*1 + 1/5* 16) U(4) = 4 = 8 euros$
$UE(L) = 4/5*U(1) + 1/5*U(16) =(4/5)*4 + (1/5)* 4 \sqrt 16$
$= (16+16)/5 = 6.4 euros ⇒ U(E(L)) >UE(L)$ 
The agent is risk averse

![[Pasted image 20241107161033.png]]
But, do we, humans, act like this, with this type of calculations in our interactions? 

Numerous experiments show that people deviate from the results provided by utility theory.

## Allais' Paradox
![[Pasted image 20241107161109.png]]
What do you choose choose L or M? What do you choose L’ or M’? … **Choose now!**

Many people tend to choose L (between L and M) and M' (between L' and M') 

Normalize utilities such that $U(2.500.000) = 1; U(500.000) = z$; $U(0) = 0$ where $0 < z < 1$ 

What must be hold to choose L over M? And M’ over L’? 

***Calculate $UE(L), UE(M), UE(L’) y UE(M’)$ … Is it consistent to choose L and M'?***

$UE(L) = 0.01z +0.1z +0.89z = z$
$UE(M) = 0.01·0 +0.1·1 +0.89z = 0.89z +0.1$

To chose L>M -> $z > 0.1/0.11 = 0.9090$

$UE(L') = 0.01z +0.1z +0.89·0 = 0.11z$
$UE(M) = 0.01·0 +0.1·1 +0.89·0 = 0.1$

To chose L>M -> $z < 0.1/0.11 = 0.9090$

<u style="color:red; font-weight:600">Those condition can never be met!</u>


# 2.4 Strategic multi-agent encounters

We need a model of the environment in which agents act:
- The agents simultaneously choose an action to execute and as a result of its execution, a scenario of Ω will be obtained. 
- Such a current scenario depends on the combination of actions 
- There are utility functions of VN-M for each combination of strategies

Behavior of the environment will be given by the state transformation function:
$$\tau: A_{ci} \space x \space A_{cj} > \Omega$$

![[Pasted image 20241107162459.png]]

## Rational action:

![[Pasted image 20241107162530.png]]
## Payoff matrix
![[Pasted image 20241107162611.png]]
Where elements are $j,i$

![[Pasted image 20241107162756.png]]

Payoff matrices allow us taking into account the influence of the presence of the other agent.
## Dominant strategies:

With the last example:

- It is said that s1 dominates s2 if every possible scenario obtained by i when playing s1 is preferred over every possible scenario obtained by i when playing s2. 
- A rational agent will never play a dominated strategy 
- Therefore, a rational agent must eliminate the dominated strategies 
- Unfortunately, not always they exist.

![[Pasted image 20241107163224.png]]

![[Pasted image 20241107163300.png]]


## Dominant strategies and rationality

***What if we're not sure agents are rational?***

![[Pasted image 20241107163558.png]]

Suppose J1 believes that J2 is 98% rational. What would be the best strategy for J1?

$UE J1 (U) = 0.98*8 + 0.02*(-100) = 5.84$
$UE J1 (D) = 0.98*7 + 0.02*6 = 6.98$

The process of eliminating dominant strategies does not always lead to an adequate solution (especially if we play with people who do not work 100% with the standard mathematical criterion of rational agent)

## NASH equilibrium

***What if there's no dominance and no action can be removed?***

In general, we will say that two strategies s1 and s2 are in Nash equilibrium if: 
- Under the assumption that agent i plays s1, agent j can do no better than play s2 
	**AND**
- Under the assumption that agent j plays s2, agent i can do no better than play s1. 

Nash equilibrium is the couple of actions chosen considered the optimal action taken by the other agent.

***Calculate the Nash equilibria of the following games in Normal Form.*** Calculate every combination, from one side and from another (might be more than one equilibrium; or none)

![[Pasted image 20241107164610.png]]

For the first example:
	$(S_1, s_2), (S_2, s_1)$ are Nash equillibria


For the second example:
	There's no equilibrium

## Mixed Nash Equilibrium

Behavior of insects according to the role of Hunter or Prey

![[Pasted image 20241107165328.png]]

There are no **pure** equilibrium strategies. If Prey is Active, Hunter is Passive => Passive Hunter, Prey is Passive => Passive Prey, Active Hunter => Active Hunter, Active Prey…

What probability for the prey makes the hunter indifferent to the choice of any of his strategies?

Let p be the probability that the prey is Active ($1-p$ that it is Passive) $2p+6(1-p) = 3p –(1-p) => p = 7/8 = 0.875$

***What probability for the hunter makes the prey indifferent to the choice of any of his strategies? Let q be the probability that the hunter is Active*** 
$$
-7q -6(1-q) = -8q
$$
$$-6+6q = -q$$
$$-6 = -7q$$
$$q = 6/7 = 0.857$$
## Competitive and Zero-Sum interactions

If the preferences of the agents are totally opposite, then we have strictly **competitive** situations.

**Zero-Sum Encounters** are those in which the sum of the utilities is zero: $ui(ω) + uj(ω) = 0$ for all $ω ∈ Ω$. Therefore it implies a strictly competitive situation ($J_1$ wins with pairs, $J_2$ wins with odds)

![[Pasted image 20241107170341.png]]![[Pasted image 20241107170444.png]]

## The prisoner's dilemma

- Two people are accused of a crime and are locked in individual cells with no possibility of communication between them. 
- The following deal is offered: 
	- If you confess and the other does not confess, the confessor will go free (for helping) and the other will be imprisoned for three years 
	- If both confess, they will each be sentenced to two years. 
	- Both defendants know that if neither confesses they will be jailed for one year (for minor crimes)

The matrix is:
![[Pasted image 20241114152212.png]]

Which creates a paradox, the prisoners will never chose defect cause they don't trust each other.

Individual rational action is defective This guarantees a payoff no worse than 2, while cooperating guarantees a payoff of at most 1.

Then default is the best answer for all possible strategies: both agents default, and they get a payoff = 2 

But intuition says that this is not the best scenario: If they both cooperated they would get a payoff = 3!

This apparent paradox is the so-called fundamental problem of multi-agent interactions. 

It seems to imply that cooperation will not occur in societies of self-interested agents.

>[!example] Some real examples:
> - Reduction of nuclear arsenals (“and if I agree, but I don't destroy them...”) 
> - Uncontrolled payment systems — public transport

The prisoner's dilemma is very common. Why then do we cooperate?

We could think that in the previous analysis: The notion of rational action in game theory is wrong The dilemma has some error in its statement that I can't find...

**Arguments to cooperate:**
- We are not so Machiavellian! 
- Blind trust in the other prisoner (the other prisoner is my brother, …)
- And if I find him when he gets out from prison...and he beats my ass?

## Iterated prisoner's dilema

Let's try to play the game more than once

It is no longer so clear that snitching is good, in fact: **The rational choice in the infinitely iterated prisoner's dilemma is to cooperate!**

**Induction from the end**: But…what if we both know that we will play the game exactly n times? In round n - 1, you have the incentive to default, to get more in the last round… This makes round n – 2 the last “real” one, and so defect would be interesting as well. ....

laying the prisoner's dilemma with a known, finite, fixed, predetermined number of rounds again shows that "Don't Cooperate" (default) is the best strategy.

### ***Axelrod*'s Tournament**:

Suppose we play the prisoner's dilemma repeatedly with several opponents..

What strategy should we choose, to maximize the total profit?

*Axelrod* (1984) investigated this problem through a computerized tournament of programs that played the prisoner's dilemma

Some strategies seen int the experiment:
![[Pasted image 20241114153225.png]]

Recipes for success:
- Axelrod suggests the following rules to be successful in your tournament: 
- Do not be jealous: Don't play like it's a zero sum game 
- Be polite: Start cooperating, and return cooperation
- Measured revenge: Always punish “non-cooperation” immediately, but do not over-punish 
- Be quick to return the good: Cooperate with whoever cooperates with you immediately