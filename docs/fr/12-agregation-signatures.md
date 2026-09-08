# 12. L'agrégation de signatures

Certains schémas de signature, comme BLS, permettent de condenser plusieurs signatures en une seule vérifiable d'un coup. `IAggregator` expose ce mécanisme.

Un compte qui utilise un agrégateur ne retourne pas 0 depuis `validateUserOp` : il retourne l'adresse de cet agrégateur, dans les 160 bits bas de `validationData`.

Le bundler emploie alors `handleAggregatedOps`, qui prend un tableau de groupes `UserOpsPerAggregator`. Pour chaque groupe muni d'un agrégateur, il appelle `validateSignatures(ops, signature)` **avant** la phase de validation individuelle ; un revert se traduit par l'erreur `SignatureValidationFailed`. L'adresse `address(1)` est explicitement refusée : c'est le marqueur d'échec de signature, jamais une adresse de contrat valable.

Les deux autres fonctions de l'interface ne sont pas appelées sur la chaîne pendant `handleOps`. `aggregateSignatures` sert au bundler, hors chaîne, à produire la signature agrégée. `validateUserOpSignature` lui permet de vérifier une opération isolée avant de l'intégrer.

L'événement `SignatureAggregatorChanged` est émis à chaque changement de groupe pendant l'exécution.

Suite : [le support d'EIP-7702](13-eip-7702.md).
