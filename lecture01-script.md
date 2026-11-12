# Speaker script: Multi-Agent Reinforcement Learning, a slight intro

Guest lecture, 75 minutes. Deck: `lecture01.tex`. Book: Albrecht, Christianos & Schäfer, *Multi-Agent Reinforcement Learning: Foundations and Modern Approaches* (MIT Press, 2024), Chapters 1 and 3.

## How to use this script

- Headings use the slide counter in the bottom right of each slide ("12 / 62"), not the PDF page number. Overlay steps share one counter.
- **[click]** means advance one overlay step on the same slide.
- *Italic lines* are stage directions, not things to say.
- Slides marked **(optional)** can be skipped if you are running late. The script for the next slide still works.
- The spoken text is about 5,700 words: roughly 39–44 minutes at 130–145 words per minute. Pauses, pointing at diagrams and the overlay steps usually add 10–15 minutes, which leaves about 15 minutes for questions. Times below assume 130 words per minute with no pauses.

| Part | Slides | Words | Time | Clock at end |
| --- | --- | --- | --- | --- |
| Opening | 1–3 | 262 | 2 min | 0:02 |
| MARL in the world | 4–9 | 590 | 5 min | 0:07 |
| The MARL loop | 10–14 | 477 | 4 min | 0:10 |
| Why many agents? | 15–17 | 202 | 2 min | 0:12 |
| What makes it hard | 18–25 | 652 | 5 min | 0:17 |
| Normal-form games | 26–32 | 799 | 6 min | 0:23 |
| Repeated games | 33–37 | 537 | 4 min | 0:27 |
| Stochastic games | 38–44 | 664 | 5 min | 0:32 |
| Partial observability | 45–51 | 750 | 6 min | 0:38 |
| Communication | 52–54 | 204 | 2 min | 0:40 |
| Knowledge | 55–58 | 294 | 2 min | 0:42 |
| Summary and questions | 59–62 | 293 | 2 min | 0:44 |

Check the clock at the end of the normal-form games (planned about 0:23 plus pauses). If you are well behind, skip the optional slides 32, 41, 48, 51, 54 and 57.

---

## Opening

### Slide 1: Title

Hi everyone, and thanks for having me. My name is Chirayu. Today is a gentle introduction to multi-agent reinforcement learning, which people shorten to MARL.

I'm assuming you have seen ordinary reinforcement learning: one agent, an environment, rewards, a policy. I am not assuming any game theory. Everything we need from game theory, we will build from pictures today.

### Slide 2: Today

Here is the plan, five steps.

First, examples: where do many learning agents show up in the real world? Second, the MARL loop: how the familiar RL picture changes when there is more than one agent. Third, why we would want many agents at all, instead of one big agent. Fourth, what makes this hard. There are four challenges that do not exist in single-agent RL. Fifth, and this is the biggest part of the talk, the game models: the mathematical objects we use to write a multi-agent problem down.

The little numbers under each step are book sections, in case you want to read more afterwards.

### Slide 3: Highly recommend this book

Almost everything today follows this book, by Stefano Albrecht, Filippos Christianos and Lukas Schäfer. It is free online at marl-book.com.

The book has two parts. Part I, in blue, is the foundations: RL, games, what it means to solve a game, and the classic algorithms. Part II, in green, is deep MARL: neural networks, deep RL, and multi-agent deep RL in practice.

Today we cover the two circled chapters: Chapter 1, the introduction, and Chapter 3, the game models. If you leave today wanting more, Chapter 4, on solution concepts, is the natural next read.

---

## MARL in the world

### Slide 4: MARL in the world (section divider)

Let's start with the world, not with the maths.

### Slide 5: Many agents systems are ubiquitous

Here are four places where many decision makers share one world. Robots in a warehouse. Players in a board game or a video game. Cars at an intersection. Traders in an electronic market.

They look very different. But in every one of them there are several agents, each one choosing actions, and what is good for one agent depends on what the others do. That last part is the whole point. Let's go through them one at a time, and for each one ask the same four questions: who is the agent, what does it observe, what can it do, and what is its reward.

### Slide 6: Warehouse robots

A large warehouse: rows of shelves, a constant stream of orders, and a team of mobile robots that drive around, pick items and bring them to a station. The book imagines a hundred robots.

*Point to each label on the right as you say it.*

The **agent** is one robot. So with a hundred robots we have a hundred agents. It **observes** what is nearby, its position, the shelves around it, and the order it is working on. It might also see other robots. It **acts** by moving and picking items up. And the **reward**: here we give +1 to *every* robot whenever *any* order is completed.

When all agents get the same reward, we call it **common reward**. It is the fully cooperative case. Keep that name in mind, it comes back. The book also mentions the alternative: give each robot its own reward only for its own orders. Same robots, different reward, and as we will see, a different problem.

### Slide 7: Board and video games

Now the opposite end. Two players, one board: think chess or Go. Each player is an agent. It **observes** the board, it **acts** by placing a stone, and the **reward** is +1 for a win and −1 for a loss.

Notice that the two rewards always add up to zero. My win is exactly your loss. That is called **zero-sum**. It is pure competition. A lot of the famous results in this field are of this type: backgammon, Go, StarCraft, Diplomacy. The book lists the papers.

### Slide 8: Autonomous driving

Now something in between. Cars at an intersection. One **agent** is one car. It **observes** what its sensors can see: the green cone. It **acts** by steering, speeding up and braking. The **reward** says: get where you are going fast, and never crash.

Two things to notice. First, the rewards are **mixed**. Every car wants to avoid a crash, so they share part of their goal. But each car also wants to arrive *quickly*, and that is selfish. This is neither fully cooperative nor fully competitive; we call it **general-sum**. The book calls it among the most challenging cases.

Second, look at the red car with the question mark. It is outside the green cone. Our car cannot see it. That is **partial observability**: the agent does not see the whole state of the world. We'll give that a precise model near the end of the talk.

### Slide 9: Electronic trading

The last example: software agents trading in an electronic market. One **agent** is one trader. It **observes** prices and the order book, shown inside the dashed box. It **acts** by buying or selling. Its **reward** is its own profit.

This is again mixed, or general-sum. Traders need each other, because someone has to sell for me to buy, but each one wants the best deal for itself.

So across four examples we already met the three reward types that organize the whole field: common reward, zero-sum and general-sum.

---

## The MARL loop

### Slide 10: The MARL loop (section divider)

Let's turn these pictures into one picture.

### Slide 11: You already know this: one agent

This is the loop you know from reinforcement learning. The agent sees an observation and a reward, it chooses an action, the action changes the environment, and around we go. The goal is to learn a policy, a rule for choosing actions, that collects as much reward as possible over time.

Hold this picture in your head. We're going to add agents to it.

### Slide 12: Many agents

*This slide builds in three steps.*

Now there are *n* agents. First: each agent gets its **own** observation and its **own** reward: *o*₁ and *r*₁ for agent 1, *o*₂ and *r*₂ for agent 2, and so on. They do not all see the same thing, and they do not all get the same reward.

**[click]** Second: each agent picks its **own** action, *a*₁ up to *a*ₙ. Nobody chooses for anybody else.

**[click]** Third: all those actions together form one **joint action**, written *a* = (*a*₁, …, *a*ₙ), and it is the joint action that changes the environment. Then every agent gets a new observation and a new reward, and the loop repeats.

This is book Figure 1.3, and it is the most important picture of the day. The key difference from single-agent RL: what happens to me depends on what *everyone* did, not just on what I did.

### Slide 13: Running example: level-based foraging

We need one small example we can reason about by hand. This is the book's running example, called **level-based foraging**.

There is a grid. The robots are the agents; the apples are items to collect. Every robot and every apple has a number, its **level**. A group of robots can collect an apple if they stand next to it, all choose "collect" at the same time, and the sum of their levels is at least the apple's level.

*Point to the dashed purple box.*

Look here: the green robot has level 2, the yellow robot has level 1, and the apple has level 3. Two plus one is three, so they can collect it, but only **together**. Neither could do it alone.

Each robot has six actions: up, down, left, right, collect, and do nothing. We will use this example all through the talk.

### Slide 14: One episode

Here is one short episode. At time 0, the green and yellow robots are on either side of an apple. They both move toward it: green moves right, yellow moves left. That pair of moves is the joint action. At time 1 they are both next to the apple. Now both choose collect, and the apple is gone at time 2, and each of them gets +1.

Of course, at the start of learning they would not do this. They act randomly. Over many episodes they learn better policies. That's the MARL learning problem: *random at first, many episodes, better policies*. The same as single-agent RL, except that the agents are learning at the same time.

---

## Why many agents?

### Slide 15: Why many agents? (section divider)

A fair question at this point: why bother with many agents? Why not use one big agent that controls everything?

### Slide 16: Split one big problem into small ones

On the left, one central agent controls all three robots. Each robot has 6 actions, so the central agent must choose among 6 × 6 × 6 = **216** joint actions. That is the purple block. On the right, each robot is its own agent and chooses among only **6** actions.

So splitting the problem makes each agent's decision much smaller. The catch is written in red: **now they must coordinate**. Each robot only controls its own action, and the good outcomes need the right *combination*.

### Slide 17: Sometimes there is no central controller

And sometimes one big agent is not even an option. On the left, imagine one controller in a tower driving every car in the city. It would need perfect communication with every car, all the time. That is usually impossible. On the right is what actually happens: each car runs its own policy, π₁, π₂, π₃, and acts only on what it can see.

So MARL is not just a trick for efficiency. In driving, search and rescue, and many other settings, the decisions *must* be made separately by each agent. We call such policies **decentralized**.

---

## What makes it hard

### Slide 18: What makes it hard (section divider)

So we want many agents. What goes wrong?

### Slide 19: Four challenges

The book names four challenges. A **moving target**, which is the problem of non-stationarity. **Which optimum?**, which is equilibrium selection. **Who earned it?**, which is credit assignment. And **too many combinations**, which is scaling. One picture each.

### Slide 20: Challenge 1: a moving target

On the left is single-agent RL. The world stays the same, so the best action stays where it is. The agent learns, and the target sits still.

On the right, there are other agents, and they are learning too. When the green robot changes its policy, the best action for the yellow robot changes as well. The target moves, *because* the others are learning. From each agent's point of view the world is **non-stationary**. That breaks an assumption under almost every single-agent RL method, and it is the most basic reason MARL is harder.

### Slide 21: Challenge 2: which optimum?

Two robots meet in a corridor. If both keep left, fine. If both keep right, also fine. Two equally good solutions. But if one keeps left and the other keeps right, they crash.

So it's not enough for each agent to find "a" good solution. They have to agree on the **same** one. And they have to agree while learning, without having talked about it in advance. The book's phrase is that the agents must essentially negotiate during learning which solution to converge to. In game theory these solutions are called equilibria, and this is the problem of **equilibrium selection**. Chapter 4 of the book is all about what "a solution" even means.

### Slide 22: Challenge 3: who earned the reward?

Back to foraging. On the right, green with level 2 and yellow with level 1 stand next to an apple of level 3. Two plus one is three: they can collect it. In the top-left corner, blue, with level 1, stands next to an apple of level 2.

All three choose "collect". Green and yellow get their apple. Blue's "collect" does nothing, because 1 is less than 2. But with a common reward, everybody gets +1, including blue.

Now put yourself in blue's shoes. You chose collect and you got +1. Was that a good action? You can't tell. That is **credit assignment**: working out whose action actually mattered. This is the book's own example, and it says the problem is still open.

### Slide 23: Challenge 4: too many combinations

The last one is about size. One robot has 6 actions. Two robots have 36 joint actions, three have 216, and four have 1296. In general, 6 to the power *n*.

The squares are drawn to scale: their area is the number of joint actions. You can see how fast it explodes. Anything that tries to reason about every joint action breaks quickly as you add agents. This is the **scaling** challenge.

### Slide 24: What do we want from MARL?

Before the maths, one question of goals. Researchers use MARL for different reasons, and the book lists three agendas.

**Computational**: use MARL as a way to *compute* a solution of a game. **Prescriptive**: care about how well agents do, *even while* they are still learning, not just at the end. **Descriptive**: use MARL to *model* how people or animals actually learn in groups.

The book, and this talk, take the first two views: we want algorithms that find good solutions and behave well along the way. The descriptive agenda is real and interesting, but it's outside the book's scope.

### Slide 25: Six questions about any MARL problem

Here is a checklist for any MARL problem, after book Figure 1.4.

**Size**: how many agents, states and actions? **Rewards**: common, zero-sum, or general-sum? **Observability**: does each agent see everything, or only part? **Knowledge**: what do the agents know about how the game works? **Objective**: what counts as a solution? And **centralization and communication**: is there a central controller, and can the agents send messages?

For the rest of the talk we'll answer the first four questions precisely by writing them down as **game models**. The objective, what it means to solve a game, is the next chapter of the book.

---

## Normal-form games

### Slide 26: Normal-form games (section divider)

Now the models. We start with the simplest one.

### Slide 27: A ladder of game models

*This slide builds in two steps.*

Here's the whole ladder, from book Figure 3.1. At the bottom right is the MDP you know: one agent, many states. At the bottom left, the normal-form game: many agents, but only one state. If you add states to the normal-form game, or add agents to the MDP, you get a **stochastic game**: many agents, many states. And if you then take away full observability, you get the **partially observable stochastic game** at the top.

**[click]** We start at the bottom left. Once you understand the one-state game, everything above it is that same game played again and again, with a state that changes.

### Slide 28: One round, everyone at once

*This slide builds in three steps.*

A normal-form game is a single round. Step 1: each agent picks a **policy**. Here a policy is just a probability for each action. In the example agent 1 likes C more than D, and agent 2 is closer to fifty-fifty.

**[click]** Step 2: each agent **samples** an action from its policy. And this is important: they do it **at the same time**. Nobody sees the other's choice first. Here agent 1 gets C and agent 2 gets D.

**[click]** Step 3: the two actions form the joint action (C, D), and each agent gets a **reward** that depends on that joint action: *r*₁ = *R*₁(*a*) and *r*₂ = *R*₂(*a*). Notice my reward depends on your action too.

The line at the bottom is the formal definition, book Definition 2: a set of agents, an action set for each agent, and a reward function for each agent that takes the *joint* action. That's all a normal-form game is.

### Slide 29: Two agents: a matrix game

With two agents we can draw the whole game as a table. Agent 1 picks a **row**. Agent 2 picks a **column**. Each cell is one joint action, and it holds two numbers: agent 1's reward first, then agent 2's.

That's why two-agent normal-form games are called **matrix games**. Let's look at three of them.

### Slide 30: Three kinds of games

These are the three games from book Figure 3.2, one for each reward type.

On the left, **Rock-Paper-Scissors**. Rock against scissors: agent 1 wins, 1 and −1. The same action twice: a draw, 0 and 0. In every cell the two numbers add up to zero. It is **zero-sum**. The yellow cells are where agent 1 wins, the green ones where agent 2 wins.

In the middle, a **coordination game**. Both pick A, or both pick B: 10 each. Mismatch: 0. Both agents always get the same reward, so we write only one number per cell. It is **common reward**: the corridor problem from before, as a table.

On the right, the **Prisoner's Dilemma**. Here there's no rule linking the two rewards. Sometimes we both do well, sometimes one gains at the other's cost. It is **general-sum**. This one deserves its own slide.

### Slide 31: The Prisoner's Dilemma

*This slide builds in four steps.*

The story: two suspects are questioned separately. Each can **cooperate** with the other, meaning stay silent, or **defect**, meaning testify. The rewards are minus the years in prison. If both stay silent: −1 each. If both testify: −3 each. If one testifies and the other stays silent, the one who testified walks free with 0, and the other gets −5.

Let's think as agent 1, the yellow robot.

**[click]** Suppose agent 2 cooperates. That's the left column. If I cooperate I get −1; if I defect I get 0. Zero is better, so I defect.

**[click]** Suppose agent 2 defects, the right column. If I cooperate I get −5, if I defect −3. Again, defecting is better.

So **D is better whatever the other agent does.** Game theorists call that a dominant action.

**[click]** Agent 2 reasons the same way. So both defect and land in the red cell, −3 each, while the green cell, both cooperating, would have given them −1 each. Each agent acted sensibly on its own, and both ended up worse off. That tension between what's good for me and what's good for us is why this game is so widely studied.

### Slide 32: How many 2 × 2 games are there? (optional)

A quick fact to show how much variety there is even in the tiniest games. Take two agents with two actions each, and let each agent rank the four outcomes from 1 to 4. If you count games as the same when they only differ by relabelling rows, columns or agents, there are exactly **78** different games. This is the taxonomy of Rapoport and Guyer from 1966, and the book lists all of them in Section 11.2.

In 21 of them, in green, the agents agree on which outcome is best: no conflict. In the other 57, in orange, they disagree. The Prisoner's Dilemma is game 27, here in the red box.

So even a two-by-two table can be cooperative, competitive, or anything in between.

---

## Repeated games

### Slide 33: Repeated normal-form games (section divider)

A single round is a bit artificial. Real interactions repeat. What changes if we play the same game again and again?

### Slide 34: Play the same game again

We play the same matrix game at time 0, time 1, time 2, and so on. The purple square in each small grid is the joint action played in that round. Now, at time *t*, each agent can remember everything that happened so far. We call that the **history** *h*ᵗ: the list of all past joint actions.

The policy can now depend on the history: *a*ᵢᵗ is drawn from πᵢ given *h*ᵗ. That one change, letting the policy look back, makes completely new behaviour possible. For example...

### Slide 35: Tit-for-Tat

...Tit-for-Tat, the most famous policy for the repeated Prisoner's Dilemma, from Axelrod and Hamilton in 1981. The rule fits in one line: **copy what the other agent did last time.** Start by cooperating. If the other cooperated last round, cooperate; if it defected, defect.

On the left it's a machine with two states. On the right you can see it play. Each arrow goes from the other agent's move at one time step to Tit-for-Tat's move at the next. When the other defects at step 2, Tit-for-Tat defects at step 3. When the other goes back to cooperating at step 3, Tit-for-Tat forgives at step 4.

It's nice, it punishes defection, and it forgives. It needs a history to work, which a single round can't give you.

### Slide 36: When does it end?

If the game repeats, we need to say how long it lasts. A neat way, from the book: after each round, the game **continues** with probability γ and **stops** with probability 1 − γ.

Then the chance of reaching round *t* at all is γ to the power *t*. So the expected total reward is the sum over *t* of γᵗ times the expected reward at step *t*.

And that is exactly the discounted return from reinforcement learning. So here's a second way to read the discount factor γ: as the probability that the interaction continues. With γ = 0.9 you expect about ten rounds.

### Slide 37: A known last round

*This slide builds in six steps.*

Here's why the ending matters. Suppose both agents know the Prisoner's Dilemma will be played exactly *T* times. What should they do?

**[click]** Start at the last round, *T* − 1. There is no future, so nothing I do now can be rewarded or punished later. It's just a single Prisoner's Dilemma, and in a single one, D is best.

**[click]** Now the round before. Both of us already know the last round will be D no matter what. So nothing we do in this round changes the future either. D again.

**[click]** The same argument works one step further back...

**[click]** ...and all the way to the start. With a known end, the agents defect at every step. This is called **backward induction**.

**[click]** But if there's no known end, say the game continues with probability γ close to 1, there's always a future to protect. The fear of being punished later can keep both agents cooperating. Results called **folk theorems** make this precise; the book covers them in Section 4.4.

So a finite game with a known end and an infinite game are *not* the same, even with the same matrix.

---

## Stochastic games

### Slide 38: Stochastic games (section divider)

So far there was one state, the same matrix every round. Now we let the world change.

### Slide 39: Stochastic games

This is Definition 3 in the book, the stochastic game, from Shapley in 1953. Don't worry about reading every symbol. Here is the idea in words.

There are agents and a set of states, some of which end the episode. Each agent has actions and a reward function, and the reward now depends on the state, the joint action, and the next state. There's a transition function *T* that gives the probability of the next state, given the current state and the joint action. And there's a distribution μ for the first state.

Play goes like this. The first state is drawn from μ. At each step, everyone sees the state, everyone picks an action, the joint action moves the world to a new state, and everyone gets a reward. It's exactly an MDP, except that the joint action of many agents drives the transition.

The last line says every agent sees the full state and all past actions: this is the **fully observable** case.

### Slide 40: The Markov property

Like MDPs, stochastic games are Markov. The next state and reward depend only on the current state and the current joint action, not on anything earlier. That's why they're also called **Markov games**, a name from Littman in 1994.

But read the first bullet carefully. The Markov property holds for the *joint* action. From one agent's point of view, looking only at its *own* action, it does **not** hold once the other agents' policies change over time. That's the moving target from earlier, in formal language.

The reward classification carries over: zero-sum, common-reward and general-sum stochastic games.

### Slide 41: Level-based foraging as a stochastic game (optional)

Let's write our running example as a stochastic game. The **state** is the positions of all robots and apples, plus a flag for each apple: collected or not. The **actions** are the six moves, so with *n* robots there are 6ⁿ joint actions. The **transitions**: moves update positions, and a successful joint collect flips an apple's flag. The episode ends when all apples are collected, or after a step limit.

For the **rewards** there are two choices. Common reward: everyone gets +1 whenever any apple is collected. Or general-sum: only the robots that helped get +1.

The last line is the point: same dynamics, different reward functions, different games. Choosing the reward is a modelling decision, and it changes the problem.

### Slide 42: Every state is a normal-form game

Here's the picture that connects the stochastic game to the normal-form game, after book Figure 3.3. Each state, *s*₁, *s*₂ and *s*₃, is a little matrix game. In each state, the agents play one round of that state's game: they pick a cell and get the rewards in it. But now the joint action also decides where they go next, with the probabilities on the arrows.

So a stochastic game is a set of normal-form games, joined by transitions. In each state you play one game, and your choice also decides which game you play next.

### Slide 43: Special cases of stochastic games

This makes the ladder precise. A **repeated normal-form game** is a stochastic game with only one state. An **MDP** is a stochastic game with only one agent. And a single, non-repeated normal-form game is one state played for one step.

So everything we have built today sits inside one family. And the normal-form game, the single round, is the building block of all of it.

### Slide 44: One agent's view of a stochastic game

This slide shows where non-stationarity really comes from. On the left is the MDP: the agent's environment has fixed *T* and *R*. It's stationary, so learning has a fixed target.

On the right is agent 1 in a stochastic game. What agent 1 experiences as "the environment" is the real environment **plus** agents 2 and 3. If agents 2 and 3 change their policies while they learn, then the transition and reward that agent 1 effectively sees change too. So from agent 1's seat, the environment is not stationary. This is the root of the moving-target problem.

---

## Partial observability

### Slide 45: Partially observable stochastic games (section divider)

Last step on the ladder. Agents don't see everything, like the car that couldn't see the red car.

### Slide 46: Partially observable stochastic games

This is Definition 4, the POSG, after Hansen, Bernstein and Zilberstein in 2004. It's a stochastic game with one addition: each agent gets an **observation** instead of the state. There's a set of possible observations for each agent, and an observation function that says how likely each observation is, given the last joint action and the current state.

Play is the same as before, with one change. Each agent receives its own observation, and it picks its action based on its **own observation history**, everything it has seen so far, not on the true state.

A note on notation: the book uses the same letter for the observation set and the observation function. On the slides I use a curly O for the function to keep them apart.

### Slide 47: What POSGs can express

This model is very flexible. If each agent observes the full state and the last joint action, you get back the stochastic game. With one agent, it's a **POMDP**. If all agents share one reward, it's called a **Dec-POMDP**, a decentralized POMDP.

You can also hide the other agents' actions, which happens in robot soccer or in markets with private orders. You can limit the view, like local vision or the fog of war in games. You can add sensor noise. And you can model communication, which is coming up next.

The picture on the right is partially observable foraging. Each robot sees only a 3-by-3 window around itself, the shaded squares. The blue robot cannot see the level-1 apple two cells to its left, even though it's right there.

### Slide 48: Dec-POMDPs and complexity (optional)

A word on how hard these models are, even when you know the model completely and only want to *plan*. For a finite horizon, solving an MDP is P-complete, which is efficient. A POMDP is PSPACE-complete, which is much harder. A Dec-POMDP, with two or more agents and partial observability, is NEXP-complete, which is harder again. That's the result of Bernstein and colleagues in 2002.

The intuition is in the last bullet. To act well, agent *i* has to reason about what the others have seen, and about what the others believe about *i*.

### Slide 49: Belief states

So how does a single agent deal with not seeing the state? It keeps a **belief**: a probability distribution over what the current state might be.

The update has two steps. **Predict**: use the transition function to work out where the state probably went after my action. **Correct**: use the observation I just got to reweight those guesses, using Bayes' rule. Then normalize so the probabilities add up to one. That's book Equation 3.5.

The belief is a sufficient statistic: it keeps everything from the history that matters for acting well. The catch is in the last bullet: an exact belief gets very expensive when the state has many parts.

### Slide 50: A belief update by hand

A tiny example. There's a prize behind the left or the right door. It doesn't move. When you listen, you hear the correct side with probability 0.85.

You start at fifty-fifty. You hear "left". Bayes' rule gives 0.85 times 0.5, divided by 0.85 times 0.5 plus 0.15 times 0.5. That's 0.85: you now believe left with 85 percent.

You hear "left" again. Now it's 0.85 squared over 0.85 squared plus 0.15 squared, which is 0.7225 over 0.745, about 0.97. You're quite sure.

If instead the second sound had said "right", the two pieces of evidence cancel and you're back at exactly 0.5. The bar chart on the right shows these three beliefs: the blue bar for hearing left twice, the orange bar for left then right.

### Slide 51: Beliefs with more than one agent (optional)

With more than one agent, exact beliefs get out of hand. Agent *i* doesn't see the other agent's actions, observations or history. To filter exactly, *i* would need a belief over the state *and* over *j*'s history. It would need *j*'s policy, and *j*'s policy depends on *j*'s beliefs about *i*. So now *i* needs beliefs about beliefs, and so on down the chain in the diagram.

On top of that, in MARL agents usually don't even know the transition or observation functions. So exact filtering is out. What people do in practice: feed the observations into a recurrent neural network, and let its hidden state learn a useful summary. The book covers that in Sections 7.5.2 and 8.3.

---

## Communication

### Slide 52: Modeling communication (section divider)

Can the agents just *tell* each other what they see?

### Slide 53: Communication actions

The model already allows this; we don't need a new one. Split each agent's actions into two parts: an **environment action** that changes the world, and a **message**. The rule, book Equation 3.7, is that messages don't change the state. They only show up in the other agents' observations. In the picture, the action *x*ᵢ goes into the environment, while the message *m*ᵢ goes straight to agent *j*.

A message can be a symbol, a number, a vector, or nothing at all. It lasts one step, but agents can remember it through their history.

### Slide 54: Unreliable communication through observations (optional)

Because messages arrive through observations, the observation function can make communication realistic. It can add **noise**, **drop** messages at random, or deliver them only within some **range**.

And there's a deeper point. In RL, agents don't know what any action means, and messages are no different. They have to learn what to send and how to read what they receive, and sometimes they invent a shared protocol along the way. Foerster and colleagues, and Sukhbaatar and colleagues, both in 2016, are good starting points.

For a learning algorithm, a message is simply part of the action.

---

## Knowledge

### Slide 55: Knowledge assumptions (section divider)

One last modelling question: what do the agents know about the game itself?

### Slide 56: What do agents know about the game?

Think of a scale. On the left, **complete knowledge**: every agent knows all the actions, rewards, states, transitions and observation functions. That's the usual game theory assumption. In the middle, a **simulator**: you can try something and see a sampled outcome, but you don't have the formulas. On the right, the usual **MARL** assumption: each agent sees only its own reward and observations, and knows none of the functions.

With complete knowledge, an agent could just *compute* what the other agent should do. MARL agents have to learn from experience instead, although they can still learn models of the world or of the other agents. That's called agent modelling, in Section 6.3 of the book.

### Slide 57: Higher-order knowledge and open systems (optional)

Game theory also asks: does everyone know the same things? And is something **common knowledge**: everyone knows it, everyone knows that everyone knows it, and so on without end?

In MARL these questions matter less, because the agents know so little to begin with. The exceptions are algorithms that use a known reward structure, for example a zero-sum or a common-reward one.

One more standard assumption: the number of agents is fixed and known. **Open systems**, where agents come and go, are an active research area, outside the book's scope.

### Slide 58: Dictionary: reinforcement learning ↔ game theory

A practical slide for reading papers. The two fields use different words for the same things. Environment is "game". Agent is "player". Reward is "payoff" or "utility". Policy is "strategy". A deterministic policy is a "pure strategy", and a probabilistic one is a "mixed strategy". And a "joint" anything is a "profile".

So when a paper says "mixed Nash equilibrium", it means an equilibrium made of probabilistic policies. This is book Figure 3.5.

---

## Summary and questions

### Slide 59: Summary (section divider)

Let me pull it together.

### Slide 60: Summary

*Go down the list briskly; one sentence each.*

A **normal-form game** is a single round: everyone acts at once, and the rewards depend on the joint action. With two agents it's a matrix. A **repeated game** lets policies use the history, and the discount factor can be read as the probability of continuing. A **stochastic game** adds a changing state, and each state is a normal-form game. A **POSG** gives each agent only its own observations, and exact beliefs over many agents are out of reach. Rewards come in three kinds: zero-sum, common reward, and general-sum. Communication is just actions that reach others' observations but not the state. And the MARL default is that agents don't know the model.

*Point to the boxes at the bottom.*

And here's the one-line takeaway. A **game model** plus a **solution concept** gives a **MARL problem**. Today was the game model: how to write the problem down. The solution concept, what it means to *solve* the game, with Nash equilibrium, minimax and the rest, is Chapter 4 of the book. That's where I'd go next.

### Slide 61: Questions?

Thank you. Happy to take questions. And once more, the book is free at marl-book.com.

*Possible questions, with short answers:*

- *"Why not treat the other agents as part of the environment and use ordinary RL?"* You can; that's called independent learning, and it often works reasonably well in practice. But the environment you see is then non-stationary (slide 44), so the guarantees from single-agent RL no longer apply. The book covers it in §5.3.2.
- *"Is MARL just game theory?"* Game theory gives the models and the solution concepts. MARL adds learning from experience, when the agents don't know the model (slide 56).
- *"Which reward type is hardest?"* The book singles out general-sum, the mixed-motive case like driving, as among the most challenging.

### Slide 62: References

*Leave this slide up during questions, or skip it.*
