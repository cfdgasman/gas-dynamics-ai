# Artificial Intelligence in Gas Dynamics

A working draft exploring how machine learning and physics-informed
neural networks can complement classical compressible-flow analysis.

> **About this version.** This is my own working draft — written, typeset and
> compiled by me from my own notes and computations. It is shared here as a
> personal record and study resource. It has not been through editorial or peer
> review, and it is not a publisher's or editor's version of anything. If this
> material is ever developed further, any released version would be produced
> separately and may differ substantially from what is here. Nothing in this
> repository should be read as a final or authorised text.

---

## What's here

| File | Description |
|---|---|
| `ai-in-gas-dynamics.tex` | LaTeX source of the draft (single-file, self-contained bibliography) |
| `ai-in-gas-dynamics.pdf` | Current build — 33 pages |
| `references.bib` | BibTeX entries, kept alongside the manuscript for reuse |
| `fig_poly_ridge_*.pdf` | Regression-baseline figures |
| `fig_nn_*.pdf` | Supervised surrogate-network figures |
| `fig_pinn_*.pdf` | Fanno PINN figures |
| `fig_burgers_*.pdf` | Inviscid and viscous Burgers PINN figures |

Start with `ai-in-gas-dynamics.pdf` if you just want to read it.

---

## Scope of the draft

The draft works through two canonical problems and, on each, compares the same
family of approaches against an analytical reference.

**Fanno flow** — steady, one-dimensional, adiabatic duct flow with wall
friction. It has a closed-form solution and a singular sonic point, which makes
it a convenient controlled benchmark. Three models are developed in sequence:

1. **Polynomial ridge regression** on physics-motivated features
   ($M$, $M^{2}$, $1/M$, $1/M^{2}$, $\ln M$).
2. **A supervised surrogate network** trained on analytically generated labels.
3. **A physics-informed network (PINN)** trained on the Fanno ODE residual
   alone, with the sonic condition $F(1)=0$ built into the architecture via the
   output transform $\hat F(M) = (M-1)\,g_\theta(M)$.

**Burgers' equation** — unsteady, one-dimensional, and the simplest setting in
which a shock forms spontaneously from smooth data. Two demonstrations:

1. **Inviscid**, on the pre-breaking window $t \in [0, 0.30]$, checked against
   the characteristic solution obtained by Newton inversion of
   $x = x_0 - \sin(\pi x_0)\,t$.
2. **Viscous**, with $\nu = 0.01/\pi$ on $t \in [0,1]$, checked against a
   Cole–Hopf reference. This follows the configuration that has become a common
   PINN benchmark in the literature.

---

## Themes the draft tries to bring out

- **Feature choice does a lot of the work.** Because the engineered features
  mirror the analytical structure of the Fanno function, even linear models fit
  well; the exercise is a reminder that this advantage disappears when the
  closed form is not known in advance.

- **Hard vs. soft constraints.** Fanno admits a hard boundary condition through
  an output transform. Burgers' conditions live on extended manifolds, so soft
  penalties are used instead. The trade-off between the two is discussed rather
  than settled.

- **Residual scaling.** The Fanno right-hand side varies over several orders of
  magnitude across the Mach range, so the residual is rescaled by
  $1/(|\mathrm{RHS}|+1)$ to keep the subsonic and supersonic branches
  comparably weighted.

- **Singularities and discontinuities are different problems.** A smooth
  $\tanh$ network copes with the singular sonic point but cannot represent a
  true discontinuity — which is why the inviscid demonstration stops short of
  the breaking time and the viscous case does not need to.

---

## Reproducing the document

The manuscript builds with a standard LaTeX distribution (built here with
MiKTeX):

```bash
latexmk -pdf ai-in-gas-dynamics.tex
```

All figures are committed as PDFs, so no computation is needed to rebuild the
document. The scripts that produced the figures are not included in this
repository.

---

## Caveats

- The draft is unreviewed and may contain errors of derivation, notation or
  numerical reporting.
- Reported error figures come from single training runs and are illustrative
  rather than statistically characterised.
- PINN results are sensitive to seeding, sampling and optimiser schedule; the
  numbers should be read as representative, not as benchmarks.

---

## Sources and attribution

The underlying gas dynamics is standard, long-established material, and the learning
methods are established techniques from the published literature. Where the
draft leans on a specific formulation — the PINN framework, the Cole–Hopf
transformation, the viscous Burgers benchmark configuration — the original work
is cited in the bibliography, and readers should go to those sources rather
than to this draft. All text and figures here are my own; no third-party
material is reproduced.

---

*Personal draft. Last built September 2026.*
