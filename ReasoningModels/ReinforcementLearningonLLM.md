

## What is Reinforcement Learning (RL)?

Imagine you're training a dog. You want to teach it to sit. You might say "Sit!" and then, if the dog sits, you give it a treat and praise. If it doesn't sit, you might gently guide it or just try again. Over time, the dog learns to associate sitting with the positive reward (treat and praise) and is more likely to sit when you say "Sit!" again. In reinforcement learning, we refer to this feedback as a **reward**.

That, in a nutshell, is the basic idea behind Reinforcement Learning! Instead of a dog, we have a **language model** (in reinforcement learning, we call it an **agent**), and instead of you, we have the **environment** that gives feedback.

![RL terms Process](https://huggingface.co/reasoning-course/images/resolve/main/grpo/3.jpg)

Let's break down the key pieces of RL:

### Agent

This is our learner. In the dog example, the dog is the agent. In the context of LLMs, the LLM itself becomes the agent we want to train. The agent is the one making decisions and learning from the environment and its rewards.

### Environment

This is the world the agent lives in and interacts with. For the dog, the environment is your house and you. For an LLM, the environment is a bit more abstract – it could be the users it interacts with, or a simulated scenario we set up for it. The environment provides feedback to the agent. 

### Action

These are the choices the agent can make in the environment. The dog's actions are things like "sit", "stand", "bark", etc. For an LLM, actions could be generating words in a sentence, choosing which answer to give to a question, or deciding how to respond in a conversation.

### Reward

This is the feedback the environment gives to the agent after it takes an action. Rewards are usually numbers. 

**Positive rewards** are like treats and praise – they tell the agent "good job, you did something right!". 

**Negative rewards** (or penalties) are like a gentle "no" – they tell the agent "that wasn't quite right, try something else". For the dog, the treat is the reward. 

For an LLM, rewards are designed to reflect how well the LLM is doing at a specific task – maybe it's how helpful, truthful, or harmless its response is.

### Policy

This is the agent's strategy for choosing actions. It's like the dog's understanding of what it should do when you say "Sit!". In RL, the policy is what we're really trying to learn and improve. It's a set of rules or a function that tells the agent what action to take in different situations. Initially, the policy might be random, but as the agent learns, the policy becomes better at choosing actions that lead to higher rewards.

## The RL Process: Trial and Error

![RL Process](https://huggingface.co/reasoning-course/images/resolve/main/grpo/1.jpg)

Reinforcement Learning happens through a process of trial and error:

| Step | Process | Description |
|------|---------|-------------|
| 1. Observation | The agent observes the environment | The agent takes in information about its current state and surroundings |
| 2. Action | The agent takes an action based on its current policy | Using its learned strategy (policy), the agent decides what to do next |
| 3. Feedback | The environment gives the agent a reward | The agent receives feedback on how good or bad its action was |
| 4. Learning | The agent updates its policy based on the reward | The agent adjusts its strategy - reinforcing actions that led to high rewards and avoiding those that led low rewards |
| 5. Iteration | Repeat the process | This cycle continues, allowing the agent to continuously improve its decision-making |

Think about learning to ride a bike. You might wobble and fall at first (negative reward!). But when you manage to balance and pedal smoothly, you feel good (positive reward!). You adjust your actions based on this feedback – leaning slightly, pedaling faster, etc. – until you learn to ride well. RL is similar – it's about learning through interaction and feedback.

## Role of RL in Large Language Models (LLMs)

Now, why is RL so important for Large Language Models? 

Well, training really good LLMs is tricky. We can train them on massive amounts of text from the internet, and they become very good at predicting the next word in a sentence. This is how they learn to generate fluent and grammatically correct text, as we learned in [chapter 2](/course/chapter2/1).

However, just being fluent isn't enough. We want our LLMs to be more than just good at stringing words together. We want them to be:

* **Helpful:** Provide useful and relevant information.  
* **Harmless:** Avoid generating toxic, biased, or harmful content.  
* **Aligned with Human Preferences:** Respond in ways that humans find natural, helpful, and engaging.

Pre-training LLM methods, which mostly rely on predicting the next word from text data, sometimes fall short on these aspects.

Whilst supervised training is excellent at producing structured outputs, it can be less effective at producing helpful, harmless, and aligned responses. We explore supervised training in [chapter 11](/course/chapter11/1).

Fine-tuned models might generate fluent and structured text that is still factually incorrect, biased, or doesn't really answer the user's question in a helpful way.

**Enter Reinforcement Learning\!** RL gives us a way to fine-tune these pre-trained LLMs to better achieve these desired qualities. It's like giving our LLM dog extra training to become a well-behaved and helpful companion, not just a dog that knows how to bark fluently\!

## Reinforcement Learning from Human Feedback (RLHF)

A very popular technique for aligning language models is **Reinforcement Learning from Human Feedback (RLHF)**. In RLHF, we use human feedback as a proxy for the "reward" signal in RL. Here's how it works:

1. **Get Human Preferences:** We might ask humans to compare different responses generated by the LLM for the same input prompt and tell us which response they prefer. For example, we might show a human two different answers to the question "What is the capital of France?" and ask them "Which answer is better?".

2. **Train a Reward Model:** We use this human preference data to train a separate model called a **reward model**. This reward model learns to predict what kind of responses humans will prefer. It learns to score responses based on helpfulness, harmlessness, and alignment with human preferences.

3. **Fine-tune the LLM with RL:** Now we use the reward model as the environment for our LLM agent. The LLM generates responses (actions), and the reward model scores these responses (provides rewards). In essence, we're training the LLM to produce text that our reward model (which learned from human preferences) thinks is good.

![RL Basic Concept](https://huggingface.co/reasoning-course/images/resolve/main/grpo/2.jpg)  

From a general perspective, let's look at the benefits of using RL in LLMs:

| Benefit | Description |
|---------|-------------|
| Improved Control | RL allows us to have more control over the kind of text LLMs generate. We can guide them to produce text that is more aligned with specific goals, like being helpful, creative, or concise. |
| Enhanced Alignment with Human Values | RLHF, in particular, helps us align LLMs with complex and often subjective human preferences. It's hard to write down rules for "what makes a good answer," but humans can easily judge and compare responses. RLHF lets the model learn from these human judgments. |
| Mitigating Undesirable Behaviors | RL can be used to reduce negative behaviors in LLMs, such as generating toxic language, spreading misinformation, or exhibiting biases. By designing rewards that penalize these behaviors, we can nudge the model to avoid them. |

Reinforcement Learning from Human Feedback has been used to train many of the most popular LLMs today, such as OpenAI's GPT-4, Google's Gemini, and DeepSeek's R1. There are a wide range of techniques for RLHF, with varying degrees of complexity and sophistication. In this chapter, we will focus on Group Relative Policy Optimization (GRPO), which is a technique for RLHF that has been shown to be effective at training LLMs that are helpful, harmless, and aligned with human preferences.

## Why should we care about GRPO (Group Relative Policy Optimization)?

There are many techniques for RLHF but this course is focused on GRPO because it represents a significant advancement in reinforcement learning for language models.

Let's briefly consider two of other popular techniques for RLHF:

- Proximal Policy Optimization (PPO)
- Direct Preference Optimization (DPO)

Proximal Policy Optimization (PPO) was one of the first highly effective techniques for RLHF. It uses a policy gradient method to update the policy based on the reward from a separate reward model.

Direct Preference Optimization (DPO) was later developed as a simpler technique that eliminates the need for a separate reward model using preference data directly. Essentially, framing the problem as a classification task between the chosen and rejected responses.

<Tip>

DPO and PPO are complex reinforcement learning algorithms in their own right, which we will not cover in this course. If you're interested in learning more about them, you can check out the following resources:

- [Proximal Policy Optimization](https://huggingface.co/docs/trl/main/en/ppo_trainer)
- [Direct Preference Optimization](https://huggingface.co/docs/trl/main/en/dpo_trainer)

</Tip>

Unlike DPO and PPO, GRPO groups similar samples together and compares them as a group. The group-based approach provides more stable gradients and better convergence properties compared to other methods.

GRPO does not use preference data like DPO, but instead compares groups of similar samples using a reward signal from a model or function.

GRPO is flexible in how it obtains reward signals - it can work with a reward model (like PPO does) but doesn't strictly require one. This is because GRPO can incorporate reward signals from any function or model that can evaluate the quality of responses.

For example, we could use a length function to reward shorter responses, a mathematical solver to verify solution correctness, or a factual correctness function to reward responses that are more factually accurate. This flexibility makes GRPO particularly versatile for different types of alignment tasks.

---


