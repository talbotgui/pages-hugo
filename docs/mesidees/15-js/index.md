---
title: JS
prev: /mesidees/14-qualimetrie/
next: /mesidees/99-trucsalire/

weight: 115
---

Quelques notes sur les incontournables choses à connaître en JS.

Au sommaire :

* [Les 6 falsies](#les-6-falsies)
* [Les paramètres par défaut](#les-paramètres-par-défaut)
* [Les déclarations de variables](#les-déclarations-de-variables)
* [Déclaration des scripts dans une page HTML](#déclaration-des-scripts-dans-une-page-html)
* [Les exceptions en JS](#les-exceptions-en-js)
* [Déclarer une classe](#déclarer-une-classe)
* [Utiliser des getter / setter en JS](#utiliser-des-getter-setter-en-js)
* [Déclarer un module encapsulant du code](#déclarer-un-module-encapsulant-du-code)


#### Les 6 falsies

* false
* 0
* '' ou "" string de length 0
* NaN
* undefined
* null

Astuce : 

* *!!maVariable* permet de récupérer un booleen TRUE si la valeur de la variable n'est pas l'un des falsies
* *a && b* renvoie **a** s'il est un falsie et **b** sinon
* *a || b* renvoie **a** s'il n'est pas un falsie et **b** sinon

#### Les paramètres par défaut

function toto(a = 1, b = 2) { return a * b; }

#### Les déclarations de variables

* *const* permet la déclaration d'une variable non ré-affectable (le *final* de Java)
* *let* permet de déclarer une variable dans un scope précis (sans écraser une variable du même nom dans le scope parent)
* *var* est et reste la déclaration de variables de l'époque mais ne devrait plus être utilisé.

une belle explication [ici] (http://putaindecode.io/fr/articles/js/es2015/const-let-var/)

#### Déclaration des scripts dans une page HTML

Un JS ne doit pas être déclaré dans le head mais à la fin du body ([source 1](https://openclassrooms.com/courses/bonnes-pratiques-javascript), [source2](https://www.alsacreations.com/astuce/lire/916-librairie-javascript-jquery-script.html)).

Au pire, si le JS ne dépend d'aucun autre, il peut être chargé avec ASYNC. Mais attention aux dépendances entre scripts d'une même page ([source](https://www.alsacreations.com/astuce/lire/1562-script-attribut-async-defer.html)).

#### Les exceptions en JS
He oui, ça existe. Mais c'est pénalisant en performance. Donc à utiliser avec modération.
```js
try {
	throw {"name":"monException", "message":"momMessage"};
} catch (e) {
}
```

#### Déclarer une classe

solution 1 : la fonction
```js
function MaClasse(args){
	this.prop = args;
	this.maMethode = function() {
		alert(this.prop);
	};
}
var monInstance = new MaClasse(true, "bonjour");
monInstance.maMethode();
```

solution 2 : la description
```js
let monInstance =  {
	prop: args,
	maMethode: function() {
		alert(this.prop);
	}
}
```

solution 3 : le prototype
```js
function MaClasse(args){ ... }
MaClasse.prototype.prop = ...;
MaClasse.prototype.maMethode = function() { alert(this.prop); };

var monInstance = new MaClasse(true, "bonjour");
monInstance.maMethode();
```

solution 4 : l'héritage 
```js
// Type Rectangle avec une largeur et une longueur
function Rectangle(largeur, longueur) {  this.largeur = largeur; this.longueur = longueur; };
Rectangle.prototype.largeur = 0;
Rectangle.prototype.longueur = 0;
Rectangle.prototype.calculSurface = function() { return this.largeur * this.longueur; };
			
// Type Carre avec une largeur (la longueur et la largeur sont égales)
function Carre(largeur) { Rectangle.call(this, largeur, largeur); };
Carre.prototype = Object.create(Rectangle.prototype);
Carre.prototype.constructor = Carre;

// Test du Rectangle
let monRec = new Rectangle(3, 10);
let surfaceMonRec = monRec.calculSurface(); //30

// Test du Carre
let monCarre = new Carre(5);
let surfaceMonCarre = monCarre.calculSurface(); //25
```


#### Utiliser des getter / setter en JS
```js
let monInstance =  {
	get prop() {
		return ...;
	}
	
	set prop(value) {
		...
	}
}
```

Si le getter n'existe pas, la valeur retournée est *undefined*

#### Déclarer un module encapsulant du code
```js
var MonModule = ( function() {
	// privé
	const compteur = 18;
	function maMethodePrivee() {
		...
	}

	// public
	let module = {};
	module.methodePublique = function() {
		...
	}
	return module;
})();
```