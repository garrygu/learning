> Angular >= 2.x doesn’t come with a (built-in) two-way data binding anymore  
> There’s one directive in Angular >= 2.x that implements two-way data binding: `ngModel`  
> It turns out that two-way data binding really just boils down to **event binding** and **property binding**  


## [Two-way data binding example](https://blog.thoughtram.io/angular/2016/10/13/two-way-data-binding-in-angular-2.html)
```
<input [value]="username" (input)="username = $event.target.value">

<p>Hello {{username}}!</p>
```
- **[value]=”username”** - Binds the expression username to the input element’s value property  
- **(input)=”expression”** - Is a declarative way of binding an expression to the input element’s input event
- **username = $event.target.value** - The expression that gets executed when the input event is fired  
- **$event** - Is an expression exposed in event bindings by Angular, which has the value of the event’s payload (It’s an `InputEventObject`, which comes with a target property, which is a reference to the DOM object that fired the event (our input element))  


## Understanding `ngModel`
```
<input [ngModel]="username" (ngModelChange)="username = $event">

<p>Hello {{username}}!</p>
```
The shorthand syntax using [()], also called “Banana in a box”:  
 `[(ngModel)]="username"`  
 
- The property binding `[ngModel]` takes care of updating the underlying input DOM element
- The event binding `(ngModelChange)` notifies the outside world when there was a change in the DOM.
- `ngModelChange` takes care of extracting `target.value` from the inner `$event` payload, and simply emits that (to be technically correct, it’s actually the [DefaultValueAccessor](https://angular.io/docs/ts/latest/api/forms/index/DefaultValueAccessor-directive.html) that takes of the extracting that value and also writing to the underlying DOM object).


## Creating custom two-way data bindings
