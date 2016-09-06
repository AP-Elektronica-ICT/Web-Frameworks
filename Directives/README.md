# Directives

HTML bestaat uit verschillende componenten. We hebben het HTML document dat bestaat uit HTML nodes die meestal zijn geformatteerd in HTML elementen. HTML elementen zijn XML elementen die bestaat uit een openingstag en een sluitende tag zoals b.v. `<p>Hallo</p>`. Sommige tags kunnen extra attributen krijgen om hun functionaliteit aan te passen. In b.v. `<a href="http://www.google.com">Ga naar Google</a>`. De anchor `<a>` tag introduceert een link tussen onze pagina en de pagina meegegeven in het `href` attribuut. HTML kan ook commentaar bevatten als volgt `<!-- Dit is commentaar -->`.

*** Probeer zelf eens ***

* Welke elementen/attributen ken je allemaal?
* Hoe zou je een date picker ontwikkelen? Is dit intuïtief? Herbruikbaar? Modulair?

Angular laat je als ontwikkelaar toe *Directives* te creëren die de DOM/HTML syntax uitbreiden. HTML bevat geen standaard ondersteuning voor date pickers. Wanneer een ontwikkelaar een date picker directive zou hebben ontwikkeld in Angular, kunnen we deze eenvoudigweg gebruiken b.v. als volgt

```html
<date-picker></date-picker>
```

of

```html
<input type="text" date-picker/>
```

Deze manier van werken laat toe om op een heel modulaire manier complexe web applicaties op te zetten. Angular 2.0 dat stilaan productieklaar begint te geraken zal nog veel meer belang hechten aan directives en component georienteerde web ontwikkeling dan Angular 1.X.X.

Je kan zelf directives aanmaken, maar Angular bevat er ook een heleboel ingebouwde. We hebben er al enkele gezien zoals ng-model, ng-repeat, ng-show,.. Het ng-repeat directive herhaalt een element en de ng-show toont een element afhankelijk van de conditionele logica.

## Verschil tussen jQuery en Angular mbt directives

In jQuery schrijf je een datepicker door een input field `<input>` te schrijven in HTML om vervolgens met jQuery: `$(element).datePicker()`. Deze code converteert het input field naar een datepicker.

Wanneer een designer je code ziet zal hij de code moeten vertalen om te weten dat je input field een datepicker is, en niet een gewoon tekstveld ... Bij Angular daarentegen blijkt duidelijk uit de HTML waar het element voor staat

```html
<date-picker></date-picker>
```

## Ingebouwde directives

We hebben al gebruik gemaakt van een aantal directives: `ng-click`, `ng-controller`, `ng-app`, enzovoorts. Angular bevat er echter nog een heleboel meer.

Angular maakt een heleboel standaard HTML tags 'Angular compatibel' door er `ng-`voor te zetten. Zo hebben we

* ng-href
* ng-src
* ng-disabled
* ng-checked
* ng-readonly
* ng-selected
* ng-class
* ng-style

Deze directives laten Angular expressies toe in tegenstelling tot hun overeenkomstige versies in de HTML standaard.

Daarnaast zijn er nog een heleboel directives voorzien. Je vindt een opsomming in de officiële documentatie hier https://docs.angularjs.org/api/ng/directive. Een voorbeeld van het gebruik van een built-in directive staat hieronder.

*** Probeer zelf eens ***

* Probeer al deze directives eens uit en zie wat ze doen. Gebruik hiervoor ook de Angular documentatie en het aanvullende materiaal bij deze cursus


### 1. Enabling/disabling DOM elements

Afhankelijk van de checkbox state wensen we de knop te disablen.
De ng-disabled directive staat in directe verbinding met de disabled HTML attribuut.
Het wordt gebonden aan het checked model door de attribuut waarde terwijl de checkbox een ng-model directive gebruikt/

```html
<body ng-app>
<label><input type="checkbox" ng-model="checked"/>Toggle button</label>
    <button ng-disabled="checked">Press me</button>
</body>
```

## Eigen directives

### Voorbeeld eigen directive

```html
<!DOCTYPE html>
<html>
  <head>
  <title>directive1</title>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
  <script>
  var app = angular.module("app", []);
  app.directive('helloWorld', function() {
    return {
      restrict : 'AE',
      replace : 'true',
      template : '<h3>Hello World!!</h3>'
    };
  });

  app.directive('myClock', function () {
    return {
      template: '<p>My clock! </p>'
    };
  });
  </script>
  </head>
  <body ng-app="app">
    <div data-my-clock></div>

    <hello-World></hello-World>
    <div hello-world></div>

    OR

    <div x-hello-world></div>
  </body>
</html>
```

Enkele zaken vallen op in het bovenstaande voorbeeld.

* De directives worden gedefinieerd in `camelCase`, maar gebruikt in html met dashes: `<camel-case>`of `<div camel-case>`.
* Je kan directives prefixen met `x-` of `data-`, al is dat niet aan te raden.
* `replace: true` is eigenlijk deprecated, dus het wordt aangeraden dat niet (te veel) te gebruiken.

*** Probeer zelf eens ***

* Open de browser eens met bovenstaande code en zoek de HTML elementen van bovenstaande directives. Kun je vanuit de HTML source code zien wat `replace: true` zou kunnen doen?
* Pas eens enkele stukken code aan en zie wat er gebeurt.
* Verander restrict `AE` naar `E`. Wat zie je? Wat zouden `A` en `E` betekenen?


We bekijken later de verschillende properties van directives in meer detail.

## Voorbeeld 2 - dynamische functionaliteit toekennen

Bovenstaande directives waren interessant om stukken code te encapsuleren, maar verder werd er niet echt iets dynamisch gedaan met de elementen. Directives kunnen ook extra functionaliteit krijgen via een eigen controller. Zo maken we nu van ons bovenstaande voorbeeld een echte dynamische klok.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>directive2</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script>
    var app = angular.module("app", []);
    app.directive('myClock', function () {
      return {
        template: "<p>{{ time | date:'HH:mm:ss' }}</p>",
        restrict: 'AE',
        replace: true,
        controller: function ($scope, $interval) {
          $scope.time = new Date();
          $interval(function () {
            $scope.time = new Date(); 
          }, 1000);
        }
      };
    });
    </script>
  </head>
  <body ng-app="app">
    <div data-my-clock></div>
  </body>
</html>
```

## Voorbeeld 3 - data meegeven met shared scope

*** Probeer zelf eens ***

* Bekijk het volgende stuk code eens en draai het in de browser. Wat zie je? Verwijder de `MainController` eens, wat zie je? Is het een goede manier om op deze manier data mee te geven?

```
<!DOCTYPE html>
<html>
  <head>
    <title>directive2</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script>
    var app = angular.module("app", []);
    app.controller('MainController', function($scope) {
      $scope.data = 4;
    });
    app.directive('myAccumulator', function () {
      return {
        template: "<p>{{ data + 1 }}</p>",
        restrict: 'AE'
      };
    });
    </script>
  </head>
  <body ng-app="app">
    <div ng-controller="MainController">
      <div my-accumulator></div>
    </div>
  </body>
</html>
```

## Voorbeeld 4 - data meegeven met een isolate scope

In voorbeeld 3 gaven we de directive toegang tot de shared scope van de controller. Dit is allemaal heel gemakkelijk als we de directive enkel willen gebruiken binnen die controllers. Zoals eerder aangegeven, gaat het bij directives echter om modulaire, herbruikbare code. Eisen dat de directive wordt omgeven door een bepaalde controller, is niet herbruikbaar of gebruiksvriendelijk. Daarom is het aangeraden voor directives met een *isolate scope* te werken. Het volgende voorbeeld maakt duidelijk wat hiermee bedoeld wordt. We passen daarom het bovenstaande voorbeeld aan om met een isolate scope te werken.

```
<!DOCTYPE html>
<html>
  <head>
    <title>directive2</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script>
    var app = angular.module("app", []);
    app.directive('myAccumulator', function () {
      return {
        scope: {
          myAccumulatorStartValue: '@myAccumulator',
          accumulateWithValue: '@'
        },
        template: "<p>{{ myAccumulatorStartValue + accumulateWithValue }}</p>",
        restrict: 'AE',
        controller: function ($scope, $interval) {
          if (!$scope.accumulateWithValue) {
            $scope.accumulateWithValue = 1;
          }
        }
      };
    });
    </script>
  </head>
  <body ng-app="app">
    <div my-accumulator="4" accumulate-with-value="4"></div>
    <div my-accumulator="4"></div>
    <div my-accumulator-start-value="4" accumulate-with-value="4"></div>
  </body>
</html>
```

In deze code zien we enkele extra's

* `scope:`: geeft aan welke variabelen in de isolate scope ter beschikking zijn.
* myAccumulatorStartValue: '@myAccumulator': zegt dat het attribuut met als naam `myAccumulator` (de naam van de directive) of in HTML `my-accumulator` binnen de controller van de directive beschikbaar is onder de naam `$scope.myAccumulatorStartValue`.
* `accumulateWithValue: '@'` zegt dat het attribuut met als naam `accumulateWithValue` binnen de controller van de directive beschikbaar is onder de naam `$scope.accumulateWithValue`. Dit is eigenlijk een korte variant van `accumulateWithValue: @accumulateWithValue`. Je zegt dat de attribuut intern dezelfde naam krijgt als waarmee hij wordt opgeroepen.
* We initialiseren in de controller het attribuut `accumulateWithValue` als de gebruiker geen waarde meegeeft met 1 als default.
* `@` is de binding strategy. In de volgende paragraaf wordt dit meer besproken.


## Directives uitgelegd

de `directive` functie neemt 2 argumenten:

```
// Definitie
angular.module('app').directive('myDirective', function ($timeout, SomeService) {
  // Definitie directive
  return {
    // directive wordt gedefinieerd via opties die je hier override
  };
});


... html
<my-directive></my-directive>
```

* de naam van de directive, hier `myDirective`. Deze is in `camelCase` gedefinieerd in Javascript code en wordt met dashes gebruikt in HTML. Hier `my-directive`.
* een functie met daarin de definitie van de directive. Zoals bij o.a. controllers en services kun je hier via dependency injection argumenten aan meegeven die je nodig hebt binnen de definitie.

Directives nemen verschillende opties die in aparte paragrafen worden besproken. Enkel de opties die gangbaar worden gebruikt worden besproken. Voor een volledige lijst, zie dan naar de Angular documentatie.

### restrict: string

Dit is een optioneel argument dat aan Angular aangeeft in welk formaat directives kunnen worden gespecificeerd: Als HTML Element (E) `<my-directive></my-directive>`, als Attribuut (A) `<div my-directive></div>`, als Klasse (C) `<div class="my-directive: expression;"></div>` en als commentaar (M) `<!-- directive: my-directive expression -->`. Een combinatie van bovenstaande is ook mogelijk, b.v. `EAC`.

Default waarde is `EA`.

### template: string|function

Optioneel argument dat een string of een functie neemt als parameter. Een string met HTML code of een functie met twee parameters: tElement en tAttrs en dat de template teruggeeft. De template moet 1 root element hebben, dus niet b.v. `template: `<p>El1</p><p>El2</p>`. Angular nest de waarde die je hierin weergeeft onder het element waarin de directive wordt opgeroepen. Het zorgt er dus voor dat de directive in HTML zichtbaar wordt.

Wanneer je directives hebt die niet in de view moeten opgenomen worden, b.v. validatie of logging naar de server, dan kan je de template weglaten. Deze directive toont b.v. niets in de DOM tree zelf.

```
angular.module('app').directive('myDirective', function () {
  // Definitie directive
  return {
    controller: function () {
      // Directive toont deze tekst bij het inladen, toont verder niets
      console.log("I'm a jalapeno on a stick.");
    }
  };
});


... html
<my-directive></my-directive>
```

# templateUrl: string | function

Alternatief voor bovenstaande attribuut dat een pad naar een HTML bestand neemt. Handig als de template uit meerdere lijnen code bestaat.

# replace: boolean

Optioneel attribuut dat - als het op true wordt gezet - aangeeft dat de template van de directive het element waarin het opgeroepen wordt moet vervangen en dat het er dus niet onder mag genest worden. 

Het volgende stuk code

```
var app = angular.module("app", []);
angular.module('app').directive('myDirective', function () {
  // Definitie directive
  return {
    template: '<h1> I am nested</h1>',
    replace: false
  };
});

... html
<my-directive></my-directive>
```

rendert naar

```
<my-directive>
  <h1>I am nested</h1>
</my-directive
```

terwijl

```
var app = angular.module("app", []);
angular.module('app').directive('myDirective', function () {
  // Definitie directive
  return {
    template: '<h1> I am replaced</h1>',
    replace: true
  };
});

... html
<my-directive></my-directive>
```

rendert naar

```
<h1>I am replaced</h1>
```

Default waarde is false.

### scope: boolean | object

Scope kan een boolean ontvangen om aan te geven of het een nieuwe scope is of niet, maar geen isolate scope (niet aan te raden) of een object met daarin de geïsoleerde variabelen. Om herbruikbaarheid te stimuleren focusen we ons enkel op de optie waarbij een object wordt meegegeven.

Een isolate scope betekent dat code buiten de directive niet aan de scope binnen de directive kan en de directive zelf niet aan code buiten de directive kan. Data komt enkel binnen en buiten de directive - al dan niet via double binding - via de opgegeven attributen.

Isolate Scope variabelen kunnen op verschillende manieren worden gebind: Via `@`, `=` of `&`.

`@` laat toe om strings mee te geven aan de directive. het ontvangt een Angular expressie, evalueert deze en toont deze. B.v. 

```
scope: {
  name: '@'
}
```

kan worden gebruikt als volgt

```
<directive-name name="Kristof"></directive-name>
<directive-name name="{{ aControllerVariable }}"></directive-name>
```

`=` wordt gebruikt voor bidirectionele binding. Een directive dat via bi-directional binding werkt is `ng-model`. Met dit attribuut kan je enkel object referenties meegeven, dus geen `{{ }}` expressies of gewone tekst.

```
scope: {
  ngModel: '='
}
```
kan worden gebruikt als volgt

```
<input ng-model="name"></input>
````

`&` laat toe een functie mee te geven als attribuut. Een voorbeeld dat we al geregeld gebruikt hebben is `ng-click`.

```
scope: {
  ngClick: '&'
}
```

kan worden gebruikt als volgt

```
<button ng-click="sendMail()">Send mail</button>
```

### controller: string | function

Indien een string wordt meegegeven, zoekt Angular de controller met de opgegeven naam op en gebruikt het deze om functionaliteit toe te kennen. Je kan ook een functie meegeven om functionaliteit toe te kennen aan je directive. Deze functie wordt opgeroepen wanneer de directive wordt geladen.

*** Probeer zelf eens ***

* Maak een directive dat experimenteert met bovenstaande scope variabelen en de verschillende opties.
* Probeer alle bovenstaande opties zelf uit.
