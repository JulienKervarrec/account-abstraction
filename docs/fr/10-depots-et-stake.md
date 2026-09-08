# 10. Dépôts et stake

`contracts/core/StakeManager.sol` gère l'argent immobilisé dans l'EntryPoint. La structure `DepositInfo` distingue deux notions qu'il ne faut pas confondre.

Le **dépôt** (`deposit`) est le solde qui paie réellement les frais. `depositTo(account)` l'alimente, `withdrawTo` le retire, sans délai.

Le **stake** (`stake`, `unstakeDelaySec`, `withdrawTime`) ne paie rien. C'est une caution destinée aux entités que le bundler doit pouvoir sanctionner : paymasters, fabriques, agrégateurs. `addStake(unstakeDelaySec)` la bloque pour la durée annoncée. Pour la récupérer, il faut d'abord appeler `unlockStake`, qui fixe `withdrawTime` à l'échéance, puis `withdrawStake` une fois ce délai écoulé.

L'intérêt de ce délai est hors chaîne : il donne au réseau de bundlers le temps de constater qu'une entité se comporte mal et de la mettre à l'écart avant qu'elle ne puisse retirer sa caution et recommencer sous une autre adresse.

`unlockStake` rend le stake inutilisable immédiatement, avant même l'expiration du délai.

Suite : [les paymasters](11-paymasters.md).
