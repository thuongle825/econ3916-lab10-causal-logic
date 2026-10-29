# econ3916-lab10-causal-logic
# Correlation & Causality: Confounders, Colliders and the Limits of Controls

## Objective
I tested whether a correlation can tell causation, confounding and selection apart, and when adding a control to a regression removes bias or creates it.

## Methodology
- I simulated three worlds from known data-generating processes: one where D causes Y with an effect of 2.0, one where a confounder U drives both D and Y with no effect of D, and one where D and Y are independent but only units with D − Y > 1.2 are kept. I compared their correlations and plotted them on shared axes.
- I drew a confounder, a collider and a mediator as directed graphs with networkx, and read the paths between D and Y off each one.
- I simulated a confounder and compared a regression of Y on D with one that also controlled for U.
- I simulated two independent variables, D and Y, that both cause a collider C. I compared a regression with no controls to one that controlled for C, then looked at the correlation of D and Y inside a narrow band of C.
- I built an M-bias structure, D ← u1 → W ← u2 → Y, where W is measured before treatment, and compared regressions with and without W.
- I wrote a function, `coef_on_d()`, that regresses Y on D plus any list of controls and returns the coefficient on D. I used it to control for both W and u1.
- I reproduced the LaLonde (1986) contrast on the Dehejia and Wahba subsample. I compared the NSW experimental estimate with a naive comparison against CPS respondents and a regression with eight pre-treatment covariates, and measured the share of the gap the covariates closed.

## Key Findings
- The three worlds gave correlations of 0.654, 0.664 and 0.632, although the true effects were 2.0, 0.0 and 0.0. The correlation alone could not tell them apart, and neither could the plots.
- Controlling for the confounder moved the estimate from 2.476 to 1.500, the true effect.
- Controlling for the collider gave −0.592, with a p-value below 0.0005, where the true effect is 0. The p-value showed the estimate was not chance, but could not show it came from the wrong control.
- In the M-bias structure, controlling for W gave −0.191 where the truth is 0, even though W was measured before treatment. Also controlling for u1 brought it back to 0.006. "Control for anything measured before treatment" is not a safe rule; whether a control helps depends on where it sits in the graph.
- In LaLonde, the experiment gave $1,794. The naive CPS comparison gave −$8,498, and adding the covariates gave $699, closing 89% of the gap. Getting closer to the benchmark does not show the adjustment is valid, and without the experiment there would be nothing to check against.
