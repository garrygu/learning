
# Template-driven forms

# Model-driven forms

# Angular Validation
## The Angular model state
- pristine: The value of this is true as long as the user does not interact with the input. Any update to the input field and ng-pristine is set to false.
- dirty: This is the reverse of ng-pristine. This is true when the input data has been updated.
- touched: This is true if the control ever had focus.
- untouched: This is true if the control has never lost focus. This is just the reverse of ng-touched.
- valid: This is true if there are validations defined on the input element and none of them are failing.
- invalid: This is true if any of the validations defined on the element are failing.

Based on the model state, Angular adds some CSS classes to an input element:
- ng-valid: This is used if the model is valid
- ng-invalid: This is used if the model is invalid
- ng-pristine: This is used if the model is pristine
- ng-dirty: This is used if the model is dirty
- ng-untouched: This is used when the input is never visited
- ng-touched: This is used when the input has focus
