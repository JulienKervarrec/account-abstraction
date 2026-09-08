# 11. Les paymasters

Un paymaster paie les frais à la place de l'utilisateur. C'est le mécanisme derrière les transactions parrainées et le paiement du gaz en jetons ERC-20.

Il est désigné par `paymasterAndData`, dont la disposition est fixée par l'EntryPoint :

```
paymaster(20) || verificationGasLimit(16) || postOpGasLimit(16) || paymasterData
```

`IPaymaster` définit deux fonctions. `validatePaymasterUserOp` est appelée pendant la phase de validation ; elle retourne un `context` et une `validationData` encodée comme au chapitre 7. Un revert rejette l'opération. Une remarque du code mérite attention : les bundlers refusent cette méthode si elle modifie l'état, sauf pour un paymaster explicitement listé.

`postOp` est appelée après l'exécution, avec le `context` retourné plus tôt, le coût réel et le prix du gaz effectif. Retourner un `context` vide dispense de cet appel. Le mode indique le résultat : `opSucceeded`, ou `opReverted` — et dans ce second cas **le paymaster paie quand même**. La troisième valeur, `postOpReverted`, est purement interne et n'est jamais transmise.

`contracts/core/BasePaymaster.sol` fournit une base qui vérifie l'appelant et expose les fonctions de dépôt et de stake.

Suite : [l'agrégation de signatures](12-agregation-signatures.md).
