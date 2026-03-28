# 🔬 TurboQuant Technical Review

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Stars](https://img.shields.io/github/stars/MorkMindy74/turbo-quant-review?style=social)](https://github.com/MorkMindy74/turbo-quant-review/stargazers)

> **An independent technical audit of the paper *TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate*.**

---

## 📋 Overview

This repository contains a rigorous, line-by-line mathematical review of the TurboQuant paper.  
The analysis focuses exclusively on whether the **theoretical claims are mathematically supported as written** — not on the practical utility of the method.

**Verdict: Mixed.**

| Aspect | Assessment |
|---|---|
| Core engineering idea | ✅ Plausible and practically useful |
| Information-theoretic lower bound | ❌ Not proved as written |
| Bit budget for TurboQuant_prod | ⚠️ Understated (extra scalar γ not counted) |
| Residual-norm statement | ❌ Mathematical inconsistency |
| MSE theorem strength | ⚠️ Overstated — relies on asymptotic approximations |

---

## 📄 Review Document

👉 [**Read the full technical review →**](turboquant_technical_note_review.md)

---

## 🔍 Key Findings

### 1 · Lower-Bound Argument is Invalid as Written
The paper applies Shannon's differential-entropy lower bound to **x ~ Unif(S^{d-1})**, a distribution that is **singular** with respect to Lebesgue measure on ℝ^d. The substitution is not lawful; the proof technique does not establish the claimed bound.

### 2 · Bit Budget is Understated for TurboQuant_prod
TurboQuant_prod emits a real scalar `γ = ‖r‖₂` alongside the quantized representation. This scalar requires storage (≈ 32 bits for float32), adding **O(1/d) bits per coordinate** overhead that is not accounted for in the stated *b*-bit budget.

### 3 · Residual-Norm Statement is Inconsistent
The paper writes `E[‖r‖] = sqrt(C(f_X, b-1))`, but the paper's own MSE derivation gives `E[‖r‖²] = d · C(f_X, b-1)`, so the correct bound would involve a **factor of sqrt(d)** and Jensen's inequality.

### 4 · MSE Theorem is Overstated
The proof uses a Gaussian approximation for "moderate" dimensions and the asymptotic Panter-Dite formula, neither of which is turned into a rigorous finite-d, all-b bound.

---

## ✅ What Still Looks Sound

- Random rotation makes the post-rotation coordinate law distributionally tractable.
- Lloyd-Max scalar codebook for rotated coordinates is a reasonable design choice.
- QJL residual correction yields an **unbiased** inner-product estimator.
- Practical performance does **not** depend on the lower-bound proof being correct.

> **This review should not be read as "TurboQuant does not work."**  
> It should be read as **"the strongest theoretical claims are not adequately proved."**

---

## 🛠 Suggested Corrections

To make the paper technically convincing, the authors would need to:

1. Replace the lower-bound proof with one that applies to **sphere-supported sources**, or reformulate so the Shannon bound is used lawfully.
2. State the bit budget of TurboQuant_prod **honestly**, including the cost of `γ`, or show how it is encoded within the claimed budget.
3. Correct `E[‖r‖]` and clearly distinguish it from `sqrt(E[‖r‖²])`.
4. Downgrade theorem statements that rely on approximations, or provide rigorous finite-dimensional bounds.

---

## 📚 Context: RaBitQ Dispute

There is a public attribution dispute around the relationship between TurboQuant and prior **RaBitQ** work.  
This review does **not** treat that dispute as primary evidence — all findings are based solely on the paper's own statements and proofs.  
The dispute is noted as relevant context for readers evaluating novelty claims.

---

## 🤝 Contributing

Corrections, counterarguments, and additional analysis are welcome!  
Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a PR or issue.

---

## 📜 License

This review is released under the [MIT License](LICENSE).  
You are free to share, cite, and build upon this work with attribution.

---

## ⭐ Support This Work

If you found this review useful — whether you agree with it or not — please consider **starring this repository**.  
It helps surface independent technical audits and encourages open scientific discourse.

[![Star this repo](https://img.shields.io/github/stars/MorkMindy74/turbo-quant-review?style=for-the-badge&logo=github)](https://github.com/MorkMindy74/turbo-quant-review/stargazers)

---

*This note is based on the paper's own source text and formulas. It is not based on social-media commentary.*
