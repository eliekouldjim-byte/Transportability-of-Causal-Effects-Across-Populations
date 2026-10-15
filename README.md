
# Transportabilité des effets causaux entre populations

**Une étude par simulation du  covariate shift et de l’estimation de l’effet causal**

##  Question de recherche

Lorsqu’un effet de traitement est estimé dans une population source, peut-on le transporter vers une population cible dont la distribution des covariables est différente ? Ce projet étudie le problème covariate shift : la distribution de la covariable change, tandis que le mécanisme conditionnel du résultat reste stable.

## Pourquoi c'est important

L’effet moyen du traitement (ATE, average treatment effect) peut différer d’une population à l’autre lorsque les effets du traitement varient selon les valeurs de la covariable. Même si le mécanisme causal conditionnel est inchangé, une population ayant un profil d’âge ou de risque différent peut avoir un effet moyen du traitement différent.

## Methodes comparés

1. **Naive source ATE**: différence des moyennes des résultats entre les participants traités et les participants témoins de l’échantillon source. Il estime l’ATE de la population source, pas nécessairement celui de la cible.
2. **Outcome standardization**: on ajuste un modèle linéaire du résultat sur l’échantillon source, incluant le traitement, la covariable et l’interaction traitement–covariable ; on prédit le résultat de chaque participant cible sous traitement et sous contrôle ; on moyenne les contrastes individuels prédits.
3. **Importance-weighted ATE**: on pondère les participants sources par le rapport de densités connu p_target(X) / p_source(X) et on calcule un contraste traités moins témoins pondéré. Le rapport est connu ici parce que les distributions génératrices sont spécifiées dans la simulation. Dans des applications réelles, il devrait généralement être estimé.

## Processus de généralisation des données

Let `X` une covariable de risque/âge standardisée et `T` un traitement binaire randomisé

- Source: `X ~ Normal(0, 1)`
- Target: `X ~ Normal(delta, 1)`
- Treatment: `T ~ Bernoulli(0.5)`
- Outcome: `Y = 1 + 0.5 X + T (2 + 1.5 X) + epsilon`, with `epsilon ~ Normal(0, 1)`

L’effet conditionnel du traitement est `tau(x) = 2 + 1.5 x`.

Par conséquent :
- True source ATE = `2 + 1.5 * E_source[X] = 2`
- True target ATE = `2 + 1.5 * E_target[X] = 2 + 1.5 * delta`

Avec `delta = 0.8`, the true target ATE is `3.2`.

La distribution des covariables change, mais l’équation conditionnelle du résultat est maintenue fixe. Cela fait de l’expérience une illustration claire du décalage de covariables avec hétérogénéité de l’effet du traitement.

