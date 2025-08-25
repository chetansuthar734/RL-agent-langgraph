# RL-agent-langgraph

Language Agent Tree Search (LATS) is a general LLM agent search algorithm that combines reflection/evaluation and search (specifically Monte-Carlo trees search) to achieve better overall task performance compared to similar techniques like ReACT, Reflexion, or even Tree of Thoughts. It adopts a standard reinforcement learning (RL) task framing, replacing the RL agents, value functions, and optimizer all with calls to an LLM. This is meant to help the agent adapt and problem solve for complex tasks, avoiding getting stuck in repetitive loops.



The search has four main steps:

    [1].Select: pick the best next actions based on the aggregate rewards from step (2) below. Either respond (if a solution is found or the max search depth is                reached) or continue searching.
    [2].Expand and simulate: generate N (5 in our case) potential actions to take and execute them in parallel.
    [3].Reflect + evaluate: observe the outcomes of these actions and score the decisions based on reflection (and possibly external feedback)
    [4].Backpropagate: update the scores of the root trajectories based on the outcomes.

If the agent has a tight feedback loop (through high quality environment rewards or reliable reflection scores), the search is able to accurately distinguish between different action trajectories and pick the best path. The final trajectory can then be saved to external memory (or used for model fine-tuning) to improve the model in the future.

The "selection" step picks the node with the highest upper confidence bound (UCT), which just balances the expected reward (the first term) with an incentive to explore new paths (the second term).
                         
UCT = value/visits  +  c√(ln(parent.visits)/visits) 
​
