# Filters

Filters zijn een manier in Angular om de data die we tonen aan de gebruiker te formatteren. Angular bevat een heleboel built-in filters om data te formatteren in upper case, als datum, als decimaal getal, als valuta, enzovoorts. Je kan ook zelf filters opstellen.

## Enkele voorbeelden van built-in filters

Deze filters zitten ingebouwd in Angular.

```html
<html>
  <head>
    <title>Sample</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script>
      angular.module('sampleApp', []);
    </script>
  </head>
  <body ng-app="sampleApp">
    <div class="currencies">
      <input type="text" ng-model="amount" placeholder="Enter amount"/>
      <p>Default : {{ amount | currency }}</p>
      <p> Euro : {{ amount | currency:"Euro" }}</p>
      <p> Number : {{ amount | number:5 }}</p>
    </div>

    <p>{{ today | date: 'short'}}</p>
    <p>{{ 'test'| uppercase }}</p>
    <p> {{ [1, 2, 3, 4, 5] | limitTo: 3 }}</p>
  </body>
</html>
```

Zoek zelf eens op welke filters Angular nog allemaal bevat.

## Schrijven van een eigen filter

Onze filter zal de tekst omgekeerd weergeven. Angular's filter
functie verwacht een filter naam en een functie als parameter. Deze functie moet 
de filterfunctie teruggeven alwaar je de logica zal programmeren.

```html

var app = angular.module("MyApp",[]);
app.filter("reverse",function()
{
        return function(input)
        {
                var result = "";
                input = input || "";
                for(var i=0; i<input.length;i++)
                {
                        result = input.charAt(i) + result;
                }
                return result;
        };
});
	
</script>
</head>

<body ng-app = "MyApp">
        <input type="text" ng-model="text" placeholder="Enter text"/>
        <p>input : {{ text }}</p>
        <p> Filtered Input : {{ text | reverse }}</p>
</body>
```

## Schrijven van een eigen filter met opties

Aan een Angular filter kan je parameters meegeven. Deze parameters worden als een hash doorgegeven en kunnen 
onmiddellijk worden verwerkt in de filter functie.
De suffix ! wordt doorgegeven als een optie aan de filter functie en wordt toegevoegd aan de output

```html

<script>
	
	var app = angular.module("MyApp",[]);
	
	app.filter("reverse",function()
	{
		return function(input,options)
		{
			var result = "";
			input = input || "";
			var suffix = options["suffix"] || "";
			for(var i=0; i<input.length;i++)
			{
				result = input.charAt(i) + result;
			}
			
			if(input.length > 0)
				result += suffix;
			return result;
		};
	});
	
</script>
	</head>

	<body ng-app = "MyApp">
		
		<input type="text" ng-model="text" placeholder="Enter text"/>
		
		<p>input : {{ text }}</p>
		
		<p> Filtered Input : {{ text | reverse: { suffix: "!"} }}</p>

	</body>
	

```

## Filters met arrays

De filter gaat door alle namen en maakt een nieuwe array aan zonder één bepaald element.

```html
<script>
	
	var app = angular.module("MyApp",[]);
	
	app.filter("exclude",function()
	{
		return function(input,name)
		{
			var result =[];
			
			for(var i=0; i<input.length;i++)
			{
				if(input[i] != name)
					result.push(input[i]);
			}
			
			return result;
		};
	});
	
</script>
	</head>

	<body ng-app = "MyApp">
		<ul ng-init="names = ['Tom','Arno','Hannes','Mieke']">
			<li ng-repeat="name in names | exclude: 'Mieke' | orderBy:'-name' ">
				{{name}}
			</li>
		</ul>
	</body>
	
```

*** Probeer zelf eens ***

Schrijf een filter die een namenlijst doorloopt en elke eerste letter in hoofdletter zet.

## Validators (voor de geïnteresseerden)

Filters zijn gebouwd om van interne data naar een mooie presentatie (view) te gaan. Omgekeerd kunnen we ook data van de UI valideren vooraleer ze intern op te slaan via validators. Angular bevat een aantal ingebouwde validators die je kan gebruiken via directives. Zelf validators schrijven of foutmeldingen geven als de validatie faalde valt buiten de context van deze cursus. het volgende voorbeeld geeft aan welke ingebouwde validators Angular bevat.

```html
<html>
  <head>
    <title>Sample</title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.8/angular-messages.js"></script>
    <script>
      angular.module('sampleApp', [ 'ngMessages' ]);
    </script>
    <style>
      input {
        display: block;
        margin: 10px;
      }

      input.ng-invalid {
        border: 1px solid red;
      }
      input.ng-valid {
        border: 1px solid green;
      }

      .error {
        font-color: red;
      }
    </style>
  </head>
  <body ng-app="sampleApp">
    <form name="myForm" novalidate>
      <input type="text" placeholder="Text required" required name="field1" ng-model="field1" />
      <div ng-messages="myForm.field1.$error"  class="error">
        <div ng-message="required">is required</div>
      </div>

      <input type="text" placeholder="Minimum 4 characters" ng-minlength="4" name="field2" ng-model="field2" />
      <div ng-messages="myForm.field2.$error" class="error">
        <div ng-message="required">cannot be less than 4 characters</div>
      </div>

      <input type="text" placeholder="Maximum 6 characters" ng-maxlength="6" required name="field3" ng-model="field3" />
      <div ng-messages="myForm.field3.$error" class="error">
        <div ng-message="required">is required</div>
        <div ng-message="maxlength">cannot be more than 6 characters</div>
      </div>

      <input type="text" placeholder="Only a-z, A-Z, 0-9" ng-pattern="/[a-zA-Z0-9]/" name="field4" ng-model="field4" />
      <div ng-messages="myForm.field4.$error" class="error">
        <div ng-message="pattern">Does not match pattern</div>
      </div>

      <input type="number" placeholder="number" ng-model="number" />
    </form>
  </body>
</html>
```

Merk op dat we voor deze functionaliteit niet alleen angular gebruiken, maar ook een extra plugin `angular-messages`. We moeten ook bij het definiëren van de module `ngMessages` als dependency definiëren. Pas dan voegt Angular de benodigde functionaliteit toe om dit mogelijk te maken.
