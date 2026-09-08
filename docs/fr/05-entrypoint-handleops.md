# 5. L'EntryPoint et handleOps

`handleOps(ops, beneficiary)` est le point d'entrée du bundler. Le traitement se fait en **deux phases séparées**, et cet ordre est le point le plus important du protocole.

La première phase, `_iterateValidationPhase`, parcourt toutes les opérations du lot et les valide une par une, sans en exécuter aucune. La seconde phase seulement, après l'événement `BeforeExecution`, appelle `_executeUserOp` pour chaque opération.

Cette séparation garantit que l'exécution d'une opération ne peut pas invalider la validation d'une autre du même lot. Sans elle, un compte pourrait faire échouer les opérations voisines après que le bundler a déjà engagé son gaz.

Les frais collectés s'accumulent dans `collected`, puis `_compensate` verse le total au `beneficiary` désigné par le bundler.

Le modificateur `nonReentrant` exige `tx.origin == msg.sender` et un appelant sans code : `handleOps` doit être appelé directement par un compte externe, pas depuis un contrat.

Suite : [la validation côté compte](06-validation-compte.md).
