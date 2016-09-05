# Web Frameworks

Binnen web frameworks gaan we bestuderen hoe we gebruik kunnen maken van web frameworks om complexe web applicaties te ontwikkelen.

We gebruiken in deze cursus liever de term web applicaties in plaats van web sites omdat hedendaagse web applicaties (Web 2.0) of web site zoals Facebook, Google, LinkedIn en dergelijke erg complexe grootschalige projecten zijn waaraan honderden ontwikkelaars aan bijdragen. Deze projecten zijn rijk aan interactieve functionaliteit en bestaan uit meer dan een tiental HTML bestanden met wat opmaak dat als enige doel heeft om statische informatie te tonen.

In grote lijnen zullen we een web applicatie ontwikkelen dat op de zogenaamde MEAN stack draait. MEAN bestaat uit

* M(ongo): een NoSQL database
* E(xpress): een server-side web framework om REST endpoints ter beschikking te stellen
* A(ngular): een client-side web framework om het ontwikkelen van complexe web applicaties gemakkelijk en snel te doen verlopen
* N(ode.js): de server-side infrastructuur dat Javascript op de V8 engine draait om Javascript backends te ontwikkelen

## Wat is een framework?

Er is vaak verwarring over het verschil tussen een *framework* en een *library*. Een handige richtlijn is dat je bij het gebruik van libraries de libraries oproept. Jij hebt dus controle waar en hoe het gebruikt wordt. Een framework daarentegen roept jouw code op. Het tegenovergestelde dus. Laten we dit verschil kort illustreren met jQuery en Angular. 

In jQuery zou je het aantal clicks op een knop bijhouden als volgt:

```
<!DOCTYPE html>
<html>
  <head>
    <title>selector</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
    <script>
      $(function(){
        var noClicks = 0;
        $("#count").text(noClicks);

        $("button").click(function(){
          $("#count").text(++noClicks);
        });
      });
    </script>
  </head>

  <body>
    <button>Button</button>
    <div id="count"></div>
  </body>
</html>
```

in Angular ziet dit er als volgt uit

```
<!DOCTYPE html>
<html>
  <head>
    <title>selector</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
  </head>

  <body ng-app="sampleApp">
    <script>
      var sampleApp = angular.module('sampleApp', []);
      sampleApp.controller('MainController', function($scope) {
        $scope.count = 0;
        $scope.increaseCount = function() {
          $scope.count ++;
        };
      });
    </script>

    <div ng-controller="MainController">
      <button ng-click="increaseCount()">Button</button>
      <div>{{ count }}</div>
    </div>
  </body>
</html>
```

Bij het eerste voorbeeld is het duidelijk dat je de functionaliteit van jQuery zelf aanroept en zelf dirigeert wat er gebeurt. Bij Angular daarentegen definieer je de functies (hier o.a. `increaseCount`) en variabelen (hier o.a. `count`). die kunnen opgeroepen worden door het framework en laat je aan Angular weten hoe je code 'heet' (hier `MainController`) en welke variabelen en welke functies moeten opgeroepen/aangepast worden bij bepaalde acties zoals b.v. een klik op de knop. Vervolgens handelt het framework al de rest zelf af. Het roept de functie die meegegeven wordt met `ng-click` op wanneer er op de knop wordt geklikt en het herevalueert automatisch alle variabelen die mogelijk kunnen aangepast zijn. In dit voorbeeld is de variabele `$scope.count` aangepast en Angular zorgt ervoor dat de nieuwe waarde wordt getoond na een button klik zonder dat je hiervoor zelf de view hebt moeten aanpassen.

Het verschil wordt nog eens geïllustreerd in onderstaande afbeelding.

![framework v.s. library](frameworkvslibrary.png)

## Wat is een framework?

Frameworks hebben verschillende voordelen en nadelen. Enkele voordelen van frameworks zijn de volgende:

* Frameworks hebben het potentieel om veel meer werk uit de handen van de ontwikkelaar te halen. Libraries vereenvoudigen bepaalde taken maar vereisen nog altijd veel meer manueel werk. Frameworks kunnen potentieel meer automatiseren.
* Frameworks leggen een bepaalde architectuur op. Dit zorgt ervoor dat je zelf niet de hele applicatie-architectuur zelf moeten definiëren.
* Nieuwe ontwikkelaars die het framework kennen zijn sneller productief. De applicaties die geschreven zijn in een bepaald framework lijken vaak sterk op elkaar en de hele infrastructuur zal sneller vertrouwd aanvoelen bij nieuwe ontwikkelaars. Dit zorgt ervoor dat ze sneller productief zijn in het project.

Natuurlijk zijn er ook enkele nadelen verbonden aan frameworks. Enkele belangrijke zijn de volgende

* Frameworks werken fantastisch voor zover je de filosofie van het framework volgt. Als je hier om bepaalde redenen van wilt afwijken, zal het vaak gebeuren dat het soms moeilijk is om hierrond te werken.
* Frameworks beperken het gebruik van externe libraries. Hun invloed is meestal zo groot dat je specifieke tools moet zoeken die samenwerken met het framework. Een jQuery date picker zal bijvoorbeeld vaak niet out of the box samenwerken met Angular. Dan moet je die functionaliteit manueel porten of het er in 'hacken', wat zelden proper is. Dit nadeel is beperkt als je met een erg populair framework werkt (zoek maar op 'Angular date picker') omdat de belangrijkste tools meestal wel voorhanden zijn daarvoor. Maar als je op een minder bekend framework werkt, zal je het probleem zeker tegen komen.
* Omdat het effect van frameworks op je architectuur zo groot is, is het vaak moeilijk zo niet onmogelijk om te veranderen van framework zonder de hele applicatie opnieuw te schrijven. Zeker in web development waar er elke week the next best framework opduikt, kan het een belangrijke invloed hebben.

Het is belangrijk een goede balans te vinden, kritisch te zijn tegenover nieuwe web frameworks en om bij de keuze van web frameworks te zien in hoeverre de opgelegde architectuur niet over-engineered is en je dus snel beperkingen zal tegenkomen.
