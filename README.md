# MiniDoc-Angular-NestJS

## 1. Qu'est-ce que NestJS ?

NestJS est un framework Node.js pour construire des applications côté serveur robustes, évolutives et bien structurées. Il utilise TypeScript par défaut et suit une architecture inspirée de MVC (Modèle-Vue-Contrôleur) combinée avec les concepts modernes comme les modules et les décorateurs.

## 2. 2. Qu'est-ce qu'Angular ?
Angular est un framework de développement côté client utilisé pour créer des applications web interactives. Écrit en TypeScript, il suit une architecture basée sur les composants et est parfait pour intégrer avec une API comme NestJS.

## 3. Pré-requis
Avant de commencer :
Installez Node.js (version LTS recommandée).
Téléchargez et installez depuis Node.js.
Vérifiez l'installation :

```
node -v
npm -v

```
Installez NestJS CLI pour créer des projets backend :

```
npm install -g @nestjs/cli
```

Installez Angular CLI pour créer des projets frontend :

```
npm install -g @angular/cli
```
Configurez une base de données MySQL :
 - Téléchargez et installez MySQL sur votre machine.
 - Créez une base de données projectdb.

Installez un IDE comme IntelliJ IDEA, WebStorm, ou Visual Studio Code, avec :
- Plugin Angular pour le frontend.
- Plugin NestJS pour le backend.
  
(Intelij & Webstorm ont par défaut déjà ces plugins)

## 4. Concepts de Base
Backend : NestJS

    Module :
        Un module dans NestJS regroupe des fonctionnalités spécifiques (exemple : UserModule gère tout ce qui concerne les utilisateurs).
        C'est une unité logique qui contient des services, contrôleurs et entités.

    Service :
        Un service est utilisé pour encapsuler la logique métier (exemple : accès à la base de données, calculs, etc.).
        Les services sont injectables dans d'autres parties de l'application.

    Contrôleur :
        Un contrôleur gère les requêtes entrantes et retourne une réponse. Il agit comme un pont entre l'utilisateur et le service.

    Entité :
        Une entité représente un modèle de données (exemple : User, Article).
        Utilisée avec TypeORM pour gérer la base de données.

Documentation Complète Angular + NestJS : Tout Depuis le Début
Introduction
1. Qu'est-ce que NestJS ?

NestJS est un framework Node.js pour construire des applications côté serveur robustes, évolutives et bien structurées. Il utilise TypeScript par défaut et suit une architecture inspirée de MVC (Modèle-Vue-Contrôleur) combinée avec les concepts modernes comme les modules et les décorateurs.
2. Qu'est-ce qu'Angular ?

Angular est un framework de développement côté client utilisé pour créer des applications web interactives. Écrit en TypeScript, il suit une architecture basée sur les composants et est parfait pour intégrer avec une API comme NestJS.
3. Pré-requis

Avant de commencer :

    Installez Node.js (version LTS recommandée).
    Téléchargez et installez depuis Node.js.
    Vérifiez l'installation :

node -v
npm -v

Installez NestJS CLI pour créer des projets backend :

npm install -g @nestjs/cli

Installez Angular CLI pour créer des projets frontend :

    npm install -g @angular/cli

    Configurez une base de données MySQL :
        Téléchargez et installez MySQL sur votre machine.
        Créez une base de données projectdb.

    Installez un IDE comme IntelliJ IDEA, WebStorm, ou Visual Studio Code, avec :
        Plugin Angular pour le frontend.
        Plugin NestJS pour le backend.

4. Concepts de Base
Backend : NestJS

```
    Module :
        Un module dans NestJS regroupe des fonctionnalités spécifiques (exemple : UserModule gère tout ce qui concerne les utilisateurs).
        C'est une unité logique qui contient des services, contrôleurs et entités.

    Service :
        Un service est utilisé pour encapsuler la logique métier (exemple : accès à la base de données, calculs, etc.).
        Les services sont injectables dans d'autres parties de l'application.

    Contrôleur :
        Un contrôleur gère les requêtes entrantes et retourne une réponse. Il agit comme un pont entre l'utilisateur et le service.

    Entité :
        Une entité représente un modèle de données (exemple : User, Article).
        Utilisée avec TypeORM pour gérer la base de données.
```
Frontend : Angular
```
    Composant :
        Un composant est une unité visuelle de l'interface utilisateur.
        Chaque composant a une logique (.ts), un modèle (.html), et un style (.css ou .scss).

    Service :
        Un service dans Angular est similaire à NestJS. Il est utilisé pour encapsuler la logique métier, comme les appels HTTP.

    Template :
        Les templates Angular définissent ce qui est affiché dans le navigateur.
        Avec Angular 17, des nouvelles syntaxes comme @for et @if simplifient le rendu conditionnel.
```

## 5. Étape 1 : Initialiser le Backend

### Création du projet

Créez un nouveau projet NestJS :
```
nest new backend
cd backend
```

Installez TypeORM et MySQL : (biensur meme principe si on souhaite installer un autre package)
```
npm install @nestjs/typeorm typeorm mysql

```
### Créer un module, un service et un controller User

Générer via le cli : 

```
nest generate module user
nest generate service user
nest generate controller user

```
### Création de l'entité User
L'entité User représente les utilisateurs de l'application.
Fichier : ```src/user/user.entity.ts```

```ts
import { Entity, PrimaryGeneratedColumn, Column, OneToMany } from 'typeorm';
import { Article } from '../article/article.entity';

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column()
  email: string;

  @OneToMany(() => Article, (article) => article.user)
  articles: Article[];
}
```
### Création de l'entité Article 
Fichier : ```src/article/article.entity.ts```
```ts
import { Entity, PrimaryGeneratedColumn, Column, ManyToOne } from 'typeorm';
import { User } from '../user/user.entity';

@Entity()
export class Article {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  title: string;

  @Column()
  content: string;

  @ManyToOne(() => User, (user) => user.articles, { eager: true, onDelete: 'CASCADE' })
  user: User;
}
```

La Relation : User et Article
Type de Relation : One-To-Many et Many-To-One

    Côté User :
    Un utilisateur peut écrire plusieurs articles.
    Cela définit une relation One-To-Many (Un à plusieurs).
    Chaque utilisateur a une liste d'articles qu'il a écrits.

    Côté Article :
    Chaque article appartient à un seul utilisateur.
    Cela définit une relation Many-To-One (Plusieurs à un).
    Un article a une propriété user, qui fait référence à son auteur.

### Création du service User 
Le service encapsule la logique pour récupérer et gérer les utilisateurs.
Pour interagir avec la base de données, nous utilisons des repositories, qui offrent des méthodes par défaut comme find, create, ou delete, permettant de simplifier les opérations courantes. Ces méthodes peuvent être enrichies avec des conditions comme where pour filtrer les résultats, ou avec des options pour récupérer des relations, par exemple : obtenir la liste des articles écrits par un utilisateur (jointure) ou, à l'inverse, récupérer l'auteur d'un article.

Fichier : ```src/user/user.service.ts```

```ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './user.entity';

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
  ) {}

  findAll(): Promise<User[]> {
    return this.userRepository.find();
  }

  findById(id: number): Promise<User> {
    return this.userRepository.findOne({ where: { id }, relations: ['articles'] });
  }

  create(user: User): Promise<User> {
    return this.userRepository.save(user);
  }
}

```

### Contrôleur User
Le contrôleur User gère les requêtes HTTP liées aux utilisateurs. Il permet de récupérer tous les utilisateurs, de récupérer un utilisateur spécifique avec ses articles, et d'ajouter un nouvel utilisateur.

Fichier : ```src/user/user.controller.ts```
```ts
import { Controller, Get, Post, Param, Body } from '@nestjs/common';
import { UserService } from './user.service';
import { User } from './user.entity';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get()
  findAll(): Promise<User[]> {
    return this.userService.findAll();
  }

  @Get(':id')
  findById(@Param('id') id: number): Promise<User> {
    return this.userService.findById(id);
  }

  @Post()
  create(@Body() user: User): Promise<User> {
    return this.userService.create(user);
  }
}

```
### Consommation de l'API
- 1. Récupérer tous les utilisateurs avec leurs articles
     URL : GET http://localhost:3000/users
```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  },
  {
    "id": 2,
    "name": "Jane Smith",
    "email": "jane@example.com"
  }
]

```
- 2. Récupérer un utilisateur par ID avec ses articles
     URL : GET http://localhost:3000/users/1

```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "articles": [
    {
      "id": 1,
      "title": "Introduction to NestJS",
      "content": "NestJS is a framework for building server-side applications."
    },
    {
      "id": 2,
      "title": "Advanced TypeORM",
      "content": "Learn how to use advanced features of TypeORM."
    }
  ]
}

```
- 3. Ajouter un nouvel utilisateur
     URL : POST http://localhost:3000/users

body à envoyer : 
```json
{
  "name": "Alice Cooper",
  "email": "alice@example.com"
}

```
     
