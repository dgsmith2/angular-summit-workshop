### Starting Up a New Project
1. New Project
    
    ```bash
    ng new workshop
    ```

2. Change directories
    
    ```
    cd workshop
    ```

3. Start the server
    
    ```bash
    ng serve
    ```

4. Open a browser to http://localhost:4200


---

### Adding Routes
1. Open another Terminal in the `workshop` directory
2. New "Home" Route
    
    ```bash
    ng g r home
    ```

3. Add Navigation in `workshop.component.html`
    
    ```html
    <a [routerLink]="['/home']">Home</a>
    ```

4. Add Two More Routes
    
    ```html
    ng g r team
    ng g r detail
    ```
    
5. Add an `id` param to the detail path in `workshop.component.ts`
    
    ```js
    {path: '/detail/:id', component: DetailComponent}
    ```
     
6. Add Navigation to Those Routes
    
    ```html
    <a [routerLink]="['/team']">Team</a>
    <a [routerLink]="['/detail', 1]">Detail</a>
    ``` 


---


### Sharing Services
1. Generate a new service
    
    ``` bash
    ng g s shared/team
    ```

2. Add a `name` property to the service
    
    ```TypeScript
    import { Injectable } from '@angular/core';
    
    @Injectable()
    export class TeamService {
      name = "Bulls";
      
      constructor() {}
      
    }
    ```

2. Import and Provide the service to `workshop.component.ts`
    
    ```TypeScript
    import { TeamService } from './shared'
    
    providers: [ROUTER_PROVIDERS, TeamService]
    ```

3. Import and Inject the service into `home.component.ts`
    
    ```TypeScript
    import { TeamService } from '../shared';
    
    constructor(public team:TeamService) {}
    ```

4. Access a Property on the Service in `home.component.html`
    
    ```html
    <p>
      {{team.name}}
    </p>
    ```

5. Import and Inject the Service into `team.component.ts`
    
    ```js
    import { TeamService } from '../shared';
    
    constructor(public team:TeamService) {}
    ```

6. Update the Team `name` on `team.component.html`
    
    ```html
    <input type="text" [(ngModel)]="team.name">
    ```

---

### Creating a Component
1. Generate a new Component
    
    ```bash
    ng g c shared/card
    ```
    

2. Import the Component and add to `team.component.ts`
    
    ```TypeScript
    import { TeamService, Card } from '../shared';
    ```

3. Add the `CardComponent` to the `team.component.ts` 's `directives:[]`
    
    ```TypeScript
      directives:[CardComponent],
    ```


3. Add the component to the `team.component.html` template
    
    ```html
    <app-card></app-card>
    ```


4. Add style to the component with `:host`
    
    ```css
    :host{
      display: flex;
      border: 2px solid black;
    }
    ```

5. Move the input inside the card component in `team.component.html`
    
    ```html
    <app-card>
      <input type="text" [(ngModel)]="team.name">
    </app-card>
    ```


6. Handle content with `<ng-content>` in `card.component.html`
    
    ```html
    <ng-content></ng-content>
    ```

---

### Looping Through Data
1. Add an array of people to the `team.service.ts` 

    ```TypeScript
    import { Injectable } from '@angular/core';
    
    @Injectable()
    export class TeamService {
      name = "Bulls";
    
      people = [
        {name: 'Susan', id: Math.random()},
        {name: 'Anne', id: Math.random()},
        {name: 'John', id: Math.random()},
        {name: 'Mindy', id: Math.random()},
        {name: 'Ben', id: Math.random()},
        {name: 'Jordan', id: Math.random()},
        {name: 'Mike', id: Math.random()}
      ];
    
      constructor() {}
    
    }
    ```

2. Create Multiple Components using `*ngFor` on `team.component.html`.

    ```html
    <app-card *ngFor="let person of team.people">
      <input type="text" [(ngModel)]="person.name">
    </app-card>
    ```

3. Add the `ROUTER_DIRECTIVES` to `team.component.ts`

    ```TypeScript
    import {ROUTER_DIRECTIVES} from '@angular/router';
    
    
    directives:[CardComponent, ROUTER_DIRECTIVES],
    ```


4. Add a `[routerLink]` to each component and link to the `person.id`

    ```html
    <app-card *ngFor="let person of team.people">
      <input type="text" [(ngModel)]="person.name">
      <a [routerLink]="['/detail', person.id]">Details</a>
    </app-card>
    ```

5. Add the `OnActivate` LifeCycle hook to `detail.component.ts`

    ```TypeScript
    import { Component, OnInit } from '@angular/core';
    import {OnActivate, RouteSegment} from '@angular/router';
    import {TeamService} from '../shared';
    
    @Component({
      moduleId: module.id,
      selector: 'app-detail',
      templateUrl: 'detail.component.html',
      styleUrls: ['detail.component.css']
    })
    export class DetailComponent implements OnInit, OnActivate {
      routerOnActivate(curr:RouteSegment):void {
    
      }
    
      constructor(public team:TeamService) {}
    
      ngOnInit() {
      }
    
    }
    
    ```

6. Find the person by looking up the `id` in the `people`

    ```TypeScript
      person;
    
      routerOnActivate(curr:RouteSegment):void {
        const id = curr.getParam('id');
    
        this.person = this.team.people.find(person =>
          person.id === +id
        );
      }
    ```

7. Render out the `person` in `detail.component.html`

    ```html
    <h2>{{person.name}}</h2>
    <h3>{{person.id}}</h3>
    ```




