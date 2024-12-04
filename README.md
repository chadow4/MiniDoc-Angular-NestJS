# MiniDoc-Angular-NestJS

## Qu'est-ce que NestJS ?

<div align="center">
  <img src="https://github.com/user-attachments/assets/f06933ec-03bd-4005-b878-846cd81c4d65" alt="Description de l'image" width="50%">
</div>



NestJS est un framework Node.js pour construire des applications côté serveur robustes, évolutives et bien structurées. Il utilise TypeScript par défaut et suit une architecture inspirée de MVC (Modèle-Vue-Contrôleur) combinée avec les concepts modernes comme les modules et les décorateurs.

##  Qu'est-ce qu'Angular ?
<div align="center">
  <img src="https://github.com/user-attachments/assets/83770971-fb7d-4b7d-9437-5fd6a13833dc" alt="Description de l'image" width="50%">
</div>



Angular est un framework de développement côté client utilisé pour créer des applications web interactives. Écrit en TypeScript, il suit une architecture basée sur les composants et est parfait pour intégrer avec une API comme NestJS.

## Autres ressources: 

Documentation officielle d'Angular : https://angular.dev et https://angular.fr <br>
Documentation officielle de NestJS : https://nestjs.com et https://nestjs.fr

# Pré-requis

Avant de commencer, assurez-vous de compléter les étapes suivantes :

## 1. Installer Node.js
<div align="center">
  <img src="https://github.com/user-attachments/assets/61b60c73-0008-432a-a00d-e6223c7b39ff" alt="Description de l'image" width="35%">
</div>



- Téléchargez et installez **Node.js** (version LTS recommandée) depuis le site officiel : [Node.js](https://nodejs.org).
- Vérifiez l'installation en exécutant les commandes suivantes dans un terminal :

```bash
node -v
npm -v
```

## 2. Installer les CLI nécessaires
- Installez **NestJS CLI** pour créer des projets backend :

```bash
npm install -g @nestjs/cli
```
puis pour lancer 
```bash
nest start
```

- Installez **Angular CLI** pour créer des projets frontend :

```bash
npm install -g @angular/cli
```
puis pour lancer 
```
ng serve
```

## 3. Configurer une base de données MySQL

<div align="center">
  <img src="https://github.com/user-attachments/assets/6d36a794-7316-49f8-81cf-26ec4e4331be" alt="Description de l'image" width="50%">
</div>


1. Téléchargez et installez **MySQL** sur votre machine.
2. Créez une base de données nommée `projectdb`.

## 4. Installer un IDE

<div align="center">
  <img src="https://github.com/user-attachments/assets/6da5bda9-258c-4cc6-a24e-9db4cf7084ad" alt="Description de l'image" width="50%">
</div>

- Utilisez un IDE comme **IntelliJ IDEA**, **WebStorm**, ou **Visual Studio Code** :
  - Ajoutez le plugin Angular pour le frontend.
  - Ajoutez le plugin NestJS pour le backend.
  - *(Note : IntelliJ IDEA et WebStorm intègrent ces plugins par défaut).*


## 4. Concepts de Base


### Backend : NestJS

<div align="center">
  <img src="https://miro.medium.com/v2/resize:fit:640/0*KikwdypTj1FVSpB2.png" alt="Description de l'image" width="50%">
</div>

   - Module :
        - Un module dans NestJS regroupe des fonctionnalités spécifiques (exemple : UserModule gère tout ce qui concerne les utilisateurs).
        - C'est une unité logique qui contient des services, contrôleurs et entités.

   - Service :
        - Un service est utilisé pour encapsuler la logique métier (exemple : accès à la base de données, calculs, etc.).
        - Les services sont injectables dans d'autres parties de l'application.

   - Contrôleur :
        - Un contrôleur gère les requêtes entrantes et retourne une réponse. Il agit comme un pont entre l'utilisateur et le service.

   - Entité :
      - Une entité représente un modèle de données (exemple : User, Article).
      - Utilisée avec TypeORM pour gérer la base de données.


### Frontend : Angular

<div align="center">
  <img src="https://v2.angular.io/resources/images/devguide/architecture/overview2.png" alt="Description de l'image" width="50%">
</div>


   -  Composant :
        Un composant est une unité visuelle de l'interface utilisateur.
        Chaque composant a une logique (.ts), un modèle (.html), et un style (.css ou .scss).

   -  Service :
        Un service dans Angular est similaire à NestJS. Il est utilisé pour encapsuler la logique métier, comme les appels HTTP.

   - Template :
        Les templates Angular définissent ce qui est affiché dans le navigateur.
        Avec Angular 17, des nouvelles syntaxes comme @for et @if simplifient le rendu conditionnel.


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

  @ManyToOne(() => User, (user) => user.articles)
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
- 1. Récupérer tous les utilisateurs
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
     
## 5. Etape 2 : Initialisation du Front-end 

### Créer le projet Angular 
```
ng new frontend
cd frontend
```

Dans une architecture typique Angular, le dossier src est organisé de la manière suivante :

   - /services : Contient les services Angular pour gérer la logique métier et les appels aux APIs.
   - /interceptors : Regroupe les intercepteurs HTTP, utilisés pour modifier ou intercepter les requêtes et réponses HTTP.
   - /guards : Contient les gardes de route, utilisés pour gérer les permissions d'accès aux différentes routes de l'application.
   - /pages : Inclut les composants spécifiques à chaque page de l'application (par exemple, les pages utilisateurs ou tableaux de bord).
   - /models : Définit les interfaces et classes modèles utilisées pour structurer les données dans l'application (DTOs).

![image](https://github.com/user-attachments/assets/471800d2-98ac-47f3-83a0-071ee0909dfb)


### Création des DTO : 

Un DTO (Data Transfer Object) est une interface ou une classe utilisée pour définir la structure des données échangées entre le frontend et le backend. Cela permet de :

   - Clarifier la structure des données : Vous savez exactement quelles propriétés sont disponibles.

 - Faciliter l'autocomplétion : Avec TypeScript, l'IDE propose les propriétés des objets.

  - Réduire les erreurs : Le compilateur vérifie les types pour vous.
    
 - Rendre le code plus lisible : Grosso modo c'est plus clean entre le back et le front.
     
Pour ce mini projet, nous avons 4 DTOs principaux :

   - UserDto: Représente un utilisateur
   - ArticleDto : Représente un Article
   - UserDtoWithArticles : Représente un utilisateur avec ses articles.
   - UserCreateDto : Représente les données nécessaires pour créer un utilisateur

Fichier : ```Fichier : /models/example.dto.ts```

```ts
sexport interface ArticleDto {
  id: number;
  title: string;
  content: string;
}

export interface UserDto {
  id: number;
  name: string;
  email: string;
}

export interface UserDtoWithArticles {
  id: number;
  name: string;
  email: string;
  articles: ArticleDto[];
}

export interface UserCreateDto {
  name: string;
  email: string;
}

```


### Création du Service Angular

Le service Angular gère les appels API et utilise les DTOs pour définir les données échangées avec le backend.

Fichier : ```/services/users.service.ts```
```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class UserService {
  private baseUrl = 'http://localhost:3000/users';

  constructor(private http: HttpClient) {}

  getUsers(): Observable<UserDto[]> {
    return this.http.get<UserDto[]>(this.baseUrl);
  }

  getUserById(id: number): Observable<UserDtWithArticles> {
    return this.http.get<UserDtoWithArticles>(`${this.baseUrl}/${id}`);
  }

  createUser(user: UserCreateDto): Observable<void> {
    return this.http.post<void>(this.baseUrl, user);
  }
}

```

### Composants Angular

#### Composant Liste des Utilisateurs + Création 

Explications :

   - Injection de Service :
    Dans Angular, les services sont injectés dans le constructeur du composant. Ici, nous injectons UserService avec ```private userService: UserService```. Cela nous permet d'accéder aux méthodes du service pour récupérer ou envoyer des données.

<div align="center">
  <img src="https://v2.angular.io/resources/images/devguide/architecture/injector-injects.png" alt="Description de l'image" width="50%">
</div>

   
   - Utilisation des Observables :
   
   Les méthodes du service, comme getUsers() ou createUser(), retournent des Observable. Nous devons souscrire (subscribe) à ces observables pour déclencher leur exécution et récupérer les données.
   
   - Méthodologie :
        Lors de l'initialisation du composant (ngOnInit), on appelle loadUsers pour charger les utilisateurs.
        Lorsqu'un utilisateur est ajouté via addUser(), la liste est rechargée après que l'ajout soit terminé.


Fichier : ```/pages/user/user.component.ts```

```ts
import { Component, OnInit } from '@angular/core';
import { UserService } from '../user.service';
import { UserDto, UserCreateDto } from '../dto/user.dto';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
})
export class UserComponent implements OnInit {
  users: UserDto[] = []; 
  newUser: UserCreateDto = { name: '', email: '' }; 

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.loadUsers();
  }

  loadUsers() {
    this.userService.getUsers().subscribe((data) => {
      this.users = data;
    });
  }

  addUser() {
    this.userService.createUser(this.newUser).subscribe(() => {
      this.loadUsers(); // Recharger la liste
      this.newUser = { name: '', email: '' }; 
    });
  }
}
```



Fichier : ```/pages/user/user.component.html```

Le routerLink est une directive Angular utilisée pour configurer une navigation interne dans une application Angular. Elle permet de générer des liens qui redirigent vers des routes définies dans le fichier de configuration des routes.
Dans cet exemple, chaque utilisateur affiché dans la liste aura un lien cliquable. Ce lien redirigera vers une page affichant les détails de l'utilisateur sélectionné.

```html
<h2>Liste des utilisateurs</h2>
<ul>
  @for (user of users; track user.id) {
    <li>
      <a [routerLink]="['/users', user.id]">
        <strong>{{ user.name }}</strong> - {{ user.email }}
      </a>
    </li>
  }
  @empty {
    <li>Aucun utilisateur trouvé.</li>
  }
</ul>

<h3>Ajouter un utilisateur</h3>
<form (ngSubmit)="addUser()">
  <label for="name">Nom :</label>
  <input id="name" [(ngModel)]="newUser.name" name="name" required />

  <label for="email">Email :</label>
  <input id="email" [(ngModel)]="newUser.email" name="email" required />

  <button type="submit">Ajouter</button>
</form>


```

### Composant : Détails d'un utilisateur

Explications :

   - Utilisation d'ActivatedRoute :
    Angular fournit le service ActivatedRoute pour accéder aux paramètres de l'URL. Ici, nous utilisons this.route.snapshot.params['id'] pour récupérer l'id de l'utilisateur depuis l'URL.
   -  Observable :
    La méthode getUserById du service retourne un observable. Nous nous abonnons (subscribe) pour obtenir les données de l'utilisateur.

Fichier : ```/pages/user-detail/user-detail.component.ts```

```ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { UserService } from '../user.service';
import { UserDto } from '../dto/user.dto';

@Component({
  selector: 'app-user-detail',
  templateUrl: './user-detail.component.html',
})
export class UserDetailComponent implements OnInit {
  user: UserDtoWithArticles | null = null;

constructor(
    private route: ActivatedRoute,
    private userService: UserService
  ) {}

  ngOnInit() {
    const userId = this.route.snapshot.params['id'];
    this.loadUser(userId);
  }

  loadUser(id: number) {
    this.userService.getUserById(id).subscribe((data) => {
      this.user = data;
    });
  }
}

```

Fichier : ```/pages/user-detail/user-detail.component.html```

```html
@if (user) {
  <h2>Détails de l'utilisateur</h2>
  <p><strong>Nom :</strong> {{ user.name }}</p>
  <p><strong>Email :</strong> {{ user.email }}</p>

  <h3>Articles écrits</h3>
  <ul>
    @for (let article of user.articles; track article.id) {
      <li>
        <h4>{{ article.title }}</h4>
        <p>{{ article.content }}</p>
      </li>
    }
    @empty {
      <li>Aucun article trouvé</li>
    }
  </ul>
} @else {
  <p>Chargement...</p>
}

```

### Configuration des routes :

Explications :

   -  Routage :
    Le tableau routes contient la définition des routes. Pour chaque route, nous spécifions :
        - path : le chemin de l'URL.
        - component : le composant à afficher lorsque cette route est active.
    - Redirections :
        - La route par défaut (path: '') redirige vers /users.
        - Une route "wildcard" (path: '**') redirige toutes les routes inconnues vers /users.


Fichier : ```/src/app.routes.ts```
```ts
const routes: Routes = [
  { path: '', redirectTo: '/users', pathMatch: 'full' },
  { path: 'users', component: UserComponent },
  { path: 'users/:id', component: UserDetailComponent},
  { path: '**', redirectTo: '/users' },
];
```
## Petit tips
Avec **JetBrains** (IntelliJ IDEA, WebStorm), vous pouvez faire un clic droit sur un dossier et générer des Angular Schématics (composants, services ou autres fichiers nécessaires automatiquement). Cela simplifie et accélère le développement !
![image](https://github.com/user-attachments/assets/ef9cb90c-5f35-4f8e-a2b4-89ac2448d080)
![image](https://github.com/user-attachments/assets/13787be5-f4d9-4cc9-b37d-2ae713fb2ce3)



