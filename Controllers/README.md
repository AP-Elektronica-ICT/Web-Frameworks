## 1 Controllers

Controllers worden gebruikt om functionaliteit toe te voegen aan de scope van een view dat verder gaat dan pure double binding. We kunnen het ook gebruiken om de initiële staat van variabelen te zetten.

We definiëren een controller als volgt

```js
var app = angular.module('sampleApp', []);

app.controller('MyController', function($scope) {
  // ... functionaliteit hier definiëren
});
```

en gebruiken dit dan in de DOM structuur als volgt

```
  <body ng-app="sampleApp">
    ...
    <div ng-controller="MyController">
      ... functionaliteit controller hier gebruiken
    </div>
    ...
  </body>
```

De `<module variabele>.controller()` functie neemt in zijn basis vorm twee argumenten. Het eerste argument is de naam dat de controller heeft en dat in de DOM tree wordt meegegeven als waarde aan de `ng-controller` directive. Het tweede argument is een functie dat wordt opgeroepen wanneer het DOM element wordt gecreëerd in de HTML. Het is conventie om je controllers te noemen als `<naam>Controller`.

Deze functie heeft hier één argument met als naam `$scope`. Angular werkt via *Dependency Injection* om het `$scope` object mee te geven zodat je het kan gebruiken binnen de functie.

*** Probeer zelf: ***

* Verwijder het `$scope` argument een keer en zie wat er gebeurt. Zet in de controller functie ook eens `$scope.name = "Test"`. Wat merk je nu?
* Geef als parameters `$timeout, $scope`mee aan de functie en zie wat er gebeurt.

Je kan ook een array meegeven als tweede argument van de `<module variabele>.controller` functie. Het bovenstaande voorbeeld zou er dan als volgt uitzien.

```js
var app = angular.module('sampleApp', []);

app.controller('MyController', [ '$scope', function($scope) {
  // ... functionaliteit hier definiëren
}]);
```

Wanneer je van plan bent je Javascript bestanden te optimaliseren minification strategieën, moet je de controller met arrays instantieren. De `$scope` variabele wordt bij minification namelijk hernoemd naar b.v. `a` en zo weet Angular nog steeds dat `a` eigenlijk overeen komt met `$scope`.

## Voorbeelden

### Voorbeeld 1

In dit voorbeeld gebruiken de ng-show directive samen met de controller om de zichtbaarheid te veranderen na een button click.

We gebruiken de ng-controller directive om ons div element samen met de child elementen te binden aan de context van `MyController`.
De ng-click directive roept de toggle functie op die geïmplementeerd is in de controller. De `ng-show` directive is gebonden aan de visible scope variabele.

```html
<head>
  ...
  <script>
    var app = angular.module('sampleApp', []);
    app.controller('MyController', function($scope) {
      $scope.visible = true;

      $scope.toggle = function() {
        $scope.visible = !$scope.visible;
      }
    }
  </script>
</head>

<body ng-app="sampleApp">
  <div ng-controller="myctrl">
    <button ng-click="toggle()">Toggle</button>
    <p ng-show="visible">Hallo</p>
  </div>
</body>

```

In ons voorbeeld is het `visible` attribuut het model. De controller wordt gebruikt om variabelen op de scope te zetten zodat ze kunnen gebruikt worden in de view (de HTML code).

### 2 Veranderen van het model door middel van de controller

We gaan bijvoorbeeld het 'value' model met 1 incrementeren door gebruik te maken
van de controller.
In voorbeeld3 wordt de ng-init directive gebruikt wanneer de pagina geladen wordt. Deze roept de increment functie , aangemaakt in de controller, aan.

```html
<script>
  var app = angular.module('sampleApp', []);
  app.controller('MyController', function($scope) {
    $scope.value = 1;
    
    $scope.increment = function(inc) {
      $scope.value += inc;
    };
  });
</script>
</head>

<body ng-app="sampleApp">
  <div ng-controller="myctrl">
    <p>{{value}}</p>
    <button ng-click="increment(3)">Increment</button>
  </div>
</body>

```

### 3 Encapsulatie een model door middel van een controller's functie

(voorbeeld 4) In plaats van rechtstreeks het model aan te spreken (via scope service), gaan we het model encapsuleren binnen een functie van de controller.


```html
<script>
  var app = angular.module('sampleApp', []);
  app.controller('MyController', function($scope) {
    $scope.value = 10;
    
    $scope.getValue = function()
    {
            return $scope.value;
    };
  });
</script>
</head>

<body ng-app="sampleApp">
  <div ng-controller="myctrl">
          <p>{{getValue()}}</p>
  </div>
</body>
```

### 4 Reageren op scope veranderingen

Als het model verandert wil je een actie triggeren. Hiervoor maak je gebruik van de watch functie in je controller.
Het eerste argument van de $watch functie is een angular expressie. Het tweede argument wordt opgeroepen waneer de expressie evaluatie een andere waarde terugkrijgt.

```html
<script>
  var app = angular.module('sampleApp', []);
  app.controller('MyController', function($scope) {
    $scope.name = "";

    $scope.$watch("name", function(newVal,oldVal) {
      if($scope.name.length>3) {
        $scope.greeting = "greetings " + $scope.name;
      }
    });
  }
</script>
  </head>

  <body ng-app="sampleApp">
    <div ng-controller="myctrl">
      <input type="text" ng-model="name"/>
      <p>{{greeting}}</p>
    </div>
  </body>
```

## Scopes

Scopes zijn een core concept in Angular applicaties die overal worden gebruikt. Scopes bevatten de data (het model) van de applicatie en de core business functionaliteit. Scopes kunnen veranderingen `watchen`. Scopes zijn in Angular (net zoals een DOM structuur) hiërarchisch opgebouwd via prototypical inheritance. Prototypical inheritance is de manier waarop Javascript via functies (closures eigenlijk) overerving toepast. Wanneer we een controller toevoegen, wordt er een nieuwe scope gecreëerd dat aan alle variabelen kan van parent scopes (parent in de zin van de DOM structuur). Zie volgend voorbeeld.

```
<html>
  <head>
    <title>Sample</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script type="text/javascript">
      var sampleApp = angular.module('sampleApp', []);
      sampleApp.controller('FirstController', function($scope) {
        $scope.firstVariable = "FirstVariable";
      });
      sampleApp.controller('NestedController', function($scope) {
        $scope.nestedVariable = "NestedVariable";
      });
    </script>
  </head>
  <body ng-app="sampleApp">
    <div style="border: 1px solid blue;" ng-controller="FirstController">
      <h1>{{ firstVariable }}</h1>
      <div style="border: 1px solid green;" ng-controller="NestedController">
        <h2>{{ firstVariable }}</h2>
        <h2>{{ nestedVariable }}</h2>
      </div>
    </div>
  </body>
</html>
```

Als we dit draaien in de browser, zien we dat de `NestedController` overerft van `FirstController` (en alle controllers daarboven) en dus aan zijn variabelen kan. Scopes erven over volgens de structuur van de DOM. Als we een Angular debugging tool gebruiken (e.g. Chrome Batarang)

*** Probeer zelf: ***

* Zet de `<h2>{{ nestedVariable }}</h2>` expressie ook eens in de scope van de `FirstController`. Wat zie je?
* Verander `NestedController` eens naar `FirstController` en andersom. Wat zien we?
* Zet in de `NestedController`volgende expressie: `$scope.firstVariable = "FirstVariableNowNested". Kan dit? Wat zien we? Hoe komt dit?

## Data binding tips

Javascript heeft twee manieren om data mee te geven: by value en by reference. Primitieve objecten worden toegekend by value en objecten by reference. In een eenvoudig project met 2 scopes gaf dit al problemen. Daarom is het in Angular best practice om attributen van de scope op een object te zetten en niet rechtstreeks op de scope. Het voorgaande voorbeeld zou er dan als volgt uit zien.

```html
<html>
  <head>
    <title>Sample</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script type="text/javascript">
      var sampleApp = angular.module('sampleApp', []);
      sampleApp.controller('FirstController', function($scope) {
        $scope.first = {
          variable: "FirstVariable"
        };
      });
      sampleApp.controller('NestedController', function($scope) {
        $scope.nested = {
          variable: "NestedVariable"
        };
      });
    </script>
  </head>
  <body ng-app="sampleApp">
    <div style="border: 1px solid blue;" ng-controller="FirstController">
      <h1>{{ first.variable }}</h1>
      <div style="border: 1px solid green;" ng-controller="NestedController">
        <h2>{{ first.variable }}</h2>
        <h2>{{ nested.variable }}</h2>
      </div>
    </div>
  </body>
</html>
```

Kortom wordt gezegd dat er altijd een punt moet staan in angular expressies. Dus wel `{{ person.name }}` en niet `{{ nameOfPerson }}`.

*** Probeer zelf ***

## Controller best practices

We hebben hierboven goede lightweight controllers staan. Als we echter complexere operaties moeten uitvoeren zoals Xhr requests of complexe business logica, zijn controllers niet meer de plek om alle logica in te zetten. We kunnen zulke logica dan beter in `services` of `directives` plaatsen. Houd de controllers zo licht mogelijk.

* Verifieer dat het laatste experiment in de vorige paragraaf nu wel werkt.

Author: Tom Peeters & Kristof Overdulve
