Alright — I’ll give you a **serious, research-grade `README.md`** that matches the depth of your theory + Silent Alpha Death framework.


# 1. Overview

This repository implements the **Silent Alpha Death framework**, a structural diagnostic system for detecting strategy degradation through intrinsic-time collapse rather than outcome-based performance metrics.

Traditional alpha decay is typically framed as:

- signal deterioration
- parameter drift
- regime switching
- volatility shifts

This project proposes a fundamentally different interpretation:

> Strategy failure occurs when **informational time stops progressing**, even while observed time continues.

The framework treats time as an endogenous state variable governed by finite information flow.

The diagnostic model implemented here is the applied counterpart of the intrinsic-time representation developed in:

**Finite Information Flow and Time–Change Representations in Non-Markovian Stochastic Dynamics**. :contentReference[oaicite:0]{index=0}

---

# 2. Mathematical Foundation

## 2.1 Observed vs Intrinsic Time

Let observed strategy outputs be:

\[
X_t
\]

Instead of modeling \(X_t\) directly, we assume:

\[
X_t = Z_{\tau(t)}
\]

where:

- \(Z_s\) is an intrinsic ergodic process
- \(\tau(t)\) is an endogenous clock

This representation arises from the **time-change rigidity theorem** under finite causal information capacity. :contentReference[oaicite:1]{index=1}

---

## 2.2 Endogenous Clock Construction

Intrinsic time is defined by:

\[
\tau(t) = \int_0^t \psi(Y_s)\,ds
\]

where:

- \(Y_t\) = latent structural regime
- \(\psi(Y_t)\ge0\) = clock speed / information efficiency

Interpretation:

| Quantity | Meaning |
|---|---|
| \(t\) | observed calendar time |
| \(\tau(t)\) | effective informational time |
| \(\psi(Y_t)\) | information flow rate |

When crowding, liquidity pressure, or capacity constraints increase, \(\psi(Y_t)\) decreases.

---

## 2.3 Aging Ratio (Core Diagnostic)

Define the **aging ratio**:

\[
A(t)=\frac{\tau(t)}{t}
\]

Interpretation:

- \(A(t)\approx1\): healthy strategy
- \(A(t)\downarrow\): aging regime
- \(A(t)\to0\): silent alpha death

This quantity measures whether observed time corresponds to genuine information accumulation.

---

# 3. Structural Theory Behind the Model

## 3.1 Finite Information Flow Principle

From the theory paper:

If a stochastic process has:

1. finite causal memory
2. bounded mutual information growth

then any persistent non-Markovian dependence reduces to an additive functional:

\[
A(t)=\int_0^t \psi(U_s)\,ds
\]

which induces a time-change representation. :contentReference[oaicite:2]{index=2}

This is the theoretical justification for intrinsic time.

---

## 3.2 Information-Theoretic Interpretation

Let \( \mathcal{F}_t \) denote the filtration of the process.

Finite information capacity implies:

\[
I(U_{t+h};\mathcal{F}_t) \le C h
\]

meaning new statistical dependence grows at most linearly.

Persistent innovation-driven memory is excluded.

Consequently:

> Non-Markovian structure cannot accumulate indefinitely — it compresses into a clock.

This is the key structural insight behind Silent Alpha Death.

---

# 4. Silent Alpha Death Mechanism

## 4.1 Structural Regimes

The framework identifies three regimes:

### Survival
\[
\tau(t)\sim t
\]

Strategy explores new informational states.

---

### Aging
\[
\tau(t)=o(t)
\]

Information flow slows.
Empirical convergence weakens.

---

### Silent Death
\[
\lim_{t\to\infty}\frac{\tau(t)}{t}=0
\]

Observed activity persists but informational novelty vanishes.

Standard backtests become statistically invalid.

This phenomenon is demonstrated in the accompanying research monograph. :contentReference[oaicite:3]{index=3}

---

## 4.2 Failure of Naive Backtests

Standard metrics assume:

\[
\frac{1}{t}\int_0^t X_s ds \rightarrow \mathbb{E}[X]
\]

But under time change:

\[
\frac{1}{t}\int_0^t Z_{\tau(s)} ds
\]

convergence depends on growth of \(\tau(t)\), not \(t\).

If intrinsic time grows sublinearly:

- effective sample size collapses
- Sharpe ratios become misleading
- retraining fails

---

# 5. Numerical Model Implemented Here

## 5.1 Intrinsic Process

We simulate an Ornstein–Uhlenbeck process:

\[
dZ_s = -\theta Z_s ds + \sigma dW_s
\]

Chosen because:

- stationary
- Gaussian
- analytically tractable

---

## 5.2 Regime Process

Structural regime:

\[
dY_t = -\alpha Y_t dt + \eta dB_t
\]

Clock speed:

\[
\psi(Y_t)=\exp(-\beta Y_t)
\]

This produces smooth endogenous time decay.

---

## 5.3 Observed Process

Observed strategy output:

\[
X_t = Z_{\tau(t)}
\]

Computed via interpolation between intrinsic-time samples.

---

# 6. Repository Structure


src/
intrinsic_time/
clock.py
regime.py
diagnostics.py
stochastic.py

models/
ou_process.py
time_changed_process.py

experiments/
silent_death_simulation.py
naive_backtest.py
intrinsic_diagnostics.py



Design philosophy:

- Theory modules mirror mathematical objects
- Experiments reproduce paper results
- Diagnostics operate independently of PnL



# 7. Key Diagnostics Implemented

## Aging Ratio
\[
A(t)=\tau(t)/t
\]

## Clock Speed
\[
\psi(Y_t)
\]

## Ergodic Failure Metric
\[
\left|\frac{1}{t}\int_0^t X_s ds - \mathbb{E}[Z]\right|
\]

---

# 8. Practical Interpretation

This framework is **not** a trading model.

It is a structural risk diagnostic designed for:

- research governance
- capacity management
- early shutdown signals
- validation beyond Sharpe ratio

It operates upstream of PnL deterioration.



# 9. Theoretical Lineage

Silent Alpha Death is the applied layer of:

- finite-information cocycle rigidity
- intrinsic-time representation
- measurable time-change classification :contentReference[oaicite:4]{index=4}

The repository translates structural probability theory into a production-style diagnostic system.



# 10. Reproducing Core Experiments


python experiments/silent_death_simulation.py
python experiments/naive_backtest.py
python experiments/intrinsic_diagnostics.py



Outputs:

- observed process plots
- rolling Sharpe diagnostics
- clock speed decay
- aging ratio collapse

---

# 11. Research Philosophy

This project assumes:

> Strategy viability is determined by information flow, not performance outcomes.

Treating time as endogenous exposes failure modes invisible to classical backtesting pipelines.


