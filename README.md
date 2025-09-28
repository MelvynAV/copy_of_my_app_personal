# uOttawa - 2024-2025 - SEG2505A - Projet - Groupe 11

## Avis pour la correction du livrable 4 

TRES IMPORTANT : Pour le livrable 4 , remplacer la ligne 14 du code de la classe DatabaseHelper.java 
private static final String DATABASE_NAME = "pc_management.db"; à remplacer par  : 
private static final String DATABASE_NAME = "pc_management_updated.db";

Nom du projet : BuildItTech

## Membres du projet

| Prénom      | NOM         | Identifiant GitHub |
|-------------|-------------|--------------------|
|Olivier      |Storey       |olivierstorey096    |
|Melvyn Avoa  |Avoa         |aavoa037            |

## Introduction

Voici notre dernier livrable "Démonstration". Pour ce livrable nous avons terminer l'implémentation des connexion entre les pages d'utilisateurs pour que le stock soit modifier par non seulement StoreKeeper mais aussi Assembler. 

## Clarifications sur les exigences

Rédaction propre de notre projet BuildItTech.

## Modélisation

Dans le ficher UML.md se trouve les diagrammes de classes, d'état, de séquence et d'activité.

#### Pour le rôle Administrator

Créer, modifier, et supprimer des utilisateurs, Requester, de la base de données.
Vide et réinitialiser la base de données SQLite.
Réinitialise des éléments de stock et initialiser d'autre données de base.

#### Pour le rôle StoreKeeper

Ajouter, mettre à jour, et supprimer des composants du stock.
Consulter le stock et visualizer les  composants existants.
Organiser les composants avec des identifiants uniques et la documentation des temps de changements/modifications.

#### Pour le rôle Assembler

L'Assembler est responsable de la construction des ordinateur.
Choisir les compossants selon les commandes.
Documentation/Mise à jour du stock.
L'Assembler peut visualiser a partir de la base de donnée les commandes placées par le requester puis les accepter ou refuser, lorsqu'il les refuse elles sont effacées du tableau, s'il les accepter il peut grace au numéro de commande les ship.

#### Pour le rôle Requester

Fait les commandes pour les ordinateurs.
Fournit des détails de quels composants ils veulent pour leur ordinateur.

## Éléments de conception

Conception de l'Interface Utilisateur:
 - LinearLayout,
 - Toolbar,
 - EditText,
 - TextView,
 - Button,
 - etc.
Gestion des donnnées:
 - Component.java est une classe représentant les composants avec des attributs comme type, subType, title, quantity, comment.
 - Utilisation de SharedPreferences pour sauvegarder les données.
 - Gestion des erreurs avec les messages, par exemple: "Missing Information"
Méthodes:
 - Approver les commandes des utilisateurs Requester;
 - Rejeter les commandes; et
 - Assembler et renvoyer virtuellement le PC sur mesure.

## Eléments de tests unitaires

Les test unitaires sont implémentés pour chacune des classes. 

#### Utilisateurs

| Rôle           | Identifiant de connexion | Mot de passe |
|----------------|--------------------------|--------------|
| Administrateur |admin@example.com         |admin123      | 
| StoreKeeper    |storekeeper@example.com   |storekeeper123|
| Assembler      |assembler@example.com     |assembler123  |

Les utilisateurs de Requester sont créer par l'administrateur ou par la base de données, qui est géré globalement par l'administrateur.

#### Fichier de données exemple

Le fichier vcs base de donnée est dans les assets. 
Le apk ainsi que le README et le projet complet de l'application sont sur GitHub.

## Choix de développement

Les seuls menus déroulants sur la page Requester qui sont affectés par StoreKeeper sont les deux première cases. Ces cases vont interroger la base de donnée afin d'aficher les éléments de stock créés par StoreKeeper. Ceci est pour démonster que nous sommes capables d'avoir une connexion fonctionelle entre StoreKeeper, Assembler, et Requester, tous des rôles qui ont un impact sur le stock pour l'application, à travers de la base de donnée. 

## Information destinées aux correcteurs

Nous utilisions une base de données fonctionnelle SQLite.
