# Causal Logic and DAGs: Diagnosing Bad Controls

## Objective
I used simulated and real data to show when adding a control variable biases a causal estimate, and I checked control sets against a stated DAG.

## Methodology
- Ran a regression with and without a control caused by both the treatment and the outcome (a collider), and compared the estimate and R-squared when the true effect was zero.
- Tested a control measured before treatment that is caused by two unobserved variables (M-bias).
- Compared a regression of earnings on training alone with one that also controls for a mediator (skill), to separate the total effect from the direct effect.
- Compared the LaLonde experimental benchmark with an observational estimate built from CPS comparison men, before and after covariate adjustment.
- Used a `dag_utils.py` module to find valid adjustment sets from a DAG.
- Had an AI propose controls for a job-training study, checked its set against the DAG with `dag_utils`, revised my prompt once, and checked the new set.
- Extension: simulated an unobserved confounder of varying strength and found the strength at which the estimate doubles.

## Key Findings
- Adding a collider as a control moved the estimate from -0.012 to -0.593 and raised R-squared from 0.000 to 0.585, when the true effect was zero. A better fit and a smaller p-value did not mean a better estimate.
- A control measured before treatment still biased the estimate: -0.201 against a true effect of zero. Timing alone does not make a control safe.
- Regressing on training alone gave 3.978 (truth 4.0), the total effect. Adding the mediator gave 0.988 (truth 1.0), the direct effect. They answer different questions, so the difference is not an error.
- The LaLonde experimental benchmark was $1,794. The observational estimate was -$8,498 before covariate adjustment and $699 after. Moving toward the benchmark is only checkable because the experiment exists.
- The AI's first control set gave 3.622 against a true effect of 4.0, because it included `risk_score`, a collider. After I revised my prompt once to say how `risk_score` is built, the estimate was 4.047.
- In the extension, an unobserved confounder explaining about 33% of the residual variation in the outcome would double my estimate.
