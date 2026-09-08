# 6. La validation côté compte

Un compte compatible implémente `IAccount`, qui ne contient qu'une fonction :

```solidity
function validateUserOp(
    PackedUserOperation calldata userOp,
    bytes32 userOpHash,
    uint256 missingAccountFunds
) external returns (uint256 validationData);
```

Le compte doit vérifier deux choses : que l'appelant est bien l'EntryPoint, et que la signature correspond à `userOpHash`. L'interface impose une règle contre-intuitive : **un échec de signature ne doit pas provoquer de revert**, mais retourner la valeur `SIG_VALIDATION_FAILED` (1). C'est ce qui rend possible la simulation d'une opération avec une signature factice. Les autres erreurs, elles, doivent bien faire échouer l'appel.

`missingAccountFunds` est le montant que le compte doit transférer à l'EntryPoint pour couvrir l'avance de frais. Il vaut zéro si un paymaster prend les frais en charge ou si le dépôt du compte suffit déjà. Le surplus éventuel reste en dépôt et se retire plus tard avec `withdrawTo`.

Le code de validation ne doit pas lire `block.timestamp` directement : la fenêtre de validité passe par la valeur de retour, expliquée au chapitre suivant.

`contracts/core/BaseAccount.sol` fournit une base qui gère ces obligations.

Suite : [la valeur validationData](07-validation-data.md).
