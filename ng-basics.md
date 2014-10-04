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
  ##foo