With the exception of the `script` tag, almost any piece of HTML can act as a template for the Angular binding infrastructure.

## Binding Constructs
### Interpolations
interpolation symbols: `{{ }}`  
Interpolations in fact are a special case of property binding, which allows us to bind HTML element/custom component properties to a model. We can consider interpolation as syntactical sugar over property binding.   

This is how Angular interprets an interpolation: `<h3>Main heading - {{heading}}</h3>`==> `<h3 [text-content]="' Main heading - '+heading"></h3>` Angular translates the interpolation in the first statement into the `textContent` property binding (second statement).  

Angular did not render the interpolation as HTML; instead, it escaped the HTML characters.
Use Property binding for HTML.

### Property binding
#### **Property versus Attribute**  
- The primary role of an element attribute is to initialize the state of the element when the corresponding DOM object is created.  

- Attribute and property synchronization is not consistent across properties. As we saw in the preceding example, changes to the value property on input do not affect the value attribute, but this is not true for all property-value pairs. The `src` property of an image element is a prime example of this; changes to property or attribute value are always kept in sync.  

- It's surprising to learn that the mapping between attributes and properties is also not one-to-one. There are a number of properties that do not have any backing attribute (such as `innerHTML`), and there are also attributes that do not have a corresponding property defined on the DOM (such as `colspan`).

- Attribute and property mapping too adds to this confusion as it does not follow a consistent pattern. An excellent example of this is available in the Angular 2 developer's guide, which we are going to reproduce here verbatim:  
>The disabled attribute is another peculiar example. A button's disabled property is false by default so the button is enabled. When we add the disabled attribute, its presence alone initializes the button's disabled property to true so the button is disabled. Adding and removing the disabled attribute disables and enables the button. The value of the attribute is irrelevant which is why we cannot enable a button by writing `<button disabled="false">Still Disabled</button>`.

#### **Property Binding General Syntax**  
`[target]="sourceExpression"; `  
The property appearing inside `[]` is a target, sometimes called binding target. The target is the consumer of the data and always refers to a property on the component/element. The source expression constitutes the data source that provides data to the target.

For example:  
`<img class="img-responsive" [src]="'/static/images/' + currentExercise.exercise.image" />
`  

Property binding, event binding, and attribute binding do not use the interpolation symbol. The following is invalid: `[src]="{{'/static/images/' + currentExercise.exercise.image}}"`

We can enable property binding using the `bind-` syntax, which is a canonical form of property binding. This implies that: `[src]="'/static/images/' + currentExercise.exercise.image"` Is equivalent to the following: `bind-src="'/static/images/' + currentExercise.exercise.image"`


#### **Guidelines for binding an expression to a property target**  
- **Quick expression evaluation**  
 Angular's change detection system will evaluate your expression binding multiple times during the life cycle of the application, while your component is alive. Hence it becomes imperative that the expressions we use evaluate quickly.

- **Side-effect-free binding expressions**  
Bad example:    
`<div [innerHTML]="getContent()"></div>`

  ```
  getContent() {
    var content=buildContent();
    this.timesContentRequested +=1;
    return content;
  }
  ```


### Attribute binding
The only reason attribute binding exists in Angular is that there are HTML attributes that do not have a backing DOM property. The `colspan` and `aria` attributes are some good examples of attributes without backing properties.

To support attribute binding, Angular uses a prefix notation, `attr`, within `[]`. An attribute binding looks like this:  
`[attr.attribute-name]="expression" `  

For example:  
`<div ... [attr.aria-valuenow]="exerciseRunningDuration" [attr.aria-valuemax]="currentExercise.duration" ...> `


### Class binding
Syntax:  
`[class.class-name]="expression"`   
This adds `class-name` when `expression` is `true` and removes it when it is `false`  

For example:  
`<div [class.highlight]="isPreferred">Jim</div> // Toggles the highlight class`


### Style binding
Use style bindings to set inline styles based on the component state:  
`[style.style-name]="expression";`  

For example:  
`[style.width.%]="(exerciseRunningDuration/currentExercise.duration) * 100" `

`style.` and `class.` are convenient bindings for setting a single class or style. For more flexibility, there are corresponding attribute directives: `ngClass` and `ngStyle`.


### Event binding
Examples:
```
<div id="pause-overlay" (keyup)= "onKeyPressed($event)">
<div id="pause-overlay" (window:keyup)= "onKeyPressed($event)">
```

Notes:
- Like properties, there is a canonical form for events too. Instead of using round brackets, the `on-` prefix can be used: `on-click="pauseResumeToggle()"`

#### Event bubbling
Notes:  
- Event bubbling stops if the expression assigned to the target evaluates to a falsey value (such as void, false). Therefore, to continue propagation, the expression should evaluate to true:  
```
<div id="parent" (click)="doWork($event) || true">
```
- Custom events created on Angular components do not support event bubbling.

#### $event
- The `$event` object contains the details of the event that occurred.
- The shape of the `$event` object is decided based on the event type. For HTML elements, it is a [DOM event object](https://developer.mozilla.org/en-US/docs/Web/Events), which may vary based on the actual event; But if it is a custom component event, what is passed in the `$event` object is decided by the component implementation.


## Two-way binding with ngModel
```
<input [(ngModel)]="workout.name">
```
equals to
```
<input [value]="workout.name"  
(input)="workout.name=$event.target.value" >
```


## Template
### Template reference variables
Template reference variables are created on the view template and are mostly consumed from the view. These variables can be identified by the `#` prefix used to declare them. (The `ref-` prefix is the canonical alternative to #. The `#runner` variable can also be declared as `ref-runner`.)  

One of the greatest benefits of template variables is that they facilitate cross-component communication at the view template level. Once declared, such variables can be referenced by sibling elements/components and their children.   

### Template variable assignment
- If a directive is present on the element, the directive sets the value.   
For example:
```
// The MyAudioDirective directive sets the ticks variable to an instance of MyAudioDirective
<audio #ticks="MyAudio" loop src="/static/audio/tick10s.mp3"></audio>
//
@Directive({
    selector: 'audio',
    exportAs: 'MyAudio'
})
export class MyAudioDirective {...}
```
- If there is no directive present, either the underlying HTML DOM element is assigned or a component object is assigned  
For example:
```
<input #emailId type="email">Email to {{emailId.value}}
<workout-runner #runner></workout-runner>
```

### Using the @ViewChild decorator
The `@ViewChild` decorator informs the Angular DI framework to search for the child component/directive/element in the component tree and inject them into the parent.   

The selector parameter on `ViewChild` can be a string value, in which case Angular searches for a matching template variable; or it can be a type.   
```
 @ViewChild('ticks') private ticks: MyAudioDirective;
 @ViewChild(MyAudioDirective) private ticks: MyAudioDirective;
```

If there are multiple same directives, the first match is injected.  

Properties decorated with `@ViewChild` are sure to be set before the `ngAfterViewInit` event hook on the component is called. This implies such properties are `null` if accessed inside the constructor.


### The @ViewChildren decorator
`@ViewChildren` works similarly to `@ViewChild` except it's used when the view has multiple child components/directives of one type.  
```
@ViewChildren(MyAudioDirective) allAudios: QueryList<MyAudioDirective>;
```
`QueryList` is an immutable collection of components/directives that Angular was able to locate. The best thing about this list is that Angular will keep this list in sync with the state of the view. When directives/components get added/removed from the view dynamically this list is updated too.   

## References
https://www.packtpub.com/mapt/book/Web%20Development/9781785887192/2/ch02lvl1sec28/building-the-7-minute-workout-view
