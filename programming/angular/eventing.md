## Custom Events
Using the `@Output` decorator and the `EventEmitter` class we can define and raise custom events.

### The @Output decorator
The `@Output` decorator instructs Angular to make an event available for template binding. You can create an event without the `@Output` decorator, but such an event cannot be referenced in html.

The `@Output` decorated can also take a parameter, signifying the name of the event. If not provided, the decorator uses the property name: `@Output("workoutPaused") exercisePaused: EventEmitter<number> = new EventEmitter<number>();` (This declares an event `workoutPaused` instead of `exercisePaused`.)


### Eventing with EventEmitter
The `EventEmitter` class acts both as an **observer** and **observable**. We can fire events on it and it can be used to listen to events too.

Two functions:
- emit
- subscribe
