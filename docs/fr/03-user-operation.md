# 3. La structure UserOperation

Tout part de `PackedUserOperation`, déclarée dans `contracts/interfaces/PackedUserOperation.sol`. Elle compte neuf champs.

`sender` est le compte concerné. `nonce` protège contre le rejeu. `initCode` sert à créer le compte s'il n'existe pas encore. `callData` est l'appel à exécuter sur le compte. `paymasterAndData` désigne un éventuel payeur de frais. `signature` autorise l'ensemble.

Trois champs sont *packés*, c'est-à-dire que deux valeurs partagent un même mot de 32 octets afin de réduire le coût en calldata :

- `accountGasLimits` = `uint128(verificationGasLimit) || uint128(callGasLimit)`
- `gasFees` = `uint128(maxPriorityFeePerGas) || uint128(maxFeePerGas)`
- `nonce` = `uint192(key) || uint64(sequence)`

Le champ `preVerificationGas` reste seul : il couvre le surcoût que l'EntryPoint ne peut pas mesurer lui-même, notamment la part de calldata de la transaction du bundler.

Ce découpage explique le nom « Packed » : il distingue cette version de la structure `UserOperation` de la v0.6, conservée dans `contracts/legacy/v06/`.

Suite : [le hachage d'une opération](04-hash-userop.md).
