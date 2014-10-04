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