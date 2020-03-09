 To detect changes in a property value Angular does a strict comparison (===) between the previous and current value. For reference types it means only the references are compared. No deep comparison is done.

 Adding elements to an existing array does not change the array reference and hence Angular fails to detect any change to the array. Once a pipe is marked as stateful, the pipe is evaluated irrespective of whether the piped array has changed or not.

 Since Angular cannot know when any bound property is updated automatically, it instead resorts to checking every bound property when a change detection run is triggered. Starting from the root of the component tree, Angular checks each bound property for changes down the component hierarchy. If a change is detected that component is marked for refresh.


 A change detection run works in two phases:
 - In the first phase it does the component tree walk and marks components that need to be refreshed due to model updates
 - In the second phase, the actual view is synchronized with the underlying model


 An Angular change detection run is triggered when any of these events are triggered:
 - User input/ browser events
 - Remote XHR requests
 - setTimeout and setInterval
