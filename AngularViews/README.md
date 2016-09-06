# Multiple views and routing

Angular is een Single page web application framework. Voor web applicaties met enigszins complexere user interactie zal er echter snel nood zijn aan meerdere schermen. Het is echter onhandig om alles in `ng-if` of `ng-show` directives te zetten om na een klik op een knop alle schermen te hiden behalve één. Niet alleen dat, maar je gebruikers zullen bij het springen van het ene naar het andere scherm verwachten dat ze terug kunnen met een back knop. Niets zo vervelend dan wanneer je op stap 8-9 van een formulier op back te drukken en alle informatie kwijt te zijn.

Om deze reden bevat Angular een extensie: angular-route. Het volgende voorbeeld maakt het wat duidelijker

## Voorbeeld

```
<!DOCTYPE html>
<html>
  <head>
    <title>directive2</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular-route.js"></script>
    <script>
    angular.module("app", [ 'ngRoute' ]).config(function ($routeProvider) {
      $routeProvider
        .when('/', {
          template: '<div><h1>Page 1</h1><button ng-click="goToPage2()">Go to page 2</button></div>',
          controller: function ($scope, $location) {
            $scope.goToPage2 = function() {
              $location.path('/page2');
            };
          }
        })
        .when('/page2', {
          template: '<div><h1>Page 2</h1></div>',
          controller: function ($scope) {
            // Zet het hier op de scope
          }
        })
        .otherwise({
          redirectTo: '/'
        })
    });
    </script>
  </head>
  <body ng-app="app">
    <div ng-view></div>
  </body>
</html>
```

In dit voorbeeld vallen enkele zaken op. 

* We hebben een nieuw script moeten includen omdat Angular by default geen ondersteuning geeft voor routes.
```
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular-route.js"></script>
```

* Vervolgens moeten we onze module laten dependen op `ngRoute` om effectief van de routering te kunnen gebruik maken.

```
angular.module("app", [ 'ngRoute' ])
```

* Dan configureren we onze module om de routes als het ware te definiëren. Elke route krijgt een template, wat de HTML inhoud van de route is en een controller die specificeert wat de root controller moet zijn van onze route.

* Op pagina 1 springen we naar pagina 2 via `$location.path('/page2');` Het argument dat je aan `$location.path()` meegeeft moet overeenkomen met 1 van de route definities. Het geheel blijft een single page app gezien je angular enkel de relatieve URL aanpast en de onderliggende views swapt.

* Ergens in de body zet je de directive `ng-view` om aan te geven dat de `templates` van die views hier mogen gerendert worden.

*** Probeer zelf ***

* Kijk eens naar de URL. Wat verandert er als we van route 1 naar route 2 springen? Kunnen we terug gaan? Gedraagt dit zich zoals je zou verwachten?
* Het bovenstaande voorbeeld is - alhoewel het heel eenvoudig is - wat spaghetti code. De routes kunnen ook een `templateUrl` ontvangen in plaats van een `template` en de controller kan een string nemen, b.v. 'MyController'waarna Angular opzoekt of er ergens een controller met de naam `MyController` is gedefinieerd geweest. Dit laat toe om je Javascript en HTML bestanden op te splitsen met een beter onderhoudbare codebase als gevolg.

