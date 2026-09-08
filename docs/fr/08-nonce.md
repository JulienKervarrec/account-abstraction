# 8. Les nonces à deux dimensions

`contracts/core/NonceManager.sol` tient une table `mapping(address => mapping(uint192 => uint256))`. Le `nonce` d'une opération se lit comme deux valeurs collées : une **clé** de 192 bits et une **séquence** de 64 bits.

```solidity
uint192 key = uint192(nonce >> 64);
uint64  seq = uint64(nonce);
return nonceSequenceNumber[sender][key]++ == seq;
```

Chaque clé possède son propre compteur, incrémenté indépendamment des autres. Un compte peut donc avoir plusieurs files d'opérations en parallèle : une opération bloquée sur la clé 0 n'empêche pas celles de la clé 1 de passer. Un compte classique n'utilise que la clé 0 et retrouve un comportement séquentiel ordinaire.

`getNonce(sender, key)` recompose la valeur complète attendue, en replaçant la clé dans les bits hauts. `incrementNonce(key)` permet à un compte de sauter volontairement une valeur, par exemple pour annuler une opération signée mais non encore incluse.

`_validateAndUpdateNonce` est appelée juste après `validateUserOp`, et incrémente le compteur même si la comparaison échoue — l'opération est alors rejetée.

Suite : [la création du compte](09-creation-compte.md).
