# 2. Architecture du dépôt

Le code Solidity est dans `contracts/`, réparti en quatre ensembles.

`contracts/interfaces/` définit les contrats que les tiers doivent respecter : `IAccount` pour un compte, `IPaymaster` pour un payeur de frais, `IAggregator` pour un agrégateur de signatures, `IEntryPoint` pour l'EntryPoint lui-même. La structure `PackedUserOperation` y est également déclarée.

`contracts/core/` contient l'implémentation. `EntryPoint.sol` en est le cœur, avec un millier de lignes ; il hérite de `StakeManager` et de `NonceManager`. Les autres fichiers isolent une responsabilité : encodage des opérations (`UserOperationLib`), décodage des résultats de validation (`Helpers`), création des comptes (`SenderCreator`), support d'EIP-7702 (`Eip7702Support`), classes de base pour les intégrateurs (`BaseAccount`, `BasePaymaster`).

`contracts/accounts/` fournit des comptes d'exemple : `SimpleAccount`, sa fabrique, et `Simple7702Account`.

`contracts/test/` et `contracts/legacy/` ne servent pas en production : le premier alimente la suite de tests, le second conserve les interfaces de la version 0.6.

Suite : [la structure UserOperation](03-user-operation.md).
