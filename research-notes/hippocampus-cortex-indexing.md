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

## 2. A Dynamical / Mathematical Framing

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

## 3. What Appears Missing in Current Thousand Brains / Monty

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

## 4. Engineering Observation: A Simpler Abstraction Is Possible

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

## 5. Why This Matters

Adding such a mechanism would allow Monty to:

- reason over **episodic trajectories** rather than only current state,
- support memory-guided prediction and simulation of future outcomes,
- enable counterfactual reasoning and controlled recombination,
- provide a functional bridge between **fast (episodic)** and **slow (semantic)**
  learning systems.

Without this, Monty remains strong at perception and recognition, but limited in
temporal abstraction, planning, and imagination.

---

## 6. Open Questions

- What is the minimal episodic indexing and replay mechanism needed to support
  these capabilities?
- How should replay be constrained to balance novelty and coherence?
- How should hippocampal replay interact with goal-directed attention and
  evaluation mechanisms?

These questions are intended as starting points for further discussion and
experimentation.