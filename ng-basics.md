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

##foo