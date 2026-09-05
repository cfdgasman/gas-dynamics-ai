# Corrections applied

Record of a self-review pass over the draft. Everything below has been applied
to `ai-in-gas-dynamics.tex` and the PDF rebuilt (33 pages, clean build, no
undefined references).

The tabulated numbers were **not** affected. The exact Fanno ratios at
M = 0.3, 0.6, 1.5, 2.5, 3.0 were recomputed independently and agree with the
tables to six digits, and the ODE right-hand side in eq. `(fanno_ode)` is the
correct derivative of Φ(M). The problems were all in the prose and in two stated
formulas — the results and figures stand as they were.

---

## 1. Friction-factor convention

The text had declared `f` as the **Darcy** friction factor with `τ_w = f ρu²/8`,
while using `4fL*/D` for the dimensionless friction length throughout — in the
tables, the figure axis labels, and the factor of 4 in the Fanno ODE. `4fL*/D`
is the **Fanning** form; with Darcy `f` the relation reads `fL*/D`. The
declaration and the rest of the chapter disagreed.

Fixed by correcting the declaration rather than the usage, since `4fL*/D` is
baked into the figures and the ODE: `f` is now stated as the Fanning friction
factor with `τ_w = f ρu²/2`.

## 2. Momentum equation

Was:

```
dp + ρu du + (4 f ρu² / D) dx = 0
```

A force balance on an element of a circular duct gives the wall term as `4τ_w/D`
per unit volume, which with the Fanning `τ_w` is `2 f ρu²/D`. The equation now
shows the `4τ_w/D` step explicitly before substituting, so the factor is visible
rather than asserted, and a following sentence ties it to the factor of four in
`4fL*/D`.

## 3. The "isentropic-stagnation expressions" block — the substantive one

Fanno flow is **adiabatic but not isentropic**: friction generates entropy, so
stagnation temperature is conserved while stagnation pressure falls. The static
pressure therefore does not follow the isentropic relation that was printed
there.

Replaced with the sonic-referenced Fanno ratios actually used by the tables and
figures:

```
T/T*   = (γ+1) / (2 + (γ−1)M²)
P/P*   = (1/M) · sqrt[ (γ+1) / (2 + (γ−1)M²) ]
ρ/ρ*   = (1/M) · sqrt[ (2 + (γ−1)M²) / (γ+1) ]
P₀/P₀* = (1/M) · [ (2 + (γ−1)M²) / (γ+1) ] ^ ((γ+1) / (2(γ−1)))
```

Added an explicit statement that these are *not* isentropic relations, and that
constant `T₀` with monotonically decreasing `P₀` is the defining thermodynamic
signature of the Fanno process — which is the reason the isentropic pressure
formula does not apply. The closing sentence now describes the forward map as
`M → (T/T*, P/P*, P₀/P₀*, 4fL*/D)`, matching the four outputs used from the
modelling section onward.

## 4. Second occurrence of the same word

"recovered from the analytical **isentropic** relations" → "the analytical
**Fanno** relations". The rest of that sentence was already correct: each ratio
genuinely is a closed-form function of M alone for a calorically perfect gas.

## 5. Where the initial profile is compressive

The viscous Burgers subsection said the initial condition is "compressive on
x ∈ [0, 1]". For `u = −sin(πx)`, `u_x = −π cos(πx)` is negative only on
`|x| < 1/2`. Now reads "compressive near `x = 0`, where `∂ₓu < 0` for
`|x| < 1/2`", which also matches how the inviscid subsection describes the
ridge.

## 6. "Triplet"

"Fanno and Burgers form a complementary **triplet** of demonstrators" — two
problems, three demonstrations, so the noun did not agree with its subject. Now
"the three demonstrations form a complementary set".

## 7. `references.bib`

Was badly out of sync: 8 entries against the 25 printed in the manuscript, four
keys that are never cited (`PINN_arxiv_review2024`, `PhysicsInformedFlows2025`,
`PhysicsInformedMicroflows2025`, `Raissi2017PINNPartI`), and an `Anderson2021`
entry naming a different book than the bibliography prints. Regenerated to
mirror the printed bibliography exactly.

The manuscript still uses its own inline `thebibliography` and does not read the
`.bib` at build time — the file is kept for reuse, so the two need to be updated
together.

---

## Left alone

- **Inviscid boundary conditions.** Imposing `u(±1, t) = 0` at both ends of a
  first-order hyperbolic problem is formally over-specified — only inflow
  boundaries take data. Harmless here, because the characteristics at `x = ±1`
  are vertical (`u = 0` there), so nothing is over-constrained in practice. Left
  as written; worth a half-sentence someday to pre-empt the question.

- **`fig_poly_ridge_error.png`** is an unused duplicate of the `.pdf`; only the
  PDF is pulled in by `\includegraphics`. Kept, but it can go.
