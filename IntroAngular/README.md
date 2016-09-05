# Angular

Angular is een client-side web framework, geschreven in Javascript. Het werkt samen met andere web technologieën (HTML, CSS en Javascript) om het ontwikkelen van web applicaties gemakkelijk en snel te doen verlopen.

# jQuery v.s. Angular

TODO





## Include Angular.js library code

1. Als je angular.js wil inladen in een webpagina moet je the angular javascript file via de script tag (<script src="http://code.jquery.com/jquery.min.js"></script> includen.

2. Daarna maak je gebruik van de ng-app directive

De magie zit in de ng-app directive!

```html

<head>
		<title>ex1Angular</title>
<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.4/angular.js"></script>
	</head>


	
```


Vergelijk met jQuery:

```html

<head>
		<title>ex1</title>

<script src="http://code.jquery.com/jquery.min.js"></script>

	<script>
		$(function()
		{
			$("input").keypress(function()
			{
				$("#name").text($(this).val());
			});
		});
	</script>
	
	</head>

	<body>

Enter your name: <input type="text"></input>
<p id="name"></p>
	</body>
	

```

## Voorbeeld 1
### We binden een text input aan een interpolatie

We voegen een ng-model attribuut toe en gebruik de "name" variabele in de expressie, zodat beiden automatisch gesynchroniseerd zijn.
Wanneer je in het input veld typt zullen deze veranderingen automatisch te zien zijn in het paragraaf element. Dit wordt data binding genoemd.

```html

	<body ng-app>
<h1>binding a text input to an expression</h1>
Enter your name : <input type="text" ng-model="name"></input>
<p> hello {{name}}</p>
	</body>
	

```
