# 14. Limites de ce parcours

Ce parcours est **documentaire**. Rien n'a été installé, compilé, déployé ni exécuté dans le cadre de sa rédaction. Aucune affirmation de ce texte ne repose sur un test qui aurait été lancé : tout provient de la lecture du code source et des commentaires du dépôt.

La suite de tests existe et se trouve dans `test/`, en TypeScript. Les fichiers `entrypoint.test.ts`, `paymaster-signature.test.ts`, `postop.test.ts` et `entrypoint-7702.test.ts` correspondent directement aux chapitres 5, 11 et 13, et constituent le meilleur point de départ pour vérifier ce qui est décrit ici.

Le dépôt contient des rapports d'audit dans `audits/`. Lire ce parcours ne remplace ni ces audits ni une revue de sécurité : comprendre un mécanisme n'est pas l'avoir validé.

Ce qui n'est volontairement pas couvert : la partie hors chaîne du protocole. Les règles de la mempool alternative, la simulation par le bundler et les critères d'exclusion d'une entité fautive sont décrits par l'ERC-7562 et ne sont pas dans ces contrats. `EntryPointSimulations.sol` n'est traité qu'en passant ; il n'est pas destiné à être déployé tel quel.

Enfin, ERC-4337 évolue. Vérifier la version de l'EntryPoint visée avant de transposer ce texte à un autre dépôt.

Retour au [sommaire](../../README.md#parcours-français).
