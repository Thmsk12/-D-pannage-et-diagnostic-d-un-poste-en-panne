---
title: Projet 4 — Dépannage et diagnostic d'un poste en panne

---

# Projet 4 — Dépannage et diagnostic d'un poste en panne

**Cours :** IT Essentials — Mettre en service un poste de travail ICT avec le système d'exploitation (ICT-187)
**Classe :** E1A
**Niveau :** Intermédiaire
**Étudiant :** Thomas Kalomiris
**Date :** 17/09/2026

---

# 1. Contexte

Dans le cadre de ce projet, une panne a été simulée sur un poste de travail afin de mettre en pratique une méthodologie de dépannage informatique.

Le problème choisi concerne la communication réseau entre deux machines virtuelles :

* **Machine 1 :** Ubuntu
* **Machine 2 :** Windows
* **Protocole testé :** ICMPv4
* **Outil utilisé :** `ping`

La panne simulée consiste à désactiver dans le pare-feu Windows la règle permettant de recevoir les requêtes d'écho **ICMPv4**.

L'objectif est d'identifier la cause du problème, de résoudre la panne et de vérifier que la communication fonctionne à nouveau.

---

# 2. Objectifs

Les objectifs de cette intervention sont les suivants :

* Identifier un problème de communication réseau.
* Vérifier la configuration réseau des deux machines.
* Tester la connectivité avec la commande `ping`.
* Formuler et tester plusieurs hypothèses.
* Identifier le rôle du pare-feu Windows dans le problème.
* Identifier la règle ICMPv4 responsable du blocage.
* Réactiver la règle afin de résoudre le problème.
* Vérifier que le problème est effectivement résolu.
* Documenter l'ensemble de la procédure sous la forme d'un rapport d'intervention.

---

# 3. Environnement de test

Le dépannage a été réalisé dans un environnement virtualisé.

| Élément         | Configuration                             |
| --------------- | ----------------------------------------- |
| Machine 1       | Ubuntu                                    |
| Machine 2       | Windows                                   |
| Virtualisation  | VMware Workstation                        |
| Communication   | Réseau entre les deux machines virtuelles |
| Protocole testé | ICMPv4                                    |
| Outil de test   | `ping`                                    |
| Pare-feu        | Pare-feu Windows Defender                 |

---

# 4. Symptôme constaté

Le problème constaté est le suivant :

> La machine Ubuntu ne reçoit plus de réponse lorsqu'elle tente d'effectuer un `ping` vers la machine Windows.

Avant de modifier la configuration, une vérification de la connectivité est effectuée afin de disposer d'un état de référence.

---

# 5. Vérification de la configuration réseau

## 5.1 Vérification de l'adresse IP de Windows

La commande suivante a été utilisée dans l'invite de commandes Windows :

```cmd
ipconfig
```

Cette commande permet de vérifier l'adresse IPv4, le masque de sous-réseau et les autres paramètres réseau de la machine.

### 📸 Capture  — Configuration réseau Windows

![Capture d'écran 2026-09-17 110022](https://hackmd.io/_uploads/ByyhG4tFfl.png)


---

## 5.2 Vérification de l'adresse IP d'Ubuntu

Sur Ubuntu, la commande suivante a été utilisée :

```bash
ip a
```

Cette commande permet de connaître l'adresse IP attribuée à l'interface réseau de la machine.

### 📸 Capture  — Configuration réseau Ubuntu

![Capture d'écran 2026-09-17 091253](https://hackmd.io/_uploads/rkKz7VFFGl.png)



---

# 6. Test de connectivité initial

Avant de provoquer la panne, un test de communication a été effectué depuis Ubuntu vers Windows avec la commande :

```bash
ping IP_DE_WINDOWS
```

Le test initial permet de vérifier que les deux machines peuvent communiquer correctement.

Les réponses reçues indiquaient que la communication fonctionnait avant la simulation de la panne.

### 📸 Capture  — Ping fonctionnel avant la panne

![Capture d'écran 2026-09-17 110450](https://hackmd.io/_uploads/BkPqQVtYzx.png)



---

# 7. Simulation de la panne

Afin de reproduire un problème de communication, la règle du pare-feu Windows autorisant les requêtes d'écho ICMPv4 a été désactivée.

Le pare-feu Windows a été ouvert avec :

```text
wf.msc
```

Puis la section suivante a été consultée :

```text
Pare-feu Windows Defender avec fonctions avancées de sécurité
→ Règles de trafic entrant
```

La règle correspondant aux requêtes d'écho ICMPv4 a ensuite été désactivée.

La règle concernée est :

> Partage de fichiers et d'imprimantes (requête d'écho - ICMPv4-Entrée)

### 📸 Capture  — Règle ICMPv4 désactivée

![Capture d'écran 2026-09-17 102629](https://hackmd.io/_uploads/r17aQNKYMe.png)
![Capture d'écran 2026-09-17 110404](https://hackmd.io/_uploads/SkaA7VFYMl.png)
![Capture d'écran 2026-09-17 110345](https://hackmd.io/_uploads/Sk6LENYYzl.png)




---

# 8. Reproduction de la panne

Après avoir désactivé la règle ICMPv4, un nouveau test a été réalisé depuis Ubuntu vers Windows :

```bash
ping IP_DE_WINDOWS
```

Le poste Ubuntu n'a alors plus reçu de réponse de la machine Windows.

Le test permet de reproduire le problème signalé.

### 📸 Capture  — Ping échoué

![image](https://hackmd.io/_uploads/HkMoEVYYMx.png)


Le résultat montre que les paquets ICMP envoyés depuis Ubuntu ne reçoivent pas de réponse de Windows.

---

# 9. Hypothèses de diagnostic

Afin de respecter une démarche de dépannage structurée, plusieurs hypothèses ont été envisagées et testées.

## Hypothèse 1 — Mauvaise configuration IP

La première hypothèse était que l'une des machines pouvait avoir une mauvaise configuration réseau.

Les commandes suivantes ont été utilisées :

```cmd
ipconfig
```

sur Windows et :

```bash
ip a
```

sur Ubuntu.

Les adresses IP et les paramètres réseau ont été vérifiés.

**Résultat :** la configuration IP des deux machines était correcte.

![image](https://hackmd.io/_uploads/r1c0NEYKGe.png)
![image](https://hackmd.io/_uploads/BJmeBEYYGe.png)


➡️ **Hypothèse écartée.**

---

## Hypothèse 2 — Problème général de communication réseau

Un test `ping` a été effectué afin de vérifier la communication entre les deux machines.

Le problème observé était spécifique à la communication vers Windows.

Un test depuis Windows vers Ubuntu a également été effectué :

```cmd
ping IP_DE_UBUNTU
```

La communication dans ce sens fonctionnait.

### 📸 Capture  — Ping Windows vers Ubuntu

![image](https://hackmd.io/_uploads/ryJHBVKYfg.png)


**Résultat :** Ubuntu était accessible depuis Windows.

➡️ Cette vérification indique que le problème ne correspond pas à une coupure complète du réseau.

---

## Hypothèse 3 — Pare-feu Windows bloquant les requêtes ICMPv4

La troisième hypothèse était qu'une règle du pare-feu Windows bloquait les requêtes ICMPv4 entrantes.

Les règles de trafic entrant du pare-feu Windows ont été examinées.

La règle :

> **Partage de fichiers et d'imprimantes (requête d'écho - ICMPv4-Entrée)**

était désactivée.

### 📸 Capture  — Identification de la règle ICMPv4

![image](https://hackmd.io/_uploads/rJl5H4ttMx.png)


**Résultat :** l'hypothèse du blocage par le pare-feu est confirmée.

➡️ **Cause identifiée.**

---

# 10. Diagnostic

Après les différents tests effectués, le diagnostic retenu est le suivant :

> **La machine Windows ne répondait pas aux requêtes ICMPv4 entrantes car la règle du pare-feu Windows autorisant les requêtes d'écho ICMPv4 était désactivée.**

La configuration IP était correcte et la communication réseau entre les deux machines fonctionnait dans l'autre sens.

Le problème était donc lié à la configuration du pare-feu Windows.

---

# 11. Résolution du problème

Pour résoudre le problème, la règle ICMPv4 précédemment désactivée a été réactivée.

Chemin utilisé :

```text
Pare-feu Windows Defender avec fonctions avancées de sécurité
→ Règles de trafic entrant
→ Partage de fichiers et d'imprimantes
→ Requête d'écho - ICMPv4
→ Activer la règle
```

### 📸 Capture d'écran 8 — Règle ICMPv4 réactivée

![Capture d'écran 2026-09-17 113341](https://hackmd.io/_uploads/B143SEFKzg.png)


La règle est maintenant active.

---

# 12. Vérification après résolution

Après avoir réactivé la règle, un nouveau test a été effectué depuis Ubuntu :

```bash
ping IP_DE_WINDOWS
```

Cette fois, Ubuntu reçoit de nouveau des réponses de la machine Windows.

Les réponses sont de la forme :

```text
64 bytes from IP_DE_WINDOWS: icmp_seq=1 ttl=128 time=...
```

Le test confirme que la communication fonctionne de nouveau.

### 📸 Capture — Ping fonctionnel après réparation

![image](https://hackmd.io/_uploads/Sy9zUNFYMl.png)


---

# 13. Vérification finale

Une vérification supplémentaire a été réalisée afin de confirmer que la communication entre les deux machines est fonctionnelle.

Les tests suivants peuvent être utilisés :

### Ubuntu → Windows

```bash
ping IP_DE_WINDOWS
```

### Windows → Ubuntu

```cmd
ping IP_DE_UBUNTU
```

Les deux machines peuvent ainsi communiquer correctement.

### 📸 Capture  — Vérification finale

![image](https://hackmd.io/_uploads/H1odIEFFGg.png)
![image](https://hackmd.io/_uploads/B17c8VtKMg.png)


---

# 14. Résumé de l'intervention

| Élément               | Résultat                                                      |
| --------------------- | ------------------------------------------------------------- |
| Symptôme              | Ubuntu ne reçoit plus de réponse lors d'un ping vers Windows  |
| Configuration IP      | Correcte                                                      |
| Connectivité réseau   | Fonctionnelle                                                 |
| Règle ICMPv4          | Désactivée                                                    |
| Cause                 | Blocage des requêtes ICMPv4 entrantes par le pare-feu Windows |
| Action corrective     | Réactivation de la règle ICMPv4                               |
| Test après réparation | Ping fonctionnel                                              |
| État final            | Poste fonctionnel                                             |

---

# 15. Méthodologie de dépannage utilisée

La démarche suivie respecte les différentes étapes d'un dépannage informatique :

### 1. Identifier

Le symptôme a été identifié : le ping Ubuntu → Windows ne recevait aucune réponse.

### 2. Isoler

La configuration IP a été vérifiée et un test dans l'autre sens a été réalisé afin de déterminer si le problème concernait l'ensemble de la communication réseau.

### 3. Tester

Plusieurs tests ont été effectués avec :

* `ipconfig`
* `ip a`
* `ping`
* les règles du pare-feu Windows

### 4. Diagnostiquer

L'analyse de la configuration du pare-feu a permis d'identifier la règle ICMPv4 désactivée.

### 5. Résoudre

La règle ICMPv4 a été réactivée.

### 6. Vérifier

Un nouveau `ping` a été effectué depuis Ubuntu vers Windows et les réponses ont été reçues.

---

# 16. Recommandations

Après résolution du problème, il est recommandé de :

* Vérifier régulièrement les règles du pare-feu Windows.
* Ne pas désactiver complètement le pare-feu pour résoudre un problème de connexion.
* Modifier uniquement les règles nécessaires.
* Documenter toute modification de configuration réseau ou de sécurité.
* Tester la connectivité après chaque modification afin de vérifier son impact.

---

# 17. Conclusion

Le problème de communication entre Ubuntu et Windows a été reproduit volontairement afin de réaliser le dépannage.

La configuration réseau des deux machines a d'abord été vérifiée. Les tests ont ensuite permis d'isoler le problème au niveau de la machine Windows.

L'analyse du pare-feu a permis d'identifier une règle **ICMPv4 entrante désactivée** comme cause du problème.

La réactivation de cette règle a permis de rétablir la communication entre Ubuntu et Windows.

Le test `ping` effectué après la réparation confirme que le poste fonctionne à nouveau normalement.

**Intervention terminée : problème identifié, corrigé et vérifié.**
