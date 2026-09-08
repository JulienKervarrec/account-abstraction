# 9. La création du compte

Un compte peut recevoir des fonds avant d'exister : son adresse est calculable à l'avance. Le champ `initCode` sert à le déployer lors de sa première opération.

Hors cas EIP-7702, `initCode` vaut `factory(20 octets) || calldata`. L'EntryPoint ne fait pas l'appel lui-même : il délègue à `SenderCreator`, un contrat séparé déployé par l'EntryPoint dans son constructeur. La raison est une question de confiance — la fabrique ne doit jamais voir l'EntryPoint comme `msg.sender`, sans quoi elle pourrait agir en son nom. `SenderCreator.createSender` vérifie symétriquement que son appelant est bien l'EntryPoint.

L'appel de bas niveau ne récupère que 32 octets : l'adresse du compte créé. En cas d'échec, `sender` reste à zéro et l'opération est rejetée.

`SimpleAccountFactory` illustre l'usage attendu. `createAccount(owner, salt)` déploie un `ERC1967Proxy` via CREATE2, mais **retourne l'adresse existante si le compte est déjà déployé** au lieu d'échouer. C'est ce qui permet à `EntryPoint.getSenderAddress()` de fonctionner avant comme après la création. `getAddress` recalcule la même adresse hors chaîne, sans rien déployer.

Suite : [dépôts et stake](10-depots-et-stake.md).
