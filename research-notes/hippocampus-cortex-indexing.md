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
internal models evolve over time as learning proceeds.

A key distinction is the classical **fast vs slow learning** split:

- **Neocortex**  
  Slow, statistical learning; stable object/world models; gradual generalization.

- **Hippocampus**  
  Fast learning; episodic binding; rapid association of co-occurring cortical
  states.

Importantly, fast learning in the hippocampus appears to rely on specialized
mechanisms (pattern separation, auto-association, replay), rather than simply
higher learning rates.

In this view, the hippocampus does **not** store cortical models themselves.
Instead, it stores **indices** that bind together *specific cortical states*
that co-occurred in time. Activating such an index later leads to
**reinstatement** of an approximate cortical state corresponding to a past
episode. This reinstatement does not rewind cortex exactly, but biases it back
toward a functionally equivalent region of its state space.

---

## 2. A Dynamical / Mathematical Framing

A useful abstraction is to treat each cortical learning module as a
**dynamical system**.

Let $S_i(t)$ denote the internal state of cortical module $i$ at time $t$.
The state evolves as:

$$
S_i(t+1) = F_i\big(S_i(t), x_i(t), \theta_i(t)\big)
$$

where:
- $x_i(t)$ is sensory or motor input,
- $\theta_i(t)$ are slowly changing parameters (learning).

Each module produces an observable output:

$$
y_i(t) = G_i\big(S_i(t)\big)
$$

which in Monty corresponds to an object vote, confidence, or similar
representation (biologically, something like an SDR).

Crucially:

- $S_i(t)$ evolves continuously and is not stored explicitly.
- Overwriting of internal state is unavoidable due to learning.

In this framing, the hippocampus does **not** store $S_i(t)$. Instead, it
stores a sparse index $H_k$ such that:

$$
H_k \Rightarrow \{\, y_i(t_k) \,\}_{i \in \mathcal{C}_k}
$$

where $\mathcal{C}_k$ is the set of cortical modules active during episode $k$.

Later, activating $H_k$ induces a **reinstated state**:

$$
\hat{S}_i(t_k) \approx S_i(t_k)
$$

not exactly, but sufficiently to reproduce similar predictions and associations.
This approximation is biologically realistic and computationally sufficient.

Under this view, cortical models are best seen as **functions of time**, and the
hippocampus provides indexed access to *earlier evaluations of those functions*,
rather than frozen snapshots.

---

## 3. What Appears Missing in Current Thousand Brains / Monty

As currently implemented, Monty captures:

- evolving cortical-like models,
- local inference within modules,
- consensus across modules.

However, it appears to lack:

- a mechanism for **time-indexed reinstatement** of module outputs,
- explicit **episodic binding** across modules,
- **replay of trajectories** of internal model states.

Learning modules evolve, but earlier functional states are not accessible once
overwritten. As a result, reasoning operates only on the current configuration
of models, not on their history.

---

## 4. Engineering Observation: A Simpler Abstraction Is Possible

In biological systems, reinstating past cortical states requires complex
attractor dynamics. In Monty, however, learning modules already expose explicit
outputs $y_i(t)$.

This allows a minimal, engineering-friendly abstraction of hippocampal
indexing:

- For each learning module $i$, store $y_i(t)$ over time (or at selected
  timesteps).
- Represent episodes as ordered sets:

$$
E_k = \{\, (i, y_i(t_k)) \mid i \in \mathcal{C}_k \,\}
$$

- Implement replay by re-injecting these stored outputs as priors, constraints,
  or contextual signals for inference, rather than rewinding internal state
  exactly.

This approach does not aim to mechanistically replicate hippocampal circuitry,
but to replicate its **computational role**: fast episodic binding and
reinstatement.

---

## 5. Why This Matters

Adding such a mechanism would allow Monty to:

- reason over the **evolution** of its own internal representations,
- support **episodic memory** and replay,
- enable **counterfactual reasoning** and potentially creative recombination,
- provide a concrete bridge between **fast (episodic)** and **slow (semantic)**
  learning.

Without this, Monty remains strong at perception and recognition, but limited in
temporal abstraction and episodic reasoning.

---

## 6. Open Questions

- Does this interpretation of hippocampal function align with current
  neuroscientific understanding?
- Is the dynamical / index-based framing the right level of abstraction?
- What is the minimal replay and reinstatement mechanism needed to gain these
  capabilities in Monty?

These questions are intended as starting points for further discussion and
experimentation.