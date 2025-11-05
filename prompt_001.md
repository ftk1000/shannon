2025.11.05


Here’s a tight, working-level digest of Shannon’s Part I, §§6–8.

# Summary

* **§6 Choice, Uncertainty, and Entropy.** Shannon asks for a measure of “choice/uncertainty” for an event with probabilities $p_1,\dots,p_n$ and, from three natural axioms (continuity, monotonicity for the equiprobable case, and additivity for successive choices), derives the *unique* form
  $$
  H(p_1,\dots,p_n)=-K\sum_i p_i\log p_i,
  $$
  i.e., (up to a constant) the familiar entropy. He notes the statistical-mechanics connection and basic identities such as $H(X,Y)=H(X)+H_X(Y)$ and $H(Y)\le H_X(Y)$ with equality only under independence.     

* **§7 Entropy of an Information Source.** For a finite-state (Markov) source with state $i$ emitting symbol $j$ with probability $p_i(j)$ and state probability $P_i$, the source entropy per symbol is
  $$
  H=\sum_i P_i,H_i ;=;-\sum_{i,j} P_i,p_i(j)\log p_i(j),
  $$
  and per second $H'=mH$ (with $m$ symbols/s). He proves the “typical-set” phenomena: almost all long sequences of length $N$ have probability about $2^{-NH}$ (Theorem 3) and the logarithm of the count of reasonably probable sequences grows like $NH$ (Theorem 4). He also gives sequence-based estimators $G_N$ and $F_N$ that decrease to $H$ (Theorems 5–6). Finally, he discusses *redundancy* (e.g., English ≈ 50%), relating it to tasks like crossword feasibility.      

* **§8 Encoding/Decoding as Transducers.** Shannon models encoders/decoders as finite-state transducers with update/output rules
  $y_n=f(x_n,\phi_n)$, $\ \phi_{n+1}=g(x_n,\phi_n)$.
  **Theorem 7:** Driving a finite-state transducer by a finite-state source yields another finite-state source whose entropy rate cannot increase; it is preserved iff the transducer is non-singular (invertible). **Theorem 8:** For a constrained channel (finite-state graph with symbol durations), there is a choice of transition probabilities that *maximizes* output entropy and achieves the channel capacity $C=\log W$.   

# Key results (with formulas)

1. **Uniqueness of entropy form (§6, Thm. 2):**
   $H=-K\sum_i p_i\log p_i$ is the only function meeting the three axioms; choose base 2 to measure bits. 

2. **Chain rule & data-processing flavor (§6):**
   $H(X,Y)=H(X)+H_X(Y)$ and $H(Y)\le H_X(Y)$ (knowing $X$ can’t increase uncertainty about $Y$).  

3. **Source entropy (§7):**
   $H=-!\sum_{i,j} P_i,p_i(j)\log p_i(j)$ (per symbol); $H'=mH$ (per second). 

4. **Typical-set laws (§7, Thm. 3 & 4):**
   For large $N$, almost all sequences satisfy $\frac1N\log(1/p)\approx H$; and $\log n(q)\sim NH$. 

5. **Finite-state transducers (§8, Thm. 7):**
   Entropy rate cannot increase through a finite-state (deterministic) encoder; equality iff invertible. 

6. **Capacity-achieving assignment under constraints (§8, Thm. 8):**
   For a constraint graph with symbol durations $\ell^{(s)}*{ij}$, choosing
   $$
   p^{(s)}*{ij}=\frac{B_j}{B_i},W^{-\ell^{(s)}*{ij}},\qquad B_i=\sum*{s,j}B_j,W^{-\ell^{(s)}_{ij}},
   $$
   attains $H=C=\log W$. 

# Intuition (why these results make sense)

* **Why the $-\sum p\log p$ form?** The axioms force a measure that (i) behaves smoothly as probabilities change, (ii) grows with the number of equally likely choices, and (iii) composes additively when a decision is made in stages. Logs are the only way to make “multiplying possibilities” translate into “adding information.” 

* **Chain rule/data processing view.** Learning $X$ can only shrink the plausible set for $Y$, never enlarge it—so conditional entropy drops. The joint uncertainty is just “first pick $X$, then whatever $Y$ remains ambiguous after seeing $X$.” 

* **Typical sets.** Long outputs of a stationary finite-memory source spend (almost) all probability mass on a “thin shell” of sequences with log-probability $\approx -NH$. So “information per symbol” is exactly the asymptotic description length per symbol for almost every sequence—hence why $H$ is *the* rate. 

* **Transducers don’t create information.** A finite-state mapping can permute, relabel, or compress structure, but unless it’s invertible it inevitably merges distinguishable inputs, losing entropy; if it is invertible, uncertainty is preserved. 

* **Maximizing entropy under constraints hits capacity.** If some sequences are forbidden or have costs (durations), the most “random” allowed process is the one that equalizes weighted path counts at the exponential growth rate $W$; picking transitions $\propto W^{-\text{length}}$ makes every allowed long string roughly equiprobable per unit time, pushing the entropy rate up to $C=\log W$. 

# A few applications you can run with

1. **Universal lossless coding.** The typical-set picture underlies practical compressors: estimate $p$, assign short codewords to frequent outcomes (Huffman/arith. coding), and approach $H$ bits/symbol; the finite-delay gap is controlled by $G_N,F_N\downarrow H$. 

2. **Language modeling & redundancy.** Shannon’s English-redundancy estimate (~50%) explains why text is highly compressible and why humans can restore deleted letters; it also motivates cryptanalysis difficulty/success. 

3. **Constrained coding (magnetic/flash/optical).** Real media forbid patterns (e.g., run-length limits). Theorem 8 gives the entropy-maximizing input for such constraints; practical run-length-limited (RLL) codes emulate these transition probabilities to operate near the combinatorial capacity. 

4. **Codec design as finite-state transduction.** Treat encoders/decoders as state machines; non-singularity preserves information (reversible transforms), while deliberate non-invertible mappings reduce entropy (compression) before a later error-control stage.  

5. **Model checking for sources.** The $H'=mH$ identity links symbol-rate and entropy-rate; e.g., for telemetry/log streams you can compare measured throughput to estimated $H$ to detect anomalies (unexpected structure) or to size channels. 

If you want, I can add one-page cheat sheets with the exact statements (Theorems 2–8) and a tiny Python demo that empirically verifies the typical-set law for a Markov source.
