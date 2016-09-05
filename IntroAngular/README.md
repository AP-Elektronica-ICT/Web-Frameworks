# Angular

Angular is een client-side web framework, geschreven in Javascript. Het werkt samen met andere web technologieën (HTML, CSS en Javascript) om het ontwikkelen van web applicaties gemakkelijk en snel te doen verlopen.

## jQuery v.s. Angular

Het verschil tussen jQuery en Angular werd al deels aangehaald bij het verschil tussen jQuery en Angular in het vorige hoofdstuk. De belangrijkste functie van Javascript is het manipuleren van de DOM structuur. Dit is echter een erg complex en moeilijk proces voor complexe single page applicaties. Bij jQuery wordt dit op een *imperatieve* manier gedaan van buiten af. jQuery definieert manueel de transformaties die nodig zijn om de HTML structuur aan te passen. Angular daarentegen definieert op een *declaratieve* manier hoe de DOM structuur eruit moet zien. Het verschil tussen beide wordt in het volgende voorbeeld mooi geïllustreerd. In het voorbeeld moet de gebruiker eerst zijn naam ingeven, waarna hij/zij wordt begroet en landen kan ingeven.

jQuery code

```
<!DOCTYPE html>
<html>
  <head>
    <title>selector</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
  </head>

  <body>
    <script type="text/javascript">
      $(function(){
        $("#authContainer").show();
        $("#countryContainer").hide();

        $("#nameButton").click(function() {
          $("#nameHeader").text("Welkom terug, " + $("#nameInput").val());
          $("#authContainer").hide();
          $("#countryContainer").show();
        });

        $("#addButton").click(function() {
          $("#countries").html($("#countries").html() + "<li>"+ $("#country").val() + "</li>");
          $("#country").val("");
        });
      });
    </script>
    <div id="authContainer">
      <input type="text" id="nameInput" placeholder="Voer je naam in." /><button id="nameButton">Ok</button>
    </div>
    <div id="countryContainer">
      <h5 id="nameHeader">Welkom terug, </h5>
      <input type="text" id="country" placeholder="Voeg land toe" /><button id="addButton">Voeg toe</button>

      <ul id="countries"></ul>
    </div>
  </body>
</html>
```

Laten we kijken naar de problemen met deze code.

* De HTML staat vol met IDs waarvan het onbekend is wat deze doen (zonder naar de Javascript te kijken) en niet bijdragen aan de opmaak.
* De Javascript code is moeilijk leesbaar met alle operaties die nodig zijn bij een klik op een knop.
* Imperatief programmeren staat bekend als erg gevoelig te zijn een bugs en inconsistenties.
* De ontwikkelaar moet kennis hebben van de hele DOM structuur om efficiënt de DOM structuur te manipuleren.
* Applicatie code wordt gemengd met view specifieke code en DOM manipulaties, wat onoverzichtelijk is.
* Als je naar de `<ul>` onderaan kijkt weet je niet met informatie deze opgevuld zal worden. De DOM bevat dus niet alle info.
* Geen duidelijke afbakening tussen de staat van je applicatie en hoe deze staat wordt gevisualiseerd. De code staat door elkaar.
* Best wat code voor een eenvoudig voorbeeldje ...
* Noem er zelf nog enkele op.

Denk ook eens na over de volgende situaties:

* Hoe zou je het moeten aanpassen om de namen gesorteerd te tonen in jQuery qua DOM manipulaties?
* Wat gebeurt er als je opmaak wilt toevoegen op de lijst items van de landen? Waar moet je dit doen? Zie je hier problemen mee?
* Wat gebeurt er met je Javascript als een designer de klasse of ID bij een DOM element verandert? Of als er per ongeluk meerdere klasses/IDs dezelfde naam krijgen?
* Hoe ben je er zeker van dat je acties enkel invloed hebben op het componentje dat je aan het aanpassen bent? Denk aan HTML bestanden van duizenden lijnen.

De Angular code die hetzelfde doet staat hieronder.

```
<!DOCTYPE html>
<html>
  <head>
    <title>selector</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
  </head>

  <body ng-app="sampleApp">
    <script type="text/javascript">
      var sampleApp = angular.module('sampleApp', []);
      sampleApp.controller('MainController', function($scope) {
        $scope.nameEntered = false;
        $scope.enterName = function() {
          $scope.nameEntered = true;
        };

        $scope.countries = [];
        $scope.addCountry = function() {
          $scope.countries.push($scope.newCountry);
          $scope.newCountry = "";
        };
      });
    </script>

    <div ng-controller="MainController">
      <div ng-hide="nameEntered">
        <input type="text" placeholder="Voer je naam in." ng-model="name" /><button ng-click="enterName()">Ok</button>
      </div>
      <div ng-show="nameEntered">
        <h5>Welkom terug, {{ name }}</h5>
        <input type="text" placeholder="Voeg land toe" ng-model="newCountry" /><button ng-click="addCountry()">Voeg toe</button>
        <ul>
          <li ng-repeat="country in countries">{{ country }}</li>
        </ul>
      </div>
    </div>
  </body>
</html>
```

In deze code worden de voordelen van declaratief werken hopelijk duidelijk. Javascript focust zich hier op het bijhouden en modificeren van de staat via gewone Javascript objecten. HTML toont hoe deze staat moet worden getoond in de DOM structuur en Angular zorgt automatisch voor de DOM manipulaties die nodig zijn om de staat correct te visualiseren. Voeg enkele landen toe en zie eens naar de DOM inhoud van de lijst van landen. Dit ziet er als volgt uit.

```
<ul>
    <!-- ngRepeat: country in countries -->
    <li class="ng-binding ng-scope" ng-repeat="country in countries">Azerbeidjan</li>
    <!-- end ngRepeat: country in countries -->
    <li class="ng-binding ng-scope" ng-repeat="country in countries">Belgie</li>
    <!-- end ngRepeat: country in countries -->
    <li class="ng-binding ng-scope" ng-repeat="country in countries">Frankrijk</li>
    <!-- end ngRepeat: country in countries -->
</ul>
```

Je ziet dat Angular hier zelf extra `<li>` elementen heeft aangemaakt op basis van de staat door gebruik van de `ng-repeat` directive.

Naast deze voordelen, biedt Angular ook mogelijkheden aan om code heel modulair op te bouwen via `directives`. Meer hierover later.

## Angular.js in je project toevoegen

1. Als je angular.js wil inladen in een webpagina moet je the angular javascript bestand via de script tag (<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script> includen.

2. Daarna zet je de ng-app directive op een element. 

```html
<html>
  <head>
    <title>Sample</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
  </head>
  <body ng-app="sampleApp">
    ... Angular code hier
  </body>
</html>
```

De magie zit hier in de ng-app directive! De ng-app directive geeft aan dat alle child elementen van het element waarop het attribuut staat behoren tot het Angular project. Je kan Angular projecten dus introduceren in slechts een deel van de web pagina of combineren met andere frameworks.

Author: Tom Peeters & Kristof Overdulve
