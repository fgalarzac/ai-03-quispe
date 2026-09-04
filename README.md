# Agentic Delegation and the Language Frontier of Software Developers

> **Paper:** Alexander Quispe & Kevin Xu (2026), *Agentic Delegation and the Language Frontier of Software Developers: A Model and Evidence from Claude Code on GitHub*, arXiv:2605.25438v2, July 8, 2026.
>
> **Core claim:** agentic coding assistants may expand the set of programming languages in which a developer can **produce** working software, even without evidence that the developer has learned those languages well enough to work in them unassisted.
>
> **Important limitation:** the empirical estimates are **event-time associations around first detectable Claude Code use, not definitive causal effects**. Adoption is voluntary and may coincide with project shocks that independently change the languages a developer uses.

An earlier draft circulated as **“Coding Beyond Your Training: Claude Code and the Technological Frontier of Software Developers.”** It is an earlier version of the same paper, not a separate study.

---

## TL;DR

The paper distinguishes two generations of AI coding assistance by **who executes the work**:

| Production mode | What the AI does | Main economic margin |
|---|---|---|
| **Solo (S)** | No AI execution | Developer works within her own language-specific skill |
| **Generation 1 / augmentation (C)** | Suggests code, explanations, and fixes that the developer must read, adapt, and run | Improves productivity mainly in **familiar languages** |
| **Generation 2 / delegation (D)** | Inspects repositories, edits files, runs commands/tests, reads errors, and iterates from a natural-language specification | Can lower the entry barrier into **unfamiliar languages** |

The model's central object is an **activation band**: opportunities that are not worthwhile under solo or conversational production, but become worthwhile once an agent can execute part of the language-specific work.

Empirically, around first detectable Claude Code adoption, the paper finds:

- **+2.53** monthly programming languages at adoption (SE 0.06; pre-adoption mean 0.90).
- **+1.19** newly-used languages at adoption (SE 0.05; pre-adoption flow 0.31).
- **+0.38** in language entropy at adoption (SE 0.01).
- **+1.60** cumulative languages at adoption (SE 0.05), with the stock continuing to rise afterward — but this outcome is treated as **descriptive** because its pre-trends are not clean.
- Roughly **two-thirds of the adoption-month language-count effect remains** when all Claude-co-authored commits are removed from outcome construction.
- First uses of unfamiliar languages are concentrated among **narrow-portfolio specialists**, consistent with the model's “headroom” mechanism.

The paper is careful not to claim that Claude Code **causes** these changes. The main unresolved threat is that a developer may adopt Claude precisely when starting an unfamiliar-language project.

---

## 1. Research question

The paper asks:

> **Do agentic coding assistants expand an individual developer's observed programming-language production frontier?**

The distinction is deliberately about **production**, not skill acquisition. A Python developer who directs and verifies an agent that successfully delivers a Rust project has expanded her **production frontier**, even if she still could not write the Rust code unassisted.

This is the key scope restriction in **Remark 1** of the paper:

- The outcome is the set of languages in which the developer can **ship work** under any available production mode.
- The model does **not** assume agentic adoption directly raises language-specific skill.
- Delegation is still joint production: the developer chooses the project, specifies and decomposes the task, verifies the output, and bears verification costs.

---

## 2. Why “agentic” AI is different

The paper's taxonomy is based on the **locus of execution**, not merely model quality or release date.

### Generation 1: augmentation

Examples include autocomplete copilots and conversational assistants such as GitHub Copilot and ChatGPT. They can suggest code, explain errors, or draft snippets, but the developer must still:

1. understand the suggestion,
2. adapt it to the repository,
3. place it in the right files,
4. run it,
5. diagnose failures, and
6. iterate manually.

The model therefore makes augmentation value proportional to existing language-specific skill.

### Generation 2: delegation

Examples include Claude Code, Devin, agentic IDE modes, Codex, and Gemini CLI. These tools can operate inside the development environment: inspect repositories, edit multiple files, run tests and commands, observe failures, and iterate.

That changes the developer's production menu from

$$
M^1 = \{S,C\}
$$

to

$$
M^2 = \{S,C,D\}.
$$

The empirical comparison is therefore **not “no AI” versus “AI.”** Conversational AI was already widely available during the pre-period. The relevant margin is **augmentation-only versus augmentation-plus-delegation**.

---

## 3. Model

### 3.1 Environment

Developers are indexed by $i$, programming languages by $k \in \{1,\dots,K\}$, and months by $t$.

Each developer has:

- a familiar-language set $K_i$,
- an unfamiliar-language set $U_i$,
- language-specific execution skill $s_{ik,t}\in[0,1]$,
- language-invariant general ability $a_i\ge0$, interpreted as the ability to **specify, decompose, and verify** work.

Each developer-language-month also has:

- opportunity value $\omega_{ik,t}$,
- activation cost $b_{ik,t}$,
- beliefs about the developer's productivity match with mean $\mu_{ik,t}$ and precision $\pi_{ik,t}$.

With CARA utility and risk-aversion coefficient $\rho$, a Normal payoff with mean $m$ and variance $\sigma^2$ has certainty equivalent

$$
CE = m - \frac{\rho\sigma^2}{2}.
$$

For readability, subscripts are suppressed below.

### 3.2 Three production modes

#### Solo production

$$
V^S = \omega + s\mu - \frac{\rho s^2}{2\pi} - b.
$$

#### Generation-1 augmentation

$$
V^C = V^S + \gamma s - r_C.
$$

Here $\gamma s$ is the augmentation gain and $r_C$ is the interaction cost.

#### Generation-2 delegation

$$
V^D = \omega + (1-\lambda)s\mu + \lambda a z(A) - \kappa(a,s) - r_D - b
- \frac{\rho}{2}\left[\frac{(1-\lambda)^2s^2}{\pi}+\sigma_D^2(a,s,A)\right].
$$

Where:

- $\lambda\in(0,1]$ is the share of execution delegated to the agent,
- $z(A)$ is agent competence, increasing in capability $A$,
- $a$ scales the value of agent execution through the developer's general specification/verification ability,
- $\kappa(a,s)$ is verification cost,
- $r_D$ is compute/delegation cost,
- $\sigma_D^2(a,s,A)$ is residual agent-error variance.

**Why this matters:** delegation substitutes agent execution for a share of the developer's language-specific execution, while introducing verification and residual-error costs.

---

## 4. Assumptions and entry thresholds

### Assumption 1 — augmentation requires a foothold

For unfamiliar languages,

$$
\gamma s-r_C\le0,
$$

while for familiar languages with higher skill $\bar s$,

$$
\gamma\bar s-r_C>0.
$$

This makes Generation 1 valuable on the intensive margin of known languages but insufficient, on impact, to lower the entry threshold for unfamiliar languages.

### Assumption 2 — verification technology

Verification costs and residual error are weakly lower for stronger developers, more familiar languages, and more capable agents:

$$
\kappa_a<0,\qquad \kappa_s\le0,
$$

$$
\frac{\partial \sigma_D^2}{\partial a}\le0,\qquad
\frac{\partial \sigma_D^2}{\partial s}\le0,\qquad
\frac{\partial \sigma_D^2}{\partial A}\le0.
$$

### Mode-specific thresholds

A mode is viable when its certainty-equivalent surplus is non-negative. Because each surplus is affine in $\omega$, this defines opportunity thresholds.

Solo:

$$
T^S=b-s\mu+\frac{\rho s^2}{2\pi}.
$$

Generation 1:

$$
T^C=T^S-(\gamma s-r_C),
$$

$$
T^1=\min\{T^S,T^C\}=T^S-\max\{0,\gamma s-r_C\}.
$$

For an unfamiliar language, Assumption 1 implies

$$
T^1=T^S.
$$

Delegation:

$$
T^D=b-(1-\lambda)s\mu-\lambda a z(A)+\kappa(a,s)+r_D
+\frac{\rho}{2}\left[\frac{(1-\lambda)^2s^2}{\pi}+\sigma_D^2(a,s,A)\right].
$$

Post-agent threshold:

$$
T^2=\min\{T^1,T^D\}\le T^1.
$$

---

## 5. The delegation advantage and the activation band

For an unfamiliar language, define the delegation advantage

$$
B\equiv T^1-T^D=T^S-T^D.
$$

Algebra gives

$$
B=
\lambda\left[a z(A)-s\mu\right]
-\kappa(a,s)-r_D
+\frac{\rho}{2}\left[\frac{(2\lambda-\lambda^2)s^2}{\pi}-\sigma_D^2(a,s,A)\right].
$$

The three components are:

1. **Execution substitution:** agent output replaces part of uncertain human execution.
2. **Verification and compute costs:** delegation is not free.
3. **Risk substitution:** human match uncertainty falls, but residual agent-error risk appears.

If $B>0$, delegation strictly lowers the unfamiliar-language entry threshold.

### Proposition 1 — weak frontier expansion

Because $M^1\subset M^2$, the developer can always ignore delegation. Therefore

$$
Z^2_{ik,t}\ge Z^1_{ik,t}
$$

for every developer-language-date realization, and hence

$$
N^2_{it}\ge N^1_{it}.
$$

The paper calls this result nearly mechanical.

### Proposition 2 — activation band for unfamiliar languages

For an unfamiliar language satisfying Assumption 1, if $B_{ik,t}>0$, then

$$
Z^2_{ik,t}-Z^1_{ik,t}
=
\mathbb{1}\left[T^D_{ik,t}\le \omega_{ik,t}<T^S_{ik,t}\right].
$$

So the economically interesting region is

$$
\boxed{T^D_{ik,t}\le \omega_{ik,t}<T^S_{ik,t}}.
$$

In words:

```text
Low opportunity value                         High opportunity value

inactive under both       ACTIVATION BAND             active under both
--------------------|==========================|-------------------------->
                   T^D                        T^S = T^1

Generation 2 can enter here; Generation 1 cannot.
```

With continuous conditional CDF $F_{ik,t}$, the activation probability is

$$
F_{ik,t}(T^S_{ik,t})-F_{ik,t}(T^D_{ik,t}),
$$

and expected language-count expansion is

$$
\mathbb{E}[N^2_{it}-N^1_{it}]
=
\sum_k\left[F_{ik,t}(T^1_{ik,t})-F_{ik,t}(T^2_{ik,t})\right]\ge0.
$$

---

## 6. Dynamic prediction: flows versus stocks

A newly-used language is a **flow**: the same language can be “new” only once. Cumulative languages are a **stock**.

### Proposition 3 — cumulative-language effect

Let $p^g_{ik}$ be the per-period first-use hazard under generation $g$. If $p^2_{ik}\ge p^1_{ik}$,

$$
\Delta C_i(s)
=
\sum_{k\in U_i}
\left[(1-p^1_{ik})^{s+1}-(1-p^2_{ik})^{s+1}\right]
\ge0.
$$

In the closed-frontier benchmark $p^1_{ik}=0<p^2_{ik}$, the cumulative effect is strictly increasing and concave over the observed horizon.

This yields the paper's stock-flow signature:

- **newly-used languages** can spike at adoption and then revert as the at-risk set is depleted;
- **cumulative languages** can continue rising because first-use events accumulate.

---

## 7. Heterogeneity: who should gain most?

Under an additional symmetry assumption across unfamiliar-language candidates, the model gives

$$
\mathbb{E}[E_i\mid a_i,U_i]=U_i\,p_i(a_i,A).
$$

This predicts larger extensive-margin expansion for developers with:

- more **headroom** ($U_i$): many unfamiliar languages available to enter;
- greater general ability $a_i$: lower verification costs and better use of agent execution.

The theoretical maximum therefore occurs for **high-ability specialists**.

Empirically, the paper finds strong support for the **headroom** part of the prediction: within both activity groups, narrow-portfolio specialists begin substantially more new languages than generalists at adoption. The **ability** margin is not cleanly identified by the paper's pre-adoption commit-volume proxy; lower-volume developers show larger measured effects, which the authors interpret as confounded by sparse observed histories rather than as a clean test of ability.

---

## 8. Empirical design

### 8.1 Detecting Claude Code adoption

Claude Code commits carry a machine-readable trailer of the form:

```text
Co-Authored-By: Claude
```

The paper harvests **7,786,771** Claude-co-authored commits from January 2025 through January 2026, covering **185,517 distinct authors**. A developer's adoption date is her **first detectable Claude-co-authored commit**.

Because true first use may precede the first public co-authored commit, the empirical design allows **one month of anticipation**.

### 8.2 Sample construction

The paper initially draws:

- **5,000 early adopters** with first Claude use in Q2–Q3 2025;
- **5,000 later adopters** in Q4 2025–Q1 2026 as not-yet-treated comparisons.

Sampling is stratified by adoption timing and Claude-commit intensity, requires at least five Claude-co-authored commits, and excludes obvious bot accounts.

Additional screening removes:

- 13 high-frequency single-repository automation accounts;
- developers with detectable pre-adoption use of competing agentic tools.

The reconstructed developer-month panel contains **5,825 developers** over 28 months. The estimation sample then requires at least one observable pre-adoption commit and no detectable competing-agent use before the developer's own adoption, yielding:

- **5,346 developers** total,
- **2,813 treated**,
- **2,533 not-yet-treated controls**,
- **149,688 developer-month observations**,
- January 2024 through April 2026.

### 8.3 Commit-level language measurement

GitHub commits do not carry a language label, so the paper reconstructs the outcome data from changed files.

Repository discovery combines:

1. GitHub's contribution API,
2. repositories appearing in the raw Claude-commit stream,
3. GH Archive public `PushEvent` records.

The resulting history covers roughly:

- **133,096 developer-repository pairs**,
- **3.15 million distinct commits**,
- **57.2 million changed files**.

Each file is assigned a language using GitHub Linguist's filename/extension rules, while generated, vendored, documentation, markup, data, and binary files are excluded from the programming-language outcomes.

### 8.4 Outcomes

The four primary language-portfolio outcomes are:

| Outcome | Interpretation | Model counterpart |
|---|---|---|
| **Monthly distinct languages** $N_{it}$ | Active languages in month $t$ | Active set / Eq. (9) |
| **Newly-used languages** $\Delta N^{new}_{it}$ | Languages absent from the developer's earlier observed history | Activation band / Proposition 2 |
| **Cumulative languages** $C_{it}$ | Running stock of languages ever observed | Proposition 3 |
| **Language entropy** $H_{it}$ | Balance/diversity of changed-file activity across languages | Secondary breadth measure |

Repository count and commit volume are treated as **activity diagnostics**, not frontier outcomes.

---

## 9. Estimation strategy

Adoption is staggered and treatment effects may vary by cohort and horizon, so the paper avoids conventional two-way fixed effects.

It uses the doubly robust **Callaway & Sant'Anna (2021)** group-time ATT estimator:

$$
ATT(g,t)
=
\mathbb{E}[Y_{it}(g)-Y_{it}(0)\mid G_i=g].
$$

Key implementation choices:

- **not-yet-treated** developers as comparisons;
- **one-month anticipation** window;
- **varying base period**;
- doubly robust estimation;
- **1,000 multiplier-bootstrap iterations**, clustered by developer;
- event-study aggregation by event time.

The design addresses staggered timing, cohort heterogeneity, negative-weight concerns from already-treated comparisons, and common calendar-time shocks. It does **not** eliminate selection on time-varying unobservables.

---

## 10. Main empirical results

### Adoption-month effects

| Outcome | ATT at $t=0$ | SE | Pre-adoption treated mean | Interpretation |
|---|---:|---:|---:|---|
| Monthly commits | 35.080 | 2.085 | 10.68 | Large activity increase |
| Repositories | 1.494 | 0.058 | 0.94 | Broader project activity |
| Programming languages | **2.528** | 0.063 | 0.90 | Large expansion in active language set |
| Language entropy | **0.382** | 0.009 | 0.15 | New languages carry meaningful work share |
| Newly-used languages | **1.193** | 0.051 | 0.31 | Direct evidence on the activation-band margin |
| Cumulative languages | **1.604** | 0.054 | 3.06 | Stock rises, but pre-trends make this descriptive |

### Dynamics

- **Programming languages:** +2.53 at adoption, +1.23 at $t=1$, +0.69 at $t=2$; significant for several months before decaying.
- **Newly-used languages:** +1.19 at adoption, +0.13 at $t=1$, statistically indistinguishable from zero thereafter — consistent with a first-use flow that rapidly exhausts available “new” entries.
- **Cumulative languages:** +1.60 at adoption, +1.89 at $t=1$, +2.07 at $t=2$; the stock keeps growing, but its pre-period coefficients are non-trivial, so the paper does not treat it as headline identified evidence.
- **Entropy:** +0.38 at adoption, +0.19 at $t=1$, +0.10 at $t=2$.

For the headline monthly-flow outcomes, the pre-period coefficients at $e\le-2$ are small and statistically insignificant. The paper also extends the pre-period to $e=-12$ for key outcomes and finds flat coefficients before the anticipation month.

---

## 11. Robustness and mechanism checks

The paper devotes substantial attention to the possibility that treatment and outcomes are mechanically linked because adoption is dated by a commit and commits contain languages.

### 11.1 Remove the language(s) in the first Claude commit

The authors remove every developer-language pair touched by the first Claude commit from **all months**, eliminating **7,529 developer-language pairs**.

At adoption:

- language-count ATT falls from **2.53 to 1.58**;
- newly-used-language ATT falls from **1.19 to 0.81**;
- entropy ATT falls from **0.38 to 0.25**.

So the treatment-defining language explains part, but not most, of the observed expansion.

### 11.2 Remove all Claude-co-authored commits from outcomes

Rebuilding the portfolio using only commits **without** the Claude co-author trailer gives at adoption:

- programming languages: **+1.66**;
- entropy: **+0.27**;
- newly-used languages: **+0.72**;
- cumulative languages: **+1.13**.

This rejects the narrow interpretation that the portfolio expansion is only a bookkeeping consequence of counting agent-authored code. The paper still cautions that a commit without the trailer is not guaranteed to be AI-free.

### 11.3 Activity volume

Conditioning the doubly robust estimator on pre-adoption commit and repository volume leaves the language-count estimate unchanged to the second decimal.

Per-commit outcomes show a more nuanced pattern:

- languages per commit rise by **0.0546** at adoption;
- the per-commit effect disappears by $t=1$ and turns slightly negative later.

The paper therefore interprets the persistent language-count effect as operating mainly through a **larger volume of work spanning more languages**, rather than every individual commit remaining more polyglot.

### 11.4 Stricter activity filters and timing checks

The adoption-month language-count ATT remains large when restricting to developers with denser pre-period activity:

- active in at least 50% of pre-treatment months: **2.09**;
- active in at least six pre-treatment months: **2.18**.

Changing the anticipation window or re-dating adoption to earlier recovered Claude commits yields similar estimates.

### 11.5 Placebo adoption dates

Assigning fake adoption dates 12 months before true adoption produces effects statistically indistinguishable from zero across the main outcomes. This supports the event-time specificity of the observed shift.

---

## 12. Specialist heterogeneity

The paper double-sorts developers by:

1. pre-adoption commit volume as an **ability proxy**, and
2. pre-adoption language breadth within each volume half as a **headroom/specialist proxy**.

The most model-relevant outcome is **newly-used languages**.

At adoption:

| Group | Newly-used language ATT |
|---|---:|
| High-activity specialist | **0.981** |
| High-activity generalist | **0.301** |
| Low-activity specialist | **2.388** |
| Low-activity generalist | **1.015** |

The specialist-generalist gap appears within both activity halves, so the **headroom prediction** is strongly supported: developers with narrower prior portfolios have more unfamiliar-language candidates that can enter the activation band.

However, the paper does **not** claim a clean empirical confirmation of the theoretical ability margin. Pre-adoption commit volume is an imperfect ability proxy and is mechanically related to how many languages have had a chance to appear in the observed history.

---

## 13. Identification: what the design does and does not establish

### What the staggered design helps with

The Callaway–Sant'Anna design addresses:

- treatment-effect heterogeneity across adoption cohorts,
- inappropriate negative weighting from already-treated comparison units,
- calendar-time shocks common to treated and comparison developers.

### Central unresolved threat: project-driven selection

The main concern is reverse causality / time-varying selection:

1. a developer decides to begin an unfamiliar-language project;
2. because the language is unfamiliar, she installs Claude Code;
3. the first Claude commit and the first use of the new language occur at nearly the same time.

Under this story, adoption and portfolio expansion are both consequences of the same project shock.

No outcome-side robustness check can fully eliminate that threat because the problem is **why adoption occurs when it does**, not simply how the outcomes are measured.

Accordingly, the paper's preferred interpretation is:

> a sharp, robust, dynamic **event-time association** around first detectable Claude Code use, quantitatively consistent with the agentic-delegation mechanism.

A stronger causal design would require exogenous variation such as regional free-tier rollouts, pricing changes, or institutional eligibility cutoffs.

---

## 14. What the paper contributes

The paper links four literatures:

1. **Automation vs. augmentation:** it adds a horizontal, within-worker extensive margin — which technological domains a worker can produce in.
2. **Human capital and technology adoption:** language-specific skill creates entry barriers; delegation can lower those barriers without immediately replacing general human judgment.
3. **AI and knowledge work:** it shifts attention from the speed of a fixed task to the **breadth of the feasible production portfolio**.
4. **Open-source/user innovation:** expanded language portfolios can change which developers can contribute to which projects.

The paper's core conceptual contribution is that agentic AI can change not only **how productively** a worker operates inside an existing boundary, but potentially **where that boundary lies**.

---

## 15. Interpretation in one sentence

**If the event-time relationships are causal, agentic AI acts not merely as a productivity multiplier within a developer's existing language portfolio, but as a delegation technology that lowers entry thresholds into previously unfamiliar technical domains.**

---

## 16. Caveats to keep in view

- **Production frontier ≠ skill frontier.** Shipping code in a language with an agent is not evidence of independent mastery.
- **Association ≠ causation.** Voluntary adoption may be triggered by the same project shocks that expand language use.
- **“Unfamiliar” is observational.** A language is unfamiliar if it does not appear in the developer's observed public GitHub history; private or pre-2024 experience may be missed.
- **First detectable use ≠ true first use.** Public co-authored commits can lag experimentation or private usage.
- **Competing-agent detection is incomplete.** Tools that leave no commit signature may be invisible.
- **The ability proxy is weak.** Pre-adoption commit volume does not cleanly identify the model's general specification/verification ability.
- **Long-horizon support thins out.** Fewer cohorts contribute at later event times, widening uncertainty.

---

## 17. Future research suggested by the paper

The paper highlights several next steps:

- identify exogenous variation in agent adoption for stronger causal inference;
- build better measures of developer ability, such as account age, accepted pull requests, or organizational affiliation;
- study the order in which developers enter unfamiliar languages, potentially using a language-similarity graph;
- separate persistent **delegation** from genuine **skill accumulation** over longer horizons;
- extend the framework from programming languages to other technical or industry domains.

---

## Citation

```bibtex
@article{quispexu2026agentic,
  title   = {Agentic Delegation and the Language Frontier of Software Developers: A Model and Evidence from Claude Code on GitHub},
  author  = {Quispe, Alexander and Xu, Kevin},
  year    = {2026},
  month   = {July},
  note    = {arXiv:2605.25438v2}
}
```

---

## Suggested reading order

For readers who want the shortest path through the paper:

1. **Introduction** — motivation, claim, and empirical headline.
2. **Section 4.2** — thresholds and the activation band (the theoretical core).
3. **Sections 5–6** — treatment construction, outcomes, and staggered-adoption estimator.
4. **Section 7** — main event-study results.
5. **Section 8** — robustness to mechanical exposure and activity volume.
6. **Section 9** — specialist/headroom heterogeneity.
7. **Section 10** — identification threats and why the paper remains associational.
8. **Appendix A** — proofs and extensions.
