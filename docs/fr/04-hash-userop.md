# 4. Le hachage d'une opération

`contracts/core/UserOperationLib.sol` calcule le condensé que l'utilisateur signe. La fonction `encode` reprend les champs dans un ordre fixe, en remplaçant les champs de longueur variable par leur `keccak256` : `initCode`, `callData` et `paymasterAndData` sont hachés avant d'entrer dans l'encodage.

Le premier élément encodé est `PACKED_USEROP_TYPEHASH`. L'ensemble suit la convention EIP-712 : `getDomainSeparatorV4()` dans `EntryPoint.sol` lie le condensé au contrat EntryPoint et à l'identifiant de chaîne. Une même opération signée pour une chaîne n'est donc pas valable sur une autre, ni sur une autre version de l'EntryPoint.

Deux champs sont exclus de l'encodage : `signature`, pour des raisons évidentes, et la signature optionnelle du paymaster, retirée par `paymasterDataKeccak`.

`hash` applique simplement `keccak256` au résultat de `encode`. Le paramètre `overrideInitCodeHash` permet de substituer une valeur au hachage de `initCode` ; il n'est utilisé que dans le cas EIP-7702, décrit au chapitre 13.

Côté public, `EntryPoint.getUserOpHash()` expose ce calcul.

Suite : [l'EntryPoint et handleOps](05-entrypoint-handleops.md).
