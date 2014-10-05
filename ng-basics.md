##Conditionally Displaying HTML Content with ng-if and ng-switch
Modern web apps require showing, hiding, adding, and removing components on a page, but HTML is not designed to do this. For many years, developers have used raw JavaScript or jQuery to listen for user input events and show and hide or entirely remove DOM content in response to events. Although Angular is a JavaScript framework, much of its functionality feels like it simply extends HTML with additional useful tags and elements.

Angular contains a variety of helpful built in directives to dynamically alter what shows up on the page. When combined with an event directive such as ng-click, magic happens. Together with ng-model, let's use a new built in directive, ng-if, to conditionally display a message on screen if the user typed in the value "5" into the search field.
```
<body>
  <input type="text" ng-model="myValue"/>
  <div ng-if="myValue=='5'" class="pass">
  	<p>You have entered 5</p>
  </div>
  <div ng-if="myValue!=='5'" class="fail">You haven't entered anything or entered something other than five.
  </div>
</body>
  ```
  An HTML element that has the ng-if directive set on it will display if the expression ng-if is set to evaluates to true. If it evaluates to false, that element will not display.

  ng-if is well suited for situations where you have two options (display one thing or another), but what if you have more than two cases? Angular's built in ng-switch directive is designed to handle this situation. If you've ever used a switch statement in JavaScript, this pattern should be familiar to you. Let's see an example:
  ```
  <body>
  <input type="text" ng-model="myValue"/>
  <div ng-switch="myValue">
  	<div ng-switch-when="5" class="pass">You have entered 5</div>
  	<div ng-switch-when="10" class="pass">You have entered 10</div>
  	<div ng-switch-default class="fail">You haven't entered anything or entered something other than 5 or 10</div>
  </div>
</body>
```

If you play with this example, you'll see that when you input 5 or 10 in the input, the expected response is displayed from the ng-switch block. If you put any other value (including putting no value at all), you'll see our default message.

We can use this approach to show and hide entire tabs based on user inputs. In the next example, we'll create a page where we have two links, one for numbers and one for form. When the user clicks on numbers, a div displaying the integers in our numbers array will be rendered. When the user clicks on form, a div with a form will display where the user can input a number to filter the numbers list by. To accomplish this, we could use either ng-if or ng-switch. Let's use ng-switch for now:
```
<body ng-init="numbers=[0,1,2,3,4,5]">
    	<nav>
			<a href="#" ng-click="tab='numbers'">Numbers</a>
			<a href="#" ng-click="tab='form'">Form</a>
		</nav>
		<div ng-switch="tab">
			<div ng-switch-when="numbers">
			  <p>Numbers interface will go here!</p>    
		  </div>
			<div ng-switch-when="form">
			   	<p>Form interface will go here!</p>
			</div>
		</div>
	</body>
```
##Dynamically Setting CSS Classes with ng-class
One popular feature in websites is to highlight the tab that is active. Typically this is done using a special CSS class that is applied to the element that is considered "active". In Angular, we can use the ng-class directive to accomplish this.
```
<body ng-init="numbers=[0,1,2,3,4,5]">
    	<nav>
        <a href="#" ng-click="tab='numbers'"  ng-class="{active:tab=='numbers'}">Numbers</a>
        <a href="#" ng-click="tab='form'" ng-class="{active:tab=='form'}" >Form</a>
		</nav>
		<div ng-switch="tab">
			<div ng-switch-when="numbers">
			  <p>Numbers interface will go here!</p>    
		  </div>
			<div ng-switch-when="form">
			   	<p>Form interface will go here!</p>
			</div>
		</div>
	</body>
	```
If you play with this example, you'll see that now when you click on the numbers link, that element will get a red background, and the same is true for the form link. With ng-class, we're able to dynamically set CSS classes.

Putting all together:
```
	<body ng-init="numbers=[0,1,2,3,4,5]">
    	<nav>
        <a href="#" ng-click="tab='numbers'"  ng-class="{active:tab=='numbers'}">Numbers</a>
        <a href="#" ng-click="tab='form'" ng-class="{active:tab=='form'}" >Form</a>
		</nav>
		<div ng-switch="tab">
			<div ng-switch-when="numbers">
				<input type="text" ng-model="myValue" /> 
			  	<h1 ng-repeat="number in numbers | filter:myValue">{{ number }}</h1>   
		  	</div>
			<div ng-switch-when="form">
			   	<button ng-click="numbers.pop(); $root.tab = 'numbers'">Pop</button>
    			<button ng-click="numbers.push(numbers.length); $root.tab = 'numbers'">Push</button>
			</div>
		</div>
	</body>
```
If you play with this example, you see that now if you click the push or pop buttons when you’re in the form tab, the app pushes or pops to the numbers list and switches view back to the numbers tab so you can see the result.

It’s important to note one peculiarity in this code that you would generally avoid in a real Angular app. For the buttons we have:
```
<div ng-switch-when="form">
    <button ng-click="numbers.pop(); $root.tab = 'numbers'">Pop</button>
    <button ng-click="numbers.push(numbers.length); $root.tab = 'numbers'">Push</button>
</div>
```
The appearance of $root.tab here on both buttons is the issue. Once we’re working with modules and controllers, we’ll be able to do something like ng-click="numbers.pop(); tab='numbers'" in order to refer to our numbers scope variable.

##How can we collect and store data from the user?
Let's revisit the form part of the interface we were working on before. When we left off, we were able to pop and push to the numbers array, but we couldn't enter a number and append it to the array. We'll use ng-model along with the ng-submit directive to implement this functionality.
```
<!DOCTYPE html>
<html ng-app=''>
	<head>
		<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
	</head>
	<body ng-init="numbers=[0,1,2,3,4,5]">
    	<nav>
        	<a href="#" ng-click="tab='numbers'"  ng-class="{active:tab=='numbers'}">Numbers</a>
       		<a href="#" ng-click="tab='form'" ng-class="{active:tab=='form'}" >Form</a>
		</nav>
		<div ng-switch="tab">
			<div ng-switch-when="numbers">
				<input type="text" ng-model="myValue" /> 
			  	<h1 ng-repeat="number in numbers | filter:myValue track by $index">{{ number }}</h1>  
		  	</div>
			<div ng-switch-when="form">
   				<form ng-submit="numbers.push(myNumber); $root.tab = 'numbers'">
					<input type="number" ng-model="myNumber" required />
					<input type="submit" />
  			    </form>
    			<button ng-click="numbers.pop();$root.tab = 'numbers'">Pop</button>
    			<button ng-click="numbers.push(numbers.length);$root.tab = 'numbers'">Push</button>
			</div>
		</div>
	</body>
</html>
```
If you play with this example, you'll see that now when you click the form tab, you can input a number. If you submit the number and then click back on the numbers tab, you can see that the number you just submitted has been appended to the DOM. Let's break down what's happening the in the form. That part of the code looks like this:

```
<div ng-switch-when="form">
    <form ng-submit="numbers.push(myNumber); ">
        <input type="number" ng-model="myNumber" required />
        <input type="submit" />
    </form>
    <button ng-click="numbers.pop();">Pop</button>
    <button ng-click="numbers.push(numbers.length);">Push</button>
</div>
```
There's a number of things going on in this code that warrant commenting. First, note that we're using HTML5 input element and setting the type equal to number (if you need a refresher on this, look here). Setting the input type to number prevents users from submitting non-numeric values in this field. AngularJS fully understands that the input is a number, and it will parse the data from this input as a number value when it assigns the value to myNumber on the scope. We're also using the HTML5 required attribute to ensure that users don't submit blank values. If you try submitting a blank value, you can see that the browser complains and asks you to supply a value.

This form also uses a built in directive we haven't yet encountered: ng-submit. You can see that we've used ng-model on the first input in the form to have it bind to myNumber. The ng-submit code causes the value we've put for myNumber to be appended to our numbers array. Note that using ng-submit causes Angular to intercept the form submission event, which would normally be directed to a server. Instead, Angular collects the data and handles it locally.

When you were playing with the example above, you may have noticed that when we submit a new number, although it does get pushed onto the numbers array, the value we have entered remains in the input field. The ideal behavior would be for the input to clear after the user has submitted data. We can achieve this by setting myNumber to null after it gets pushed into the numbers array. We can modify the ng-submit expression in the opening form tag so it looks like this 

```
<form ng-submit="numbers.push(myNumber); myNumber = null;>

<body ng-init="numbers=[0,1,2,3,4,5]">
    	<nav>
        	<a href="#" ng-click="tab='numbers'"  ng-class="{active:tab=='numbers'}">Numbers</a>
       		<a href="#" ng-click="tab='form'" ng-class="{active:tab=='form'}" >Form</a>
		</nav>
		<div ng-switch="tab">
			<div ng-switch-when="numbers">
				<input type="text" ng-model="myValue" /> 
			  	<div ng-repeat="number in numbers | filter:myValue track by $index">
			  		<h1>{{ number }}</h1>
				</div>   
		  	</div>
			<div ng-switch-when="form">
   				<form ng-submit="numbers.push(myNumber); myNumber = null; $root.tab='numbers'">
					<input type="number" ng-model="myNumber" required />
					<input type="submit" />
  			    </form>
    			<button ng-click="numbers.pop(); $root.tab='numbers'">Pop</button>
    			<button ng-click="numbers.push(numbers.length); $root.tab='numbers'">Push</button>
			</div>
		</div>
</body>

```
##cont.soon
