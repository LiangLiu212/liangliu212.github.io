---
layout: post
title: "Visualizing Intranuclear Cascades: INCL and GENIE-INCL Simulations on Argon"
date: 2026-05-16
---

The animations below show the intranuclear cascade inside an argon-40 nucleus,
as simulated by two different setups: the standalone INCL model and the
GENIE-INCL interface currently under development.

Each animated point represents a particle (proton or neutron) tracked through
the nuclear medium step by step. The cascade begins when an incoming particle
strikes a nucleon inside the nucleus, producing a shower of secondary particles
that can re-scatter, escape, or be absorbed before the nucleus reaches its
final state.

---

## Standalone INCL — Argon cascade

INCL (Liège Intranuclear Cascade) propagates each nucleon classically through
a local nuclear potential, handling binary collisions, Pauli blocking, and
coalescence to produce light clusters.

<div style="width:100%; margin: 1em 0;">
  <iframe src="/assets/animations/incl.html"
          width="100%" height="600"
          style="border:none; border-radius:6px; background:#fff;">
  </iframe>
</div>

---

## GENIE-INCL — Neutrino-argon cascade

In this simulation the primary neutrino-argon interaction vertex is generated
by GENIE, and the outgoing hadrons are then handed to INCL for final-state
propagation. Interfacing the two codes gives a self-consistent nuclear
description: the same Fermi-gas model and binding energy used for the primary
vertex drive the cascade that follows.

<div style="width:100%; margin: 1em 0;">
  <iframe src="/assets/animations/genie-incl.html"
          width="100%" height="600"
          style="border:none; border-radius:6px; background:#fff;">
  </iframe>
</div>

---

Comparing the two animations highlights how the initial-state conditions set
by GENIE (neutrino kinematics, struck-nucleon momentum) shape the cascade
topology relative to a hadron-induced reaction.
