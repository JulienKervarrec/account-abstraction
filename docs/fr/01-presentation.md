# 1. Présentation

ERC-4337 permet d'utiliser Ethereum avec un compte qui est un contrat, sans modifier le protocole de la chaîne. Les règles de signature, de récupération et de paiement des frais deviennent du code que l'on choisit, au lieu d'être imposées par la clé privée d'un compte externe.

L'utilisateur n'envoie pas une transaction. Il signe une **UserOperation**, une structure de données transmise à un réseau de relayeurs appelés *bundlers*. Un bundler regroupe plusieurs opérations et les soumet au contrat **EntryPoint**, qui les valide puis les exécute. C'est le bundler qui paie le gaz au niveau de la chaîne ; il se rembourse sur le dépôt du compte ou d'un *paymaster*.

Ce dépôt est l'implémentation de référence maintenue par eth-infinitism. Il contient le contrat `EntryPoint` déployé sur la plupart des réseaux EVM, les interfaces que doivent respecter les comptes et les paymasters, et des exemples de comptes.

Ce n'est pas un bundler : la partie hors chaîne (mempool alternative, simulation, regroupement) n'est pas dans ce dépôt.

Suite : [architecture du dépôt](02-architecture.md).
