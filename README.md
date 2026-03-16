# The Attention Probability Conjecture: Polymarket Prices as Q-Measure Objects

## Abstract

We conjecture that Polymarket contract prices are not well-calibrated estimates of real-world event probabilities, but rather **Q-measure (risk-neutral) probabilities** — equilibrium prices shaped by capital allocation, participant attention, and liquidity constraints. We call these **"attention probabilities."** Borrowing the P/Q distinction from mathematical finance, we propose a logit-space decomposition of Polymarket prices into a physical probability component and three distortion terms: risk bias, liquidity noise, and an attention premium. We further conjecture that **network-implied pricing** — computing what a market's price "should be" given its correlated neighbors — can distinguish information-driven (P) from attention-driven (Q) price movements. These conjectures are motivated by structural observations about Polymarket's participant base, market microstructure, and cross-market correlation patterns, but remain formally unproven and empirically untested.

## 1. Introduction

Polymarket, the largest decentralized prediction market by volume, processed over $9 billion in trading volume during the 2024 U.S. presidential election cycle alone (Karamouzis et al., 2026). Its contract prices are routinely cited by media, analysts, and policymakers as probability estimates for future events. The implicit assumption is that a contract trading at 73% means "this event has a 73% probability of occurring."

We believe this assumption is wrong, or at least importantly incomplete.

In quantitative finance, the distinction between the **P-measure** (physical probability) and **Q-measure** (risk-neutral probability) is foundational. The price of a derivative does not reflect the actual likelihood of the underlying event; it reflects a risk-adjusted expectation that incorporates the preferences, constraints, and beliefs of market participants (Harrison & Kreps, 1979). The two measures are related by a change-of-measure density (the pricing kernel), and the gap between them encodes economically meaningful information about risk premia.

We conjecture that the same distinction applies to Polymarket. A contract price is a Q-measure object: it reflects the intersection of genuine information about the event with the capital allocation decisions, attention patterns, and liquidity conditions of Polymarket's specific participant population. We propose calling these prices **attention probabilities**, because the dominant non-informational distortion on Polymarket appears to be attention — where crypto-native participants choose to direct their capital and engagement.

### 1.1 Why Polymarket Specifically

Polymarket's participant base is not a representative cross-section of informed forecasters. It is overwhelmingly crypto-native: the same population active on Pump.fun, DexScreener, and crypto Twitter (Karamouzis et al., 2026; Easley et al., 2026). This creates specific biases:

- **Attention clustering**: When a topic goes viral in crypto circles, capital floods into related Polymarket contracts regardless of whether new information has emerged about the underlying event.
- **Whale effects**: A single large trader can move prices in thin markets, creating price signals that reflect portfolio decisions rather than probability assessments.
- **Emotional engagement premium**: Contracts on emotionally salient topics (elections, culture war issues, celebrity events) attract disproportionate volume relative to their informational complexity (Brown et al., 2025).
- **Structural "Yes" bias**: Participants tend to overtrade the affirmative position (Reichenbach & Walther, 2025), introducing a directional distortion unrelated to event probability.

These are not bugs in an otherwise efficient market. They are structural features of Polymarket's Q-measure — the specific way this market transforms beliefs into prices.

## 2. The Conjecture: Logit-Space Decomposition

### 2.1 Setup

A Polymarket binary contract pays $1 if event $A$ occurs and $0$ otherwise. The observed market price is $p_Q \in (0,1)$.

**Conjecture 1.** The observed Polymarket price $p_Q$ can be decomposed in log-odds space as:

$$\text{logit}(p_Q) = \text{logit}(p_P) + \alpha_{\text{risk}} + \alpha_{\text{liq}} + \alpha_{\text{attn}} + \varepsilon$$

where:
- $p_P$ is the latent physical probability (operationally: the calibration target — the frequency with which events at price $p$ actually occur)
- $\alpha_{\text{risk}}$ is the favorite-longshot bias, a well-documented systematic distortion
- $\alpha_{\text{liq}}$ is a liquidity noise term with $E[\alpha_{\text{liq}}] \approx 0$ but $\text{Var}(\alpha_{\text{liq}}) \propto 1/L$
- $\alpha_{\text{attn}}$ is the attention premium, driven by capital flow independent of new information
- $\varepsilon$ is residual noise

We use log-odds space because the decomposition is additive and naturally respects the $(0,1)$ bounds of probabilities. In odds space, the equivalent multiplicative form is:

$$\frac{p_Q}{1 - p_Q} = \frac{p_P}{1 - p_P} \cdot \lambda_{\text{risk}} \cdot \lambda_{\text{liq}} \cdot \lambda_{\text{attn}}$$

where $\lambda_i = e^{\alpha_i}$.

### 2.2 What Each Term Means on Polymarket

**$\alpha_{\text{risk}}$ (favorite-longshot bias):** Low-probability contracts are systematically overpriced; high-probability contracts are systematically underpriced. This is the most studied prediction market distortion (Snowberg & Wolfers, 2010). On Polymarket specifically, Reichenbach & Walther (2025) find prices closely track realized probabilities overall but with a structural "Yes" overtrade bias. On Kalshi, Brown et al. (2025) document a clear favorite-longshot pattern with wealth transferring systematically from takers to makers.

**$\alpha_{\text{liq}}$ (liquidity noise):** Polymarket uses a CLOB (central limit order book) rather than an automated market maker. In thin order books, a $10,000 trade can move the price several percentage points. This does not systematically bias prices upward or downward, but it inflates the variance of the price as a probability estimator. The noise magnitude is inversely related to order book depth.

**$\alpha_{\text{attn}}$ (attention premium):** This is the conjectured novel component. When a Polymarket topic trends on crypto Twitter, or when a whale enters a position, capital flows into the contract. This capital inflow shifts the price independent of any change in the underlying event probability. The attention premium is:
- Positive when capital flows in (price inflated above $p_P$)
- Negative when attention withdraws (price deflated below $p_P$)
- Time-varying and potentially mean-reverting as attention decays

### 2.3 Why Not Just "Risk Premium"?

One might argue that $\alpha_{\text{attn}}$ is simply a component of the risk premium. We conjecture it is distinct for three reasons:

1. **Directionality**: Risk premia in prediction markets are structurally tied to the favorite-longshot bias and apply across all markets at a given price level. Attention premia are market-specific and topic-specific.
2. **Dynamics**: Risk premia are relatively stable over time for a given market. Attention premia are bursty — they spike when a topic goes viral and decay as attention fades.
3. **Cross-market signature**: Risk premia do not produce inconsistencies across correlated markets. Attention premia do — when one market in a causal cluster receives attention and its neighbors do not, prices become inconsistent with the network structure (Section 3).

## 3. Network-Implied Pricing Conjecture

### 3.1 Motivation

Polymarket hosts hundreds of thousands of markets, many of which are causally or statistically related. "Will Bitcoin dip to $65,000 in March?" is correlated with "Will Bitcoin reach $75,000 in March?", "Will the Fed cut rates?", and dozens of other crypto and macro contracts.

If all price movements were P-driven (reflecting genuine new information), then correlated markets should move coherently: a real-world shock that changes one probability should propagate through the correlation network to adjust related probabilities.

But if a price movement is Q-driven (attention, whale trade, viral tweet), it will affect only the specific contract that received attention, leaving its correlated neighbors unchanged. This creates a detectable inconsistency.

### 3.2 The Method

**Conjecture 2.** For a Polymarket contract $B$ with validated statistical neighbors $\{A_1, \ldots, A_k\}$, the **network-implied price** is:

$$\hat{p}_B = \text{logit}^{-1}\left(\text{logit}(p_B^{(t-\tau)}) + \hat{\Delta}_B^{\text{logit}}\right)$$

where the implied logit-return is:

$$\hat{\Delta}_B^{\text{logit}} = \frac{\sum_i w_i \cdot r_i \cdot \frac{\hat{\sigma}_B}{\hat{\sigma}_{A_i}} \cdot \Delta^{\text{logit}}_{A_i}}{\sum_i w_i}$$

and:
- $r_i$ is the validated correlation between logit-returns of $A_i$ and $B$
- $\hat{\sigma}_{A_i}, \hat{\sigma}_B$ are estimated standard deviations of logit-returns
- $\Delta^{\text{logit}}_{A_i}$ is the observed logit-return of neighbor $A_i$ over the window $\tau$
- $w_i$ are confidence weights (reflecting sample size, statistical significance, and validation status)

The **network deviation** $\delta_B = \text{logit}(p_B) - \text{logit}(\hat{p}_B)$ measures how inconsistent $B$'s price is with its neighborhood.

**Conjecture 3.** Large $|\delta_B|$ values are predictive: markets with large positive deviations (actual price higher than network-implied) should, on average, experience subsequent price declines as the attention premium decays. Conversely, markets with large negative deviations should experience subsequent price increases.

This conjecture is empirically testable but has not been tested.

### 3.3 Heuristic P vs Q Classification

We propose the following heuristics for classifying Polymarket price movements, with the caveat that these are conjectural rules of thumb, not identified causal claims:

| Signal Pattern | Conjectured Dominant Measure | Reasoning |
|---|---|---|
| Correlated cluster moves coherently | P (information) | Real-world event propagated through network |
| Isolated spike, neighbors static | Q (attention) | Market-specific capital flow |
| High volume + price change | P-leaning | Capital committed on information |
| Price change + low volume | Q (noise) | Thin book, mechanical impact |
| News-corroborated movement | P | External evidence of real-world change |
| No news, price moved | Q | Attention or speculative flow |

Each of these heuristics has failure modes. "No news, price moved" could reflect private information. "Correlated cluster moves together" could reflect attention contagion rather than information propagation. These heuristics are starting points for empirical investigation, not conclusions.

## 4. Implications (If the Conjectures Hold)

### 4.1 For Polymarket Users

If Polymarket prices are Q-measure objects, then a price of 73% should not be read as "73% chance." It should be read as: "given where Polymarket's crypto-native participant base is allocating capital and attention right now, this contract clears at 73%." The distinction matters most for:
- Low-liquidity markets where $\alpha_{\text{liq}}$ is large
- Markets experiencing attention spikes (viral topics, whale activity)
- Markets where Polymarket's participant base has known biases (crypto-related events, U.S. politics)

### 4.2 For Forecasting Systems

Systems that use Polymarket data as forecasting inputs should:
1. Report liquidity-adjusted confidence alongside price
2. Use network consistency checks to flag Q-dominated signals
3. Calibrate using resolved markets to estimate $\alpha_{\text{risk}}$
4. Ideally, report both $p_Q$ (market price) and an estimated $\hat{p}_P$ (bias-corrected)

### 4.3 For Market Designers

If attention premia are substantial, Polymarket could reduce the Q-P gap by:
- Deeper automated market maker reserves for important but low-attention markets
- Publishing a "network consistency score" alongside each market price
- Flagging markets where price movements are inconsistent with correlated contracts

## 5. What Would Disprove These Conjectures

**Conjecture 1** would be weakened if Polymarket prices are found to be perfectly calibrated across all liquidity levels, attention regimes, and market types — i.e., if the calibration curve is flat and the decomposition terms are all negligibly small.

**Conjecture 2** would fail if network deviations are purely random noise — uncorrelated with any observable market characteristic and unpredictive of future price movements.

**Conjecture 3** would be disproved if large network deviations do NOT predict subsequent price reversals — i.e., if "overpriced relative to network" markets are just as likely to continue rising as to fall.

All three conjectures are testable with Polymarket's publicly available on-chain data and price history.

## 6. Conclusion

We conjecture that Polymarket prices are attention probabilities: Q-measure objects that encode the beliefs, risk preferences, liquidity conditions, and attention allocation of a specific crypto-native participant base. The gap between what Polymarket says (Q) and what is actually likely to happen (P) is not a failure of the market — it is a structural feature of how any market transforms heterogeneous beliefs into a single price.

The network-implied pricing conjecture offers a potential method to detect this gap by checking whether a market's price is consistent with its correlated neighborhood. If empirically validated, this would provide a principled way to distinguish signal from noise in prediction market data.

These conjectures are offered in the spirit of making a useful distinction precise. Whether the attention premium is large enough to matter, whether network deviations predict reversals, and whether the logit decomposition is the right functional form — these are open empirical questions.

Polymarket is not an oracle. It is a map of where a specific community puts its money. The map is useful, but it is not the territory.

## References

Arrow, K. J., Forsythe, R., Gorham, M., et al. (2008). The promise of prediction markets. *Science*, 320(5878), 877-878.

Brown, Z. Y., Byun, H., Pollock, A., & Snowberg, E. (2025). Makers and takers: The economics of the Kalshi prediction market. *UCD Centre for Economic Research Working Paper Series*, WP2025/19.

Easley, D., O'Hara, M., & Yang, L. (2026). Prediction laundering: The illusion of neutrality, transparency, and governance in Polymarket. *arXiv preprint* arXiv:2602.05181.

Harrison, J. M., & Kreps, D. M. (1979). Martingales and arbitrage in multiperiod securities markets. *Journal of Economic Theory*, 20(3), 381-408.

Karamouzis, S., et al. (2026). The anatomy of Polymarket: Evidence from the 2024 presidential election. *arXiv preprint* arXiv:2603.03136.

Manski, C. F. (2006). Interpreting the predictions of prediction markets. *Economics Letters*, 91(3), 425-429.

Reichenbach, F., & Walther, M. (2025). Exploring decentralized prediction markets: Accuracy, skill, and bias on Polymarket. *SSRN Working Paper* 5910522.

Snowberg, E., & Wolfers, J. (2010). Explaining the favorite-longshot bias: Is it risk-love or misperceptions? *Journal of Political Economy*, 118(4), 723-746.

Wolfers, J., & Zitzewitz, E. (2006). Interpreting prediction market prices as probabilities. *NBER Working Paper* 12200.
