# Hippocampus–Neocortex Interaction: Fast vs Slow Learning and a Possible Extension to Monty

## Purpose

This note documents ongoing thoughts about hippocampus–neocortex interaction,
with a focus on fast vs slow learning, episodic binding, and how these ideas
might translate into a computational extension of the Thousand Brains / Monty
framework.

The goal is exploratory: to clarify understanding, identify gaps in current
models, and outline a minimal, technically feasible abstraction suitable for
engineering experimentation.

---

## 1. Biological / Conceptual Background

In the Thousand Brains framework, cortical columns (or *learning modules* in
Monty terms) can be viewed as learning local sensorimotor models of the world.
Each module integrates sensory input with a location signal and converges on
hypotheses such as object identity, pose, or other latent structure. These
models are learned slowly and reused across many experiences.

A key distinction is the classical **fast vs slow learning** split:

- **Neocortex**  
  Slow, statistical learning; stable and reusable world models; gradual
  generalization.

- **Hippocampus**  
  Fast learning; episodic binding; rapid association of co-occurring cortical
  states and their temporal relationships.

Importantly, fast learning in the hippocampus relies on specialized mechanisms
(pattern separation, auto-association, replay), rather than simply higher
learning rates.

In this view, the hippocampus does **not** store cortical models themselves.
Instead, it stores **indices** that bind together *specific constellations of
cortical states* that co-occurred during an experience. Activating such an index
later does not rewind cortex to a previous internal state, but instead biases
ongoing cortical inference toward a functionally compatible configuration,
resulting in approximate **reinstatement**.

---

## 2. Intuitive Walkthrough: Episodic Memory, Replay, and Simulation

This section provides a simple, concrete walkthrough of how cortical learning
modules interact with hippocampal circuitry (DG–CA3–CA1) to support three closely
related phenomena:

1. **episodic memory formation** (what happened),
2. **replay** (remembering what happened),
3. **simulation of the future** (imagining what could happen).

Crucially, these are not different mechanisms, but different *uses* of the same
hippocampal dynamics.

---

### 2.1 Perception: Cortical Columns in the Present Moment

Consider a simple experience:

> **Touching and lifting a dumbbell.**

At that moment, multiple cortical columns are active across different sensory
regions. Each column uses its own sensor and location signal to infer the same
object.

For example:

- **Somatosensory columns**  
  Infer object structure from pressure, texture, and weight at specific contact
  locations.

- **Visual columns** (if available)  
  Infer the same object using visual features at different locations on the
  object.

- **Auditory columns** (if relevant)  
  Infer object-related properties from sounds produced during interaction.

According to the Thousand Brains Theory:

- Each column learns a **complete object model**, not just a feature.
- All columns use a **location signal** to anchor sensation to the object.
- Object identity emerges through **consensus across columns**.

Each column produces a momentary **vote** (e.g. an SDR-like output) expressing
its current inference.

Important clarifications:

- These votes are **not memories**.
- They exist only while the interaction is ongoing.
- The same columns and models are reused across many experiences.

At this moment, cortex jointly settles into a coherent interpretation, which can
be summarized informally as:

> “I am gripping and lifting a heavy cylindrical object.”

---

### 2.2 Dentate Gyrus (DG): Creating an Episodic Index

The joint pattern of cortical votes is sent to the hippocampus via the entorhinal
cortex.

The **dentate gyrus (DG)** performs **pattern separation**:

- It converts dense cortical activity into a **sparse, unique activation**.
- Similar cortical states produce **distinct DG patterns**.
- The DG pattern does **not encode sensory details or object models**.

Instead, the DG pattern simply means:

> “This specific constellation of cortical activity occurred.”

This sparse pattern is an **episodic index**.

At this moment, an **episode is created**.

Nothing about the dumbbell’s shape, weight, or texture is stored in DG.
Only a pointer-like index referring to the joint cortical state is created.

---

### 2.3 CA3: Binding and Linking Episodes

The DG index activates neurons in **CA3**, which has strong recurrent
connectivity.

CA3 performs two functions:

1. **Binding**  
   The DG-driven pattern becomes a stable attractor representing this episode.

2. **Linking**  
   CA3 learns transitions between episodic indices over time.

For example, during the interaction CA3 may learn:

- reaching → gripping → lifting → holding

CA3 therefore stores **relations between episodes**, not sensory content.

You can think of CA3 as learning a **graph of experiences**, where:

- nodes are episodic indices,
- edges encode temporal transitions.

---

### 2.4 CA1: Biasing Cortical Inference (Reinstatement)

**CA1** receives two inputs:

- predictions from CA3 (which episodic index is active or expected),
- current cortical activity via the entorhinal cortex.

CA1 effectively asks:

> “Do hippocampal expectations match what cortex is currently inferring?”

When an episodic index is active, CA1 sends feedback signals to cortex that:

- boost cortical populations that participated in the original episode,
- suppress incompatible interpretations.

This does **not** restore cortex to a past internal state.

Instead, it biases ongoing inference, allowing cortex to reconstruct a
functionally similar interpretation using its current models.

This process is called **reinstatement**, not retrieval.

---

### 2.5 Case 1: Episodic Memory (What Happened)

**Situation:**  
You experience lifting the dumbbell.

**Mechanism:**

- Cortex produces object-related votes.
- DG creates a new episodic index.
- CA3 binds and links it to neighboring episodes.

**Result:**  
An episode is stored as an index and its relationships.

This happens automatically during experience.

---

### 2.6 Case 2: Replay (Remembering the Past)

**Situation:**  
Later, you think:

> “That moment when I lifted the dumbbell.”

**What changes:**  
Nothing in the mechanism — only the entry point.

**Mechanism:**

- A partial cortical cue activates the same DG index.
- CA3 completes the attractor.
- CA3 reactivates linked indices in sequence.
- CA1 biases cortex to reinstate compatible interpretations.

**Result:**  
You recall the event.

Replay is:

- fast,
- compressed in time,
- driven by learned episodic transitions.

---

### 2.7 Case 3: Reorganization and Future Simulation (What Could Happen)

**Situation:**  
You imagine:

> “What if the dumbbell slips?”

**What changes:**  
Again, nothing fundamental — only how CA3 traverses the graph.

**Mechanism:**

- CA3 samples alternative paths through episodic indices.
- Paths may:
  - branch,
  - skip steps,
  - combine indices from different experiences.

For example:

- gripping → slipping → dropping → impact

Even if that exact sequence never occurred.

CA1 biases cortex to evaluate these sequences.
Prefrontal and reward systems assess outcomes.

**Result:**  
Simulation of possible futures.

---

### 2.8 What Is the Same, What Is Different

**Same mechanism in all cases:**

- DG indexing
- CA3 attractor dynamics and transitions
- CA1 biasing of cortical inference

**What differs:**

- how replay is initiated (external input, memory cue, goal),
- how CA3 traverses the episodic graph,
- how results are evaluated downstream.

There is no separate “memory”, “imagination”, or “future” system.

They all emerge from the **same hippocampal dynamics interacting with cortex**.

---

## 3. A Dynamical / Mathematical Framing

Each cortical learning module can be treated as a **dynamical system** that
maintains an internal state and produces predictions about the world.

Let $S_i(t)$ denote the internal state of cortical module $i$ at time $t$.
The state evolves as:

$$
S_i(t+1) = F_i\big(S_i(t), x_i(t), \theta_i(t)\big)
$$

where:
- $x_i(t)$ is sensory or motor input,
- $\theta_i(t)$ are slowly changing parameters learned from experience.

Each module produces an observable output:

$$
y_i(t) = G_i\big(S_i(t)\big)
$$

which in Monty corresponds to an object vote, confidence, or similar
representation (biologically, something like an SDR).

Crucially:

- Internal cortical states $S_i(t)$ are transient and are not stored explicitly.
- Cortical models are **not time-indexed memories**, but predictive structures
  whose parameters implicitly reflect past experience.

The hippocampus therefore does **not** store $S_i(t)$. Instead, it stores a
sparse episodic index $H_k$ such that:

$$
H_k \Rightarrow \{\, y_i(t_k) \,\}_{i \in \mathcal{C}_k}
$$

where $\mathcal{C}_k$ is the set of cortical modules jointly active during
episode $k$.

Later, reactivation of $H_k$ biases cortical dynamics, leading to a reconstructed
state:

$$
\hat{S}_i \approx S_i(t_k)
$$

This reconstruction is approximate and context-dependent, but sufficient to
support recall, prediction, and imagination.

Under this view, **time is represented in the hippocampus through the sequencing
of indices**, not through time-indexed cortical models.

---

## 4. What Appears Missing in Current Thousand Brains / Monty

As currently implemented, Monty captures:

- slowly evolving cortical-like models,
- local inference within modules,
- consensus across modules.

However, it appears to lack:

- an explicit mechanism for **episodic indexing and reinstatement**,
- binding of distributed module outputs into temporally ordered episodes,
- **replay of trajectories** of episodic indices that could support recall,
  prediction, or imagination.

Because learning modules continuously overwrite internal state, cortex alone
cannot retrieve or reason over specific past experiences. As a result, reasoning
operates primarily on the current configuration of models, rather than over
episodic history.

---

## 5. Engineering Observation: A Simpler Abstraction Is Possible

In biological systems, reinstating past cortical configurations relies on
hippocampal attractor dynamics (e.g. DG–CA3–CA1 loops). In Monty, however,
learning modules already expose explicit outputs $y_i(t)$.

This suggests a minimal **engineering abstraction** of hippocampal indexing:

- Treat joint module outputs as episodic observations.
- Store selected tuples $(i, y_i)$ indexed by episode identifiers.
- Represent episodes as ordered sequences of such indices:

$$
E_k = \{\, (i, y_i(t_k)) \mid i \in \mathcal{C}_k \,\}
$$

Replay can then be approximated by re-injecting these stored outputs as biases,
priors, or contextual constraints on ongoing inference, rather than by rewinding
internal module state.

This abstraction does not claim biological equivalence, but aims to replicate
the **computational role** of the hippocampus: fast episodic binding, sequencing,
and index-driven reinstatement.

---

## 6. Why This Matters

Adding such a mechanism would allow Monty to:

- reason over **episodic trajectories** rather than only current state,
- support memory-guided prediction and simulation of future outcomes,
- enable counterfactual reasoning and controlled recombination,
- provide a functional bridge between **fast (episodic)** and **slow (semantic)**
  learning systems.

Without this, Monty remains strong at perception and recognition, but limited in
temporal abstraction, planning, and imagination.

---

## 7. Open Questions

- What is the minimal episodic indexing and replay mechanism needed to support
  these capabilities?
- How should replay be constrained to balance novelty and coherence?
- How should hippocampal replay interact with goal-directed attention and
  evaluation mechanisms?

These questions are intended as starting points for further discussion and
experimentation.