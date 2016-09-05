# Angular data binding

In klassieke web frameworks (en ook b.v. PHP) of data binding systemen, worden view templates gemaakt met daarin expressies die worden vervangen door hun effectieve inhoud. In PHP doe je bijvoorbeeld het volgende om een naam in een header te tonen.

```html
<html>
  <body>
    <h1><?php echo $name; ?></h1>
  </body>
</html>
```

Deze binding is echter relatief statisch. Het systeem bouwt de HTML van de web pagina op op het moment dat de view wordt gerendert (als de pagina wordt geladen of na een refresh). Het is een one-way binding; van het model naar de view. Angular breidt zulke functionaliteit uit naar two way binding. Het bouwt een live template op dat expressies zal evalueren en vervangen door hun resultaat. Het verschil is echter dat Angular continu evalueert in zijn zogenaamde `$digest` loop of er variabelen van waarde zijn verandert en - zo ja - om alle expressies die deze waarde te bevatten te herevalueren en hun nieuwe resultaat mee te nemen.

Data binding vormt ook de basis voor de MVC (Model View Controller) architectuur van Angular. Het Model bevat de staat van de applicatie en wordt gebonden aan de View via data binding en Controllers. Dit wordt duidelijker met een eenvoudig voorbeeld.

## Voorbeeld

### We binden een text input aan een interpolatie

We voegen een ng-model attribuut toe en gebruik de "name" variabele in de expressie, zodat beiden automatisch gesynchroniseerd zijn.
Wanneer je in het input veld typt zullen deze veranderingen automatisch te zien zijn in het paragraaf element. Dit komt door de two way data binding.

```html
<html>
  <head>
    <title>Sample</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
  </head>
  <body ng-app>
    <h1>binding a text input to an expression</h1>
    Enter your name : <input type="text" ng-model="name"></input>
    <p> hello {{name}}</p>
  </body>
</html>
```

In dit voorbeeld bevat het model (dus de staat) uit 1 waarde: de naam van de gebruiker. Data binding wordt in dit voorbeeld synchroon gehouden via `ng-model="name"` en `{{name}}`. Dit eenvoudige voorbeeld bevat nog geen controller. We bekijken data binding in meer detail in de context van controllers in het volgende hoofdstuk.

*** TODO Iets over Angular expressies? ***

## Modules

De `ng-app` directive neemt optioneel een attribuut: de naam van de module die je definieert. Dit laat toe om verschillende modules te gebruiken onder verschillende namen, naam collisies te voorkomen, en je code op te splitsen over verschillende bestanden.

Het wordt daarom aangeraden een naam aan je module te geven. Het bovenstaande ziet er dan als volgt uit.

```
<html>
  <head>
    <title>Sample</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script type="text/javascript">
      angular.module('sampleApp', []);
    </script>
  </head>
  <body ng-app="sampleApp">
    <h1>binding a text input to an expression</h1>
    Enter your name : <input type="text" ng-model="name"></input>
    <p> hello {{name}}</p>
  </body>
</html>
```

De voornaamste wijzigingen zie je op twee plaatsen. Het definiëren van een module doe je als volgt:

```
angular.module('sampleApp', []);
```

Het effectief gebruiken van die module doe je dan op een HTML element als volgt.

```html
  <body ng-app="sampleApp">
```

De `angular.module()` functie heeft twee varianten. Ofwel geef je er zoals hierboven twee argumenten aan mee. met twee argumenten waarvan de eerste de naam van de module is en de tweede een lijst van dependencies (hier leeg). Dit is de setter variant van `angular.module`.

```
angular.module('sampleApp', []);
```

Je kan ook de functie oproepen met 1 argument


```
angular.module('sampleApp');
```

Hierbij vraag je de module op. Dit is de getter variant van `angular.module()`.

Author: Tom Peeters & Kristof Overdulve
