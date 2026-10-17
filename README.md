# Econometrics Foundations

**Study 01 — Ordinary Least Squares (OLS) and Omitted Variable Bias (OVB)**  
**Estudo 01 — Mínimos Quadrados Ordinários (MQO) e Viés por Variável Omitida**

An open and reproducible learning repository in applied econometrics and causal inference, developed as part of the *Cifras & Contextos Quant Lab* research agenda.

> **Research question / Pergunta:** What happens to the estimated association between education and log wages when an economically relevant, correlated determinant of wages (ability) is omitted from the regression?

## 1. Economic model / Modelo econômico

We generate **synthetic data** from a known data-generating process:

$$
\log(wage_i)=\beta_0+\beta_1 education_i+\beta_2 ability_i+u_i.
$$

The baseline parameters are $\beta_0=1$, $\beta_1=0.08$ and $\beta_2=0.12$. Both education and ability are standardized *synthetic indices*; the outcome is a constructed log-wage measure, **not observed earnings**. Consequently, coefficients cannot be interpreted as real-world returns to an additional year of schooling.

The simulation uses $education\sim N(0,1)$, $z\sim N(0,1)$, $ability=\rho\,education+\sqrt{1-\rho^2}\,z$ and $u\sim N(0,0.5^2)$, with mutually independent draws of education, z and u. Baseline $\rho=0.6$.

## 2. Identification / Identificação

The correctly specified regression includes education and ability. The restricted regression omits ability. Its population slope is:

$$
\tilde\beta_1 = \beta_1 + \beta_2\frac{\operatorname{Cov}(education,ability)}{\operatorname{Var}(education)}.
$$

Under the simulation's unit variance and correlation $\rho=0.6$:

$$
\tilde\beta_1=0.08+0.12(0.6)=\boxed{0.152}.
$$

The **theoretical omitted-variable bias is +0.072**. This is a property of the specified synthetic DGP, not an estimate from Brazilian labor-market data. The full-model OLS estimator is unbiased *under the simulation assumptions*; this does not prove identification for observational studies.

**Português:** quando habilidade influencia salários e está positivamente correlacionada à escolaridade, omiti-la transfere parte de sua associação com salários para o coeficiente de educação. O sinal do viés depende tanto do efeito da variável omitida quanto da correlação com a regressora incluída.

## 3. Reproduce / Reprodução

Requirements: Python 3.10+.

```bash
python -m venv .venv
# Linux/macOS: source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m src.study
pytest -q
```

The script exports a synthetic sample, estimates from 500 Monte Carlo replications and summary tables under `results/`, and a chart under `figures/`. Random seeds are fixed for reproducibility. The `notebooks/01_ols_ovb.ipynb` notebook provides an explanatory, interactive walkthrough.

## 4. Outputs / Resultados

![Simulated sampling distributions](figures/sampling_distributions.png)

- `results/summary.csv`: baseline OLS and Monte Carlo summary.
- `results/monte_carlo_estimates.csv`: replication-level coefficients.
- `results/synthetic_sample.csv`: synthetic observations.
- `figures/sampling_distributions.png`: comparison of sampling distributions.

Monte Carlo means fluctuate around the target population values; the published CSV files contain the actual seeded simulation estimates.

## 5. Scientific limits / Limitações

- Simulation, not a causal evaluation with empirical data.
- Ability is observed in the *simulated complete model*, but may be unobserved in real datasets.
- Conditional mean-zero is imposed by design; observational datasets need substantive identification arguments.
- Normality and linear functional form are modeling choices, not general economic facts.
- This demonstration does not identify a real-world causal return to schooling.

## 6. Roadmap / Próximos passos

- Compare different correlations and omitted-variable coefficients.
- Introduce heteroskedastic errors and robust standard errors.
- Demonstrate Frisch–Waugh–Lovell residualization.
- Develop an application to properly sourced, documented public data.

## 7. References / Referências

- Wooldridge, J. M. *Introductory Econometrics: A Modern Approach* (multiple regression and OLS assumptions).
- Angrist, J. D. & Pischke, J.-S. *Mastering 'Metrics* (causal identification and regression).

**Project:** Cifras & Contextos Quant Lab  

**Author:** Jaques Pires Duarte Junior 

**License:** MIT
