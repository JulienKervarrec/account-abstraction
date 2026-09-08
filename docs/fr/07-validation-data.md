# 7. La valeur validationData

`validateUserOp` retourne un seul `uint256` qui encode trois informations. `contracts/core/Helpers.sol` définit la structure correspondante et les fonctions de conversion.

| Bits | Champ | Rôle |
| --- | --- | --- |
| 0-159 | `aggregator` | `0` = signature validée par le compte, `1` = échec de signature, sinon adresse d'un agrégateur |
| 160-207 | `validUntil` | dernier horodatage de validité ; `0` signifie « sans limite » |
| 208-255 | `validAfter` | premier horodatage de validité |

`_packValidationData` construit cette valeur, `_parseValidationData` la relit. À la lecture, un `validUntil` nul est converti en `type(uint48).max`, ce qui évite de traiter le cas « pas de limite » comme une date passée.

Le passage par une valeur de retour, plutôt que par une lecture directe de `block.timestamp` dans le compte, permet au bundler de simuler l'opération hors chaîne puis de décider lui-même si la fenêtre est encore ouverte au moment où il l'inclut.

Un paymaster retourne la même structure, mais l'adresse d'agrégateur n'y est pas admise : seules les valeurs 0 et 1 ont un sens.

Suite : [les nonces à deux dimensions](08-nonce.md).
