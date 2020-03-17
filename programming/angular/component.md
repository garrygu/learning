Defined with two pieces:
- Class definition
- Class decoration


## Template
- Internal template
```
@Component({
    selector: '...',
    template: `...`
})
```

- External Template
```
@Component({
    selector: '...',
    templateUrl: './pomodoro.component.html'
})
```

### Encapsulating CSS styling
https://angular.io/docs/ts/latest/guide/component-styles.html  
- The styles property
  ```
  @Component({
  selector: 'my-component',
  styles: [`
    p {
      text-align: center;
    }
    table {
      margin: auto;
    }`]
  })
  ```
- The styleUrls property
  ```
  @Component({
  selector: 'my-component',
  styleUrls: ['path/to/my-stylesheet.css'],
  styles: [`
    p {
      text-align: center;
    }
    table {
      margin: auto;
    }`]
  })
  ```
- Inline style sheets
  ```
  @Component({
  selector: 'app',
  template: `
    <style> p { color: red; } </style>
    <p>I am a red paragraph</p>`
  })
  ```

- Managing view encapsulation
    - Emulated
    - Native
    - None

  ```
  import {Component, EventEmitter, Input, Output, ViewEncapsulation} from '@angular/core';
  import { bootstrap } from '@angular/platform-browser-dynamic';

  @Component({
    selector: 'countdown',
    template: '<h1>Time left: {{seconds}}</h1>',
    styles: ['h1 { color: #900 }'],
    encapsulation: ViewEncapsulation.Emulated
  })
  class CountdownComponent {
    // Etc...
  }
  ```


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


## Lifecycle hooks
https://angular.io/guide/lifecycle-hooks  

- `ngOnInit` - Gets fired when the component's data-bound properties are initialized but before the view initialization starts
- `ngOnChanges` - When a component's input properties change
- `ngOnDestroy` - When a component is destroyed
- `ngAfterViewInit`
