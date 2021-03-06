---
title: Angular
weight: 116
---

#### Quelques astuces Angular à retenir :

**[Angular Cheat Sheet](https://angular.io/guide/cheatsheet)**

Au besoin, pour réinstaller Angular/cli : 
```npm install -g @angular/cli --no-optional```

Dans une interpolation, pour naviguer sans risque, il suffit d'utiliser le Safe Navigation Operator ('?.').
Exemple : ```unObjet?.unAttributPotentiellementUndefined```

Two way binding :

 * ```[ngModel]="monAttribut"``` si la valeur change dans le JS, le DOM est mis à jour
 * ```(ngModel)="monAttribut"``` si le DOM change, la valeur du JS est mise à jour

Exemple de binding : ```<div [style.background-color]="getStyle()">```

Évènement : 

 * déclencher du code : ``` (click)="auClick()"```
 * récupérer l'event : ```(click)="auClick($event)"```

Dirty checking :

 * tout changement est pris en compte dans le composant : ```@Component {..., changeDetection: ChangeDetectionStrategy.Default, ...}``` => 
 * seul les @Input sont raffraichis : ```@Component {..., changeDetection: ChangeDetectionStrategy.OnPush,  ...}```

Pipes :

 * date, currency, decimal, uppercase, lowercase, slice (slice:start:end), i18nSelect, i18nPlural, percent, json, async
 * exemple : ```{{dateNaissance | date:"dd/MM/yy"}}```
 * exemple : ```<li *ngFor="let i of collection | slice:1:3">{{i}}</li> ```
 * exemple : ```{{monAttribut | async"}}``` pour se lier à un Observable et être notifié
 * exemple : ```{{note.valeur | i18nSelect: libellesNote}}``` avec libellesNote un Objet {'clef1':'valeur1','clef2':'valeur2'} et non une Map<string, string>

Constante :

 * à déclarer dans ```json providers: [..., {provide: 'maConstante', useValue:'azertyuiop'}, ...]```
 * à utiliser dans ```json constructor(..., @Inject('maConstante') public maConstante, ...)```

Interfaces implémentables dans un composant : OnInit, AfterContentInit

Exemple de directive : pour logger le clic sur tous un ensemble de boutons

 * ```<button [track]="unBouton">coucou</button>```
 * ```@Directive({selector: '[track]'}) export class TrackDirective {
  @Input() track;
  @HostListener('click')
  onClick(){ console.log(this.track); }
}```

Pour faire des checkbox dans un *ngFor : ```<div *ngFor="let aze of azes">
  <input [id]="aze", name="monChamp" ngModel [value]="aze" type="radio"></input>
  <label [attr.for]="aze">{{aze}}</label>
</div>```

Ajouter remplacer ngModel par ```[ngModel]="azes[0]"``` pour définir une valeur par défaut
 
Créer une variable locale à la page HTML (sans lien avec un attribut du composant) : ```#toto="NgForm"``` ou ```#toto="NgModel"```

Validation de formulaire :

 * il faut un form avec les attributs ```novalidate``` et ```#toto="ngForm"```
 * l'attribut ```required``` sur les champs obligatoires
 * une div avec le message d'erreur (pristine = inchangé) : ```<div [hidden]="inputNom.valid || inputNom.pristine" class="alert alert-danger">Le nom est obligatoire</div>```

Pour la validation des radios, c'est l'input qui est invalide et le label est à coté. Astuce CSS : ```css .ng-invalid + label:after { content:'sélectionne en un'; }```

Faire une boucle *ngFor et avoir l'index : ```*ngFor="let aze of azes; let i=index"```

Afficher/masquer un élément :

 * ```*ngIf``` supprime l'élément du DOM
 * ```[hidden]``` change la valeur de l'attribut display (none/block)

#### Pour tester une application Angular et ses composants :

Dans les tests, si l'erreur *Uncaught NetworkError: Failed to execute 'send' on 'XMLHttpRequest': Failed to load 'ng:///DynamicTestModule/ComposantNoteComponent.ngfactory.js'. thrown* survient, c'est un problème de données/objets non fournis en entrée d'un composant. Par exemple : un composant appel un mock dans le onInit mais le mock a été 'reset' et ne renvoie rien.

Ne jamais oublier de créer l'instance après avoir créer le mock avec Mockito

Quelques liens :

 * [cheat sheet pour Jasmine](http://blog.bandzarewicz.com/blog/2012/03/08/jasmine-cheat-sheet/)
 * [débugger des tests Karma depuis VsCode](http://blog.mlewandowski.com/Debugging-Karma-tests-with-VSCode.html)
 * [Mockito](https://github.com/NagRock/ts-mockito)

Quelques commandes :

 * Pour lancer les tests : ```ng test```
 * Pour lancer les tests avec couverture de code : ```ng test --code-coverage --reporters=coverage-istanbul```
 * Pour lancer les tests avec PhantomJS : ```ng test --config=karma-ic.conf.js```
 
Pour tester des XPath ou des sélecteurs CSS dans Chrome, il suffit d'utiliser $x('//div') ou $$('div.maClass') ([source](http://yizeng.me/2014/03/23/evaluate-and-validate-xpath-css-selectors-in-chrome-developer-tools/)).

#### Intégrer des frameworks supplémentaires :
* [Pour AwesomeFont] (https://github.com/angular/angular-cli/blob/master/docs/documentation/stories/include-font-awesome.md)

#### Quelques lectures utiles :
* [modularité avec TS] (https://stackoverflow.com/questions/30357634/how-do-i-use-namespaces-with-typescript-external-modules)
* [pour trouver un type] (http://microsoft.github.io/TypeSearch/)

#### Quelques commandes NPM utiles :
* pour lister les versions d'un package : ```npm show monPackage@* version```
* pour lister les packages qui ne sont pas en dernière version : ```npm outdated```