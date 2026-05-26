# Mobile Game Experimentation: Optimizing Progression Friction in Cookie Cats

## Executive Summary

I analyzed a large-scale A/B test (~90K users) to evaluate how progression friction (game "gates") impacts player retention and engagement in a mobile game.

**Key result:**
- Moving the first gate from Level 30 → Level 40 **decreases retention**
- The effect is statistically significant for **7-day retention**, a critical long-term engagement metric

> **Recommendation:** Keep the gate at Level 30 to maximize long-term player retention and downstream monetization potential.

---

## 1. Business Context & Problem Framing

In free-to-play games like Cookie Cats, progression gates serve two key purposes:
- **Monetization lever** (in-app purchases to skip wait time)
- **Engagement control** (pacing player progression to avoid burnout)

However, gates introduce **friction**, which can negatively impact retention if poorly timed.

**Core Business Question**
> *Where should the first gate be placed to optimize player retention and engagement?*

| Group | Gate Position |
|-------|--------------|
| Control | Level 30 |
| Treatment | Level 40 |

![Cookie Cats](https://amytakeuchi.github.io/images/Cookiecat_img.png)

---

## 2. Experimental Design

- **Sample size:** 90,189 players (randomized)
- **Unit of analysis:** Player-level
- **Primary metrics:**
  - 1-day retention (short-term engagement)
  - 7-day retention (long-term engagement)
- **Secondary metric:** Total game rounds played

This is a classic product tradeoff experiment: *Reduce friction (later gate) vs. maintain structure (earlier gate)*

### Dataset Variables

The data was collected from 90,189 players who installed the game while the AB test was running.

| Variable | Description |
|----------|-------------|
| `userid` | Unique identifier for each player |
| `version` | Group assignment: `gate_30` (control) or `gate_40` (treatment) |
| `sum_gamerounds` | Number of rounds played in the first week after install |
| `retention_1` | Whether the player returned after 1 day |
| `retention_7` | Whether the player returned after 7 days |

---

## 3. Hypothesis & Statistical Approach

- **H₀ (Null):** No difference in retention between Level 30 and Level 40
- **H₁ (Alternative):** Retention differs between the two groups

Given the large sample size, I used a **two-proportion z-test** to evaluate differences in retention rates.

![Hypothesis](https://amytakeuchi.github.io/images/Cookiecat_Hypothesis.png)

- **Significance level (α):** 0.05 (95% confidence interval)
- No sampling needed — full dataset of 90,189 rows used

### Z-Statistic Formula

![Z-Test](https://amytakeuchi.github.io/images/ztest.png)

The Z-statistic quantifies how far a data point is from the mean in standard deviations. A smaller p-value indicates stronger evidence against the null hypothesis.

---

## 4. Summary Statistics & Visualization

![Visualization](https://amytakeuchi.github.io/images/Cookiecat_viz.png)

![Summary Stats](https://amytakeuchi.github.io/images/Cookiecat_sumstats.png)

| Metric | Gate 30 (Control) | Gate 40 (Treatment) |
|--------|:-----------------:|:-------------------:|
| 1-day retention | 44.8% | 44.2% |
| 7-day retention | 19.0% | 18.2% |

**Key observations:**
- No missing values in the dataset
- Some outliers present in `sum_gamerounds`
- **3,994 players never played after installing** — a notable onboarding gap
- Fewer players retained as game rounds increased (typical funnel decay)

---

## 5. Hypothesis Testing & Key Findings

![Results](https://amytakeuchi.github.io/images/Cookiecat_results.png)

![AB Test Result](https://amytakeuchi.github.io/images/abtest_result.png)

| Metric | Significance | Direction |
|--------|-------------|-----------|
| 1-day retention | ❌ Not significant (α = 0.05) | Directionally negative |
| 7-day retention | ✅ Statistically significant | Negative — gate delay harms retention |

---

## 6. Interpretation & Business Recommendations

**Business question:** Does moving the first gate from Level 30 → Level 40 affect player retention and game rounds?

At first glance, delaying the gate seems beneficial: *"Let players enjoy more gameplay before friction."* However, the data suggests the opposite.

### Key Insight
> **Early gating (Level 30) improves long-term retention.**

**Why this likely happens:**
- Introduces structured pacing early
- Reinforces habit formation loops
- Prevents content exhaustion / burnout
- Creates intentional breaks, increasing return probability

> *Not all friction is bad — well-timed friction can improve retention.*

### Business Recommendation

✅ **Ship Decision: Keep the gate at Level 30**

| Expected Impact | |
|---|---|
| 7-day retention | Higher (statistically validated) |
| Player Lifetime Value (LTV) | Stronger player lifecycle |
| Monetization | Improved downstream opportunities |

### Additional Insights
- ~4.4% of users never engaged after install → **onboarding opportunity**
- Engagement drops sharply as rounds increase → typical funnel decay
- Heavy-tailed distribution of game rounds → presence of highly engaged **"power users"**
