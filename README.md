### 1. **Différences entre les fichiers séquentiels (TEXT) et les fichiers à accès directs (RANDOM)**

- **Fichiers séquentiels (TEXT)** :
  - Les données sont enregistrées de manière linéaire, dans l’ordre d’écriture.
  - Pour accéder à une donnée spécifique, il faut parcourir le fichier depuis le début jusqu’à l’emplacement de la donnée.
  - Adaptés aux traitements par lots où toutes les données doivent être lues ou traitées.
  - Inconvénient : lenteur d’accès pour une donnée spécifique.

- **Fichiers à accès directs (RANDOM)** :
  - Les données sont organisées pour permettre un accès immédiat à une donnée spécifique via une clé ou un index.
  - Permettent de sauter directement à l’emplacement d’une donnée en utilisant un calcul basé sur une clé.
  - Adaptés aux systèmes nécessitant des accès rapides et fréquents à des données précises.
  - Inconvénient : gestion plus complexe (notamment pour la mise à jour des index).

---

### 2. **Principe de l’organisation des données composites dans un fichier à accès direct (RANDOM)**
- **Organisation des données composites** :
  - Les données composites (comme Nom, Prénom, DateNaissance) sont souvent stockées sous forme de structures organisées, par exemple des enregistrements.
  - Une clé unique, souvent dérivée de l’ensemble des champs (par exemple une clé composée comme `Nom+Prenom+DateNaissance`), est utilisée pour accéder directement à l’enregistrement.

- **Fichier d’index** :
  - Il sert à associer une clé unique à l’emplacement physique de la donnée dans le fichier.
  - Exemple : Une base client avec des clés comme `ClientID`. Le fichier d'index contient les clés et leurs positions dans le fichier de données principal. Lorsqu’une requête utilise une clé, le fichier d’index permet de localiser immédiatement la donnée.

---

### 3. **Fonctions d’un gestionnaire de bases de données (SGBD)**
- **Stockage des données** : Gère le stockage des données sur le disque en respectant une structure logique.
- **Accès concurrentiel** : Permet à plusieurs utilisateurs ou programmes d’accéder simultanément aux données tout en assurant leur cohérence.
- **Sécurité** : Fournit des mécanismes pour contrôler l’accès aux données (authentification, autorisation).
- **Optimisation des requêtes** : Analyse et optimise les requêtes pour exécuter les opérations le plus rapidement possible.
- **Sauvegarde et récupération** : Permet de sauvegarder les données et de les restaurer en cas de panne.
- **Gestion des transactions** : Garantit les propriétés ACID pour assurer la fiabilité des opérations.

---

### 4. **Intégrité des données et notion de transaction**
- **Intégrité des données** :
  - Assure que les données sont correctes, cohérentes et fiables.
  - Inclut des règles comme l’intégrité référentielle (relation parent-enfant dans les tables), l’intégrité d’entité (unicité des clés primaires), etc.

- **Notion de transaction** :
  - Une transaction est une unité logique de travail sur les données, qui peut inclure plusieurs opérations.
  - Elle est soit complètement effectuée, soit complètement annulée.

- **ACID** :
  - **Atomicité** : Tout ou rien. Si une transaction échoue, toutes ses modifications sont annulées.
  - **Cohérence** : Une transaction amène la base d’un état cohérent à un autre état cohérent.
  - **Isolation** : Les transactions concurrentes ne doivent pas interférer.
  - **Durabilité** : Une fois validée, une transaction est enregistrée de façon permanente.

---

### 5. **Déclencheurs (Triggers)** 
- **Définition** :
  - Un trigger est un ensemble d’instructions SQL exécutées automatiquement lorsqu’un événement spécifique se produit sur une table (ex : `INSERT`, `UPDATE`, ou `DELETE`).

- **Avantages** :
  - **Performance** : Permet d’automatiser des tâches récurrentes sans nécessiter d’intervention manuelle ou de programme externe.
  - **Sécurité** : Peut être utilisé pour vérifier des règles métier ou empêcher des modifications non autorisées.

---

### 6. **Cascade DELETE et Cascade UPDATE**
- **Explication** :
  - Ces mécanismes permettent de propager automatiquement des modifications dans une table parent à ses tables enfants.
  - **Cascade DELETE** : Supprime automatiquement les enregistrements enfants associés à un enregistrement parent supprimé.
  - **Cascade UPDATE** : Met automatiquement à jour les clés étrangères dans les enfants lorsqu’une clé primaire est modifiée dans le parent.

- **Intérêt** :
  - Maintient l’intégrité référentielle et réduit les erreurs manuelles lors de la suppression ou de la mise à jour de données.

---

### 7. **Mise à jour perdue**
- **Explication** :
  - Se produit lorsque deux transactions lisent la même donnée, effectuent des modifications, puis enregistrent leur résultat. La première modification est alors écrasée par la seconde.
  - Exemple :
    1. Transaction A lit la valeur `x=100`.
    2. Transaction B lit la valeur `x=100`.
    3. Transaction A met à jour `x=110`.
    4. Transaction B met à jour `x=120` sans tenir compte de la modification de A.

- **Solution** :
  - Utilisation de mécanismes de verrouillage pour empêcher plusieurs transactions d’écrire sur une donnée simultanément.

- **Risque majeur** :
  - Si les verrous sont mal gérés, il peut y avoir un **blocage (deadlock)**.

- **Gestion par le SGBD** :
  - Les SGBD utilisent des protocoles comme les verrous exclusifs, les timestamps ou le multiversioning pour gérer ce problème.

---

### 8. **Accès DB/mémoire Page/mémoire OS/disque dans un SGBD**
- **Fonctionnement** :
  - Les données sont stockées sur le disque dans des pages.
  - Lorsqu’une requête est exécutée, le SGBD :
    1. Charge les pages nécessaires depuis le disque dans la mémoire (mémoire tampon).
    2. Traite les données en mémoire.
    3. Écrit les modifications dans les fichiers journaux avant de mettre à jour les pages sur disque (approche du Write-Ahead Logging).

---

### 9. **Rôles du Backup et du Journal**
- **Backup** :
  - Une copie complète ou partielle des données pour récupérer celles-ci en cas de panne majeure.
  - Utilisé en cas d’incident grave.

- **Journal** :
  - Enregistre toutes les modifications effectuées sur la base.
  - En cas d’incident mineur, permet de rejouer les modifications ou d’annuler une transaction incomplète.
