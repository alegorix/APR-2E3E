# Cahier des Charges : Création d'un Blog Complet en Architecture MVC

* **Niveau :** Développeur Web — 2ème année et 3ème année
* **Modalité :** Projet individuel
* **Durée totale :** 32 heures (8 séances de 4 heures)
* **Livrable :** Dépôt GitHub complet avec historique de commits réguliers

---

## 1. Contexte et Objectifs Pédagogiques

L'objectif de cet atelier est de concevoir et développer de A à Z un moteur de blog dynamique et sécurisé, sans utiliser de framework PHP (type Symfony ou Laravel). Vous devrez concevoir votre propre architecture **MVC (Modèle-Vue-Contrôleur)** orientée objet.

### Compétences visées :
* Concevoir une architecture logicielle propre et maintenable (MVC, routage, autoloading).
* Modéliser et interroger une base de données relationnelle avec **PDO** (requêtes préparées).
* Sécuriser une application web (failles XSS, injections SQL, CSRF, hachage de mots de passe, gestion des rôles).
* Séparer strictement l'intégration Front-Office (sur-mesure) de l'interface d'administration Back-Office.
* Versionner son code efficacement avec **Git** et **GitHub**.

---

## 2. Contraintes Techniques

### Stack Technologique
* **Langage Backend :** PHP 8.2+ (Programmation Orientée Objet stricte recommandée).
* **Base de données :** MySQL / MariaDB via l'extension **PDO** uniquement.
* **Architecture :** Pattern **MVC** maison (un point d'entrée unique `public/index.php` avec réécriture d'URL via `.htaccess` ou routeur PHP).
* **Frontend (Partie publique / Lecteurs) :** HTML5 sémantique et CSS3 **100% natif** (Flexbox, Grid, variables CSS). **Aucun framework CSS (ni Bootstrap, ni Tailwind) n'est autorisé sur le front-office.**
* **Backend (Partie administration / Back-office) :** Libre choix entre **Bootstrap (v5/v6)** ou **Tailwind CSS** pour accélérer le développement de l'interface d'administration.
* **JavaScript :** Vanilla JS autorisé (et encouragé pour l'UX : confirmations de suppression, prévisualisation d'image, éditeur de texte, etc.).

### Bonnes pratiques & Sécurité obligatoires
* **Zéro requête SQL dans les contrôleurs ou les vues** (tout passe par les Modèles / Repositories).
* **Protection contre les injections SQL :** Utilisation systématique des requêtes préparées (`prepare()` / `execute()`).
* **Protection contre les failles XSS :** Échappement systématique des données affichées (`htmlspecialchars()`).
* **Protection CSRF :** Mise en place de jetons (tokens) sur les formulaires sensibles (suppression, modification, connexion).
* **Gestion des mots de passe :** `password_hash()` et `password_verify()`.
* **Gestion des fichiers sensibles :** Les identifiants de connexion à la BDD ne doivent pas être versionnés en clair (utiliser un fichier `.env` ou un `config.example.php` + `.gitignore`).

---

## 3. Périmètre Fonctionnel

### A. Front-Office (Espace Public — CSS Natif)
1. **Page d'accueil :**
   * Affichage des derniers articles publiés (titre, image à la une, extrait, date de publication, auteur, catégorie).
   * Système de **pagination** (ex: 5 ou 6 articles par page).
2. **Filtrage et Recherche :**
   * Filtrer les articles par **catégorie**.
   * Moteur de **recherche** par mot-clé (dans le titre et le contenu).
3. **Page de lecture d'un article :**
   * Affichage complet de l'article (contenu mis en forme, auteur, date, catégorie).
   * Espace **Commentaires** : affichage des commentaires validés et formulaire pour poster un nouveau commentaire (réservé aux utilisateurs connectés ou ouvert à tous avec modération).
4. **Authentification :**
   * Inscription (Pseudo, Email, Mot de passe) avec validation des données côté serveur.
   * Connexion / Déconnexion.
   * Page de profil basique (modification des infos personnelles / mot de passe).

### B. Back-Office (Espace Administration — Bootstrap ou Tailwind)
*Accès strictement restreint aux utilisateurs ayant le rôle `ADMIN` (ou `AUTHOR`). Redirection automatique si accès non autorisé.*

1. **Tableau de bord (Dashboard) :**
   * Statistiques clés : nombre total d'articles, de commentaires (dont ceux en attente de modération), d'utilisateurs et de catégories.
2. **Gestion des Articles (CRUD complet) :**
   * Lister tous les articles (brouillons et publiés).
   * Créer / Modifier un article (Titre, Slug, Contenu, Catégorie, Statut : *Brouillon / Publié*, Upload d'une image de couverture sécurisé).
   * Supprimer un article (avec confirmation).
3. **Gestion des Catégories (CRUD) :**
   * Créer, renommer, supprimer des catégories.
4. **Modération des Commentaires :**
   * Approuver, rejeter ou supprimer les commentaires postés par les utilisateurs.
5. **Gestion des Utilisateurs :**
   * Lister les membres inscrits.
   * Changer le rôle d'un utilisateur (`USER` $\leftrightarrow$ `ADMIN`) ou bannir/supprimer un compte.

---

## 4. Architecture Suggérée du Projet

```text
mon-blog-mvc/
├── config/
│   └── database.example.php  # Modèle de configuration BDD
├── database/
│   └── schema.sql            # Script de création des tables + jeu de données (fixtures)
├── public/                   # Dossier racine exposé par le serveur web
│   ├── index.php             # Front Controller (point d'entrée unique)
│   ├── .htaccess             # Réécriture d'URL
│   ├── assets/
│   │   ├── css/              # Votre CSS natif pour le Front-Office
│   │   ├── js/               # Scripts JS
│   │   └── uploads/          # Images uploadées pour les articles
├── src/
│   ├── Controllers/          # Contrôleurs (FrontController, AdminController, AuthController...)
│   ├── Core/                 # Classes cœurs (Router, Database, Controller, Model, Session...)
│   ├── Models/               # Modèles / Managers (ArticleModel, UserModel, CommentModel...)
│   └── Views/                # Vues PHP
│       ├── front/            # Templates du site public
│       ├── admin/            # Templates du back-office
│       └── layouts/          # Gabarits globaux (header, footer, navbar)
├── .gitignore
└── README.md
```

---

## 5. Planning Prévisionnel (32h — 8 Séances de 4h)

Ce découpage est indicatif pour vous aider à gérer votre temps et jalonner vos commits Git.

* **Séance 1 (4h) : Conception & Initialisation**
  * Création du dépôt GitHub, du `.gitignore` et de l'arborescence.
  * Conception du MCD/MLD et création du script `schema.sql` (Tables : `users`, `categories`, `articles`, `comments`).
  * Mise en place de la classe `Database` (Singleton PDO) et du `Router` de base.
* **Séance 2 (4h) : Noyau MVC & Affichage Public (Lecture)**
  * Création des classes mères `Controller` et `Model`.
  * Développement de `ArticleModel` et affichage de la liste des articles et de la vue détaillée d'un article.
* **Séance 3 (4h) : Intégration Front-Office (HTML/CSS Natif)**
  * Réalisation de la maquette HTML/CSS sans framework (Responsive Design, Grid/Flexbox, typographie soignée).
  * Implémentation de la pagination, du filtre par catégorie et de la barre de recherche.
* **Séance 4 (4h) : Authentification & Gestion des Sessions**
  * Création de `UserModel` et `AuthController`.
  * Formulaires d'inscription et de connexion, validation des champs, gestion des sessions et messages flash.
* **Séance 5 (4h) : Back-Office — Mise en place & CRUD Articles**
  * Intégration du template Back-Office (Bootstrap ou Tailwind) et sécurisation des routes `/admin`.
  * Développement du CRUD complet des articles + gestion de l'upload d'images (vérification du type MIME, taille, renommage unique).
* **Séance 6 (4h) : CRUD Catégories & Système de Commentaires**
  * Gestion des catégories dans le back-office.
  * Ajout de commentaires sur le front-office et interface de modération côté admin.
* **Séance 7 (4h) : Gestion des Utilisateurs & Sécurisation globale**
  * Gestion des rôles utilisateurs dans le back-office.
  * Audit de sécurité : vérification des failles XSS, ajout des tokens CSRF sur les formulaires, gestion propre des erreurs 404 et 403.
* **Séance 8 (4h) : Tests, Peaufinage & Documentation**
  * Jeu de données complet (`schema.sql` à jour avec un compte admin de test).
  * Tests fonctionnels complets (parcours utilisateur et administrateur).
  * Rédaction d'un `README.md` professionnel (procédure d'installation, identifiants de test, captures d'écran).

---

## 6. Livrables Attendus sur GitHub

À la fin des 32 heures, votre dépôt GitHub devra contenir :
1. Le **code source complet** et fonctionnel.
2. Un fichier `database/schema.sql` permettant de recréer la base de données en un clic (incluant quelques articles, catégories et un compte administrateur par défaut).
3. Un fichier `config/database.example.php` prêt à être copié/renommé.
4. Un fichier `README.md` clair expliquant :
   * Le contexte du projet.
   * Les prérequis et les étapes d'installation pas à pas.
   * Les identifiants du compte Administrateur de test (ex: `admin@blog.fr` / `Admin1234!`).
5. Un **historique Git propre** (commits atomiques avec des messages explicites tout au long des 8 séances).

---

## 7. Grille d'Évaluation (Barème indicatif / 20)

| Critère | Détails | Points |
| :--- | :--- | :---: |
| **Architecture MVC & POO** | Respect de la séparation des responsabilités, propreté du routage, code réutilisable et lisible. | **/ 5** |
| **Base de données & PDO** | Pertinence du modèle relationnel, requêtes préparées systématiques, gestion des jointures. | **/ 4** |
| **Fonctionnalités Front & Back** | CRUD complets fonctionnels, pagination, recherche, modération, upload d'image. | **/ 5** |
| **Sécurité** | Protection XSS, CSRF, SQLi, hashage des mots de passe, contrôle d'accès aux routes admin. | **/ 3** |
| **Intégration UI/UX** | Qualité du CSS natif sur le Front (responsive, esthétique) et ergonomie du Back-Office. | **/ 2** |
| **Git & Documentation** | Régularité des commits, `.gitignore` propre, qualité du `README.md` et du fichier SQL. | **/ 1** |
