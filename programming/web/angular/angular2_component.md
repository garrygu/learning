Defined with two pieces:
- Class definition
- Class decoration

## Data Binding with input properties
Format：  
`<pomodoro-timer [seconds]="25"></pomodoro-timer>`

Some extra syntactic sugar when binding expressions:
```
<div [attr.hidden]="isHidden">...</div>
<input [class.is-valid]="isValid">
<div [style.width.px]="myWidth">...</div>
```

Local references in templates: `#<name>`  
```
<countdown [seconds]="25"
  (complete)="onCountdownCompleted()"
  #counter>
</countdown>
```


## Event binding with output properties
Format:  
`<pomodoro-timer (countdownComplete)="onCountownCompleted()"></pomodoro-timer>`  


## Define input and output properties
- Through `@Input()` and `@Output()`  
```
@Component({})
class CountdownComponent {
  @Input() seconds: number;
  @Output() complete: EventEmitter<any> = new EventEmitter();
  @Output() progress: EventEmitter<number> = new EventEmitter();
}
```

- Through `inputs` and `outputs` property names by means of the @Component decorator
```
@Component({
  inputs: ['seconds'],
  outputs: ['complete', 'progress']
})
class CountdownComponent {
  seconds: number;
  complete: EventEmitter<any> = new EventEmitter();
  progress: EventEmitter<number> = new EventEmitter();
}
```    

## Communicating between components through custom events
- EventEmitter   
`EventEmitter` class provides support for emitting Observable data and subscribing Observer consumers to data changes.  
  - emit()  
  - subscribe()  
