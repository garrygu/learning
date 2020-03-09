Angular 2 defines directives as components without views.  
"Directives" is an umbrella term used for component directives (also known as components), attribute directives, and structural directives.


# Core Directives
## Attribute directives
Attribute directives are HTML extensions that change the behavior of a component/element. These directives do not define their own view.   
For example: ngStyle, ngClass, ngValue, ngModel, ngSelectOptions, ngControl, ngFormControl, etc.

### NgStyle
- bind data on a per property basis  
```
<p [ngStyle]="{ 'color': myColor, 'font-weight': myFontWeight }">I am red and bold</p>
```

- or pass an object  
```
<p [ngStyle]="myCssConfig">I am red and bold</p>
```

- the expression parser also allows the use of the ternary operator ?:  
```
<div [ngStyle]= "{
'width':componentWidth,  
'height':componentHeight,  
'font-size': 'larger',  
'font-weight': ifRequired ? 'bold': 'normal' }"></div>
```

- use function  
```
<div [ngStyle]= "getStyles()"></div>
getStyles () {
    return {
      'width':componentWidth,
      ...
    }
}
```

### NgClass
```
<p [ng-class]="{{myClassNames}}">Hello Angular!</p>
```

Can use:
- a string with one or several classes delimited by a space
- an array
- an object in which each key corresponds to a CSS class name referred to by a Boolean value.
```
<div [ngClass]= "{'required':inputRequired, 'email':whenEmail}"></div>
```


## Structural directives
### NgIf
```
<p *ngIf="timeout === 0">Time up!</p>`  
```

* \*ngIf="conditional"   
Clone and inject pieces of templated HTML snippets in the markup, removing it from the DOM when the condition evaluates to false.

* [hidden]="conditional"  
Does not inject or remove any markup from the DOM. It simply sets the visibility of the already existing chunk of HTML annotated with that DOM attribute.

### NgFor
```
<ul>
  <li *ngFor="let employee of employees; let i = index; let last = last">
    Employee #{{i}}: - {{employee.name}}, {{employee.position}}
    <span *ngIf="last"><br />End of list</span>
  </li>
</ul>
```

### NgSwitch, NgSwitchCase, and NgSwitchDefault
```
<div [ngSwitch]="weatherForecastDay">
    <ng-template ngSwitchCase="today">{{weatherToday}}</ng-template>
    <ng-template ngSwitchCase="tomorrow">
      {{weatherTomorrow}}</ng-template>
    <ng-template ngSwitchDefault>
        Pick a day to see the weather forecast
    </ng-template>
</div>
```
