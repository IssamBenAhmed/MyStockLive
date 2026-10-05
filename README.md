# MyStockLive — Système de Gestion d'Entrepôt & Stocks (WMS)

> Application modulaire de logistique et d'inventaire développée en **C** avec persistance de données relationnelle **SQL**.

MyStockLive est une solution logicielle simulant un système de gestion d'entrepôt (*Warehouse Management System* — WMS) complet.  
Conçu avec une architecture modulaire en C et un modèle de données relationnel rigoureux (`schema.sql`), le système assure la traçabilité intégrale des marchandises, de la réception fournisseur jusqu'à l'expédition client.

---

## 🏗️ Architecture & Modules Métier

Le projet est découpé en modules spécialisés reflétant les différents postes et responsabilités opérationnels au sein d'un entrepôt :

- **🔑 Authentification & Gestion des Rôles (`auth.c`) :**
  - Contrôle d'accès basé sur les rôles (RBAC) pour isoler les prérogatives des opérateurs.
  - Sécurisation des sessions utilisateurs.

- **📦 Catalogue & Référentiel Articles (`catalogue.c`) :**
  - Gestion des fiches articles (SKU, désignations, catégories, unités de conditionnement).
  - Gestion des seuils de réapprovisionnement et des stocks de sécurité.

- **📥 Réception Marchandises (`reception.c`) :**
  - Enregistrement des flux entrants en provenance des fournisseurs.
  - Contrôle quantitatif et affectation des articles aux emplacements physiques de stockage.

- **📋 Préparation de Commandes / Picking (`picker.c`) :**
  - Traitement des commandes clients en attente.
  - Génération des listes de prélèvement (picking) pour guider l'opérateur en entrepôt.

- **🚚 Expédition & Traçabilité (`expedition.c`) :**
  - Contrôle final avant départ, validation des colisages et génération des bons d'expédition.
  - Décrémentation atomique des stocks en base de données.

- **📊 Inventaire & Audit (`inventaire.c`) :**
  - Procédures de comptage physique et rapprochement avec le stock théorique.
  - Journalisation systématique des mouvements de stock et des régularisations d'écarts.

- **📈 Tableau de Bord Superviseur (`superviseur.c`) :**
  - Vue consolidée des stocks, indicateurs de rotation et détection précoce des ruptures.

- **🗄️ Couche d'Accès aux Données (`db.c` & `schema.sql`) :**
  - Schéma relationnel normalisé (3NF) modélisant articles, emplacements, utilisateurs, commandes et mouvements.
  - Exécution des requêtes SQL avec gestion des transactions pour garantir l'intégrité des données.

---

## 📁 Structure du Répertoire

```text
├── include/        # En-têtes C (déclarations des structures et interfaces modules)
├── src/            # Implémentation modulaire (.c)
│   ├── auth.c          # Module d'authentification
│   ├── catalogue.c     # Gestion du catalogue
│   ├── db.c            # Couche de persistance SQL
│   ├── expedition.c    # Sorties et expéditions
│   ├── gestionnaire.c  # Supervision et alertes stocks
│   ├── inventaire.c    # Campagnes d'inventaire
│   ├── picker.c        # Prélèvement / Picking
│   ├── reception.c     # Entrées et réceptions
│   ├── superviseur.c   # Tableaux de bord et statistiques
│   ├── ui.c            # Interface utilisateur console
│   └── main.c          # Point d'entrée de l'application
├── schema.sql      # Script de création du schéma relationnel
└── Makefile        # Script d'automatisation de la compilation
```

---

## 🚀 Compilation et Déploiement

### Prérequis
- Un compilateur C compatible C99 / C11 (`gcc` ou `clang`).
- `make`.
- Moteur de base de données relationnelle (PostgreSQL / MySQL / SQLite selon la configuration de connexion).

### Déploiement de la Base de Données
Initialisez le schéma relationnel en exécutant le script SQL fourni :
```bash
# Exemple avec PostgreSQL
psql -U postgres -d mystocklive -f schema.sql
```

### Compilation du Logiciel
```bash
make
```

### Exécution
```bash
./bin/mystocklive
```
