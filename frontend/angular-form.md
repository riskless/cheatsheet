### reactive forms
- 2 ways to create forms
	- Template Driven Forms
	- Reactive Forms (Also called Model Driven Forms)
- Creating a form group model 
```js
/* component */
import { Component, OnInit } from '@angular/core';
// Import FormGroup and FormControl classes
import { FormGroup, FormControl } from '@angular/forms';

@Component({
  selector: 'app-create-employee',
  templateUrl: './create-employee.component.html',
  styleUrls: ['./create-employee.component.css']
})
export class CreateEmployeeComponent implements OnInit {
  // This FormGroup contains fullName and Email form controls
  employeeForm: FormGroup;

  constructor() { }

  // Initialise the FormGroup with the 2 FormControls we need.
  // ngOnInit ensures the FormGroup and it's form controls are
  // created when the component is initialised
  ngOnInit() {
    this.employeeForm = new FormGroup({
      fullName: new FormControl(),
      email: new FormControl()
    });
  }

	onSubmit(): void {
		console.log(this.employeeForm.value);
}
}

/* html */
<form class="form-horizontal" [formGroup]="employeeForm" (ngSubmit)="onSubmit()">
  <div class="panel panel-primary">
    <div class="panel-heading">
      <h3 class="panel-title">Create Employee</h3>
    </div>
    <div class="panel-body">
      <div class="form-group">
        <label class="col-sm-2 control-label" for="fullName">Full Name</label>
        <div class="col-sm-8">
          <input id="fullName" type="text" class="form-control" formControlName="fullName">
        </div>
      </div>

      <div class="form-group">
        <label class="col-sm-2 control-label" for="email">Email</label>
        <div class="col-sm-8">
          <input id="email" type="text" class="form-control" formControlName="email">
        </div>
      </div>

    </div>
    <div class="panel-footer">
      <button class="btn btn-primary" type="submit">Save</button>
    </div>
  </div>
</form>

<table border="1">
  <tr>
    <th style="padding: 10px">FormGroup</th>
    <th style="padding: 10px">FormControl (fullName)</th>
  </tr>
  <tr>
    <td style="padding: 10px">
      touched : {{ employeeForm.touched }}
      <br/> dirty : {{ employeeForm.dirty }}
      <br/> valid : {{ employeeForm.valid }}
      <br/> Form Values : {{employeeForm.value | json}}
    </td>
    <td style="padding: 10px">
      touched : {{ employeeForm.get('fullName').touched }}
      <br/> dirty : {{ employeeForm.get('fullName').dirty }}
      <br/> valid : {{ employeeForm.get('fullName').valid }}
      <br/> FullName Value : {{employeeForm.get('fullName').value}}
    </td>
  </tr>
</table>

/* AppModule  */
import { ReactiveFormsModule } from '@angular/forms';

/* To access a FormControl in a FormGroup */
employeeForm.controls.fullName.value
employeeForm.get('fullName').value
```

### nested form group
```js
/* component */
ngOnInit() {
  this.employeeForm = new FormGroup({
    fullName: new FormControl(),
    email: new FormControl(),
    // Create skills form group
    skills: new FormGroup({
      skillName: new FormControl(),
      experienceInYears: new FormControl(),
      proficiency: new FormControl()
    })
  });
}

/* html */
<div formGroupName="skills">

  <div class="form-group">
    <label class="col-sm-2 control-label" for="skillName">
      Skill
    </label>
    <div class="col-sm-4">
      <input type="text" class="form-control" id="skillName"
          placeholder="Name" formControlName="skillName">
    </div>
    <div class="col-sm-4">
      <input type="text" placeholder="Experience in Years"
          class="form-control" formControlName="experienceInYears">
    </div>
  </div>

  <div class="form-group">
    <label class="col-md-2 control-label">Proficiency</label>
    <div class="col-md-8">
      <label class="radio-inline">
        <input id="proficiency" type="radio" value="beginner"
               formControlName="proficiency">Beginner
      </label>
      <label class="radio-inline">
        <input id="proficiency" type="radio" value="intermediate"
               formControlName="proficiency">Intermediate
      </label>
      <label class="radio-inline">
        <input id="proficiency" type="radio" value="advanced"
               formControlName="proficiency">Advanced
      </label>
    </div>
  </div>

</div>
Form Values : {{employeeForm.value}}
```

### setvalue and patchvalue methods
```js
/* html */
<div class="panel-footer">
  <div class="btn-toolbar">
  <button class="btn btn-primary" type="submit">Save</button>
  <button class="btn btn-primary" type="button"
          (click)="onLoadDataClick()">Load Data</button>
</div>

/* use setValue() to update all form controls */
onLoadDataClick(): void {
  this.employeeForm.setValue({
    fullName: 'Pragim Technologies',
    email: 'pragim@pragimtech.com',
    skills: {
      skillName: 'C#',
      experienceInYears: 5,
      proficiency: 'beginner'
    }
  });
}

/* patchValue() to update a sub-set of form controls */
onLoadDataClick(): void {
  this.employeeForm.patchValue({
    fullName: 'Pragim Technologies',
    email: 'pragim@pragimtech.com',
  });
}
```
### FormBuilder 
- The FormBuilder class provides syntactic sugar that shortens creating instances of a FormControl, FormGroup, or FormArray.
	- control() - Construct a new FormControl instance
	- group() - Construct a new FormGroup instance
	- array() - Construct a new FormArray instance
- 2 ways to create reactive forms
	- Explicitly creating instances of FormGroup and FormControl classes using the new keyword
	- Using the FormBuilder class

```js
/* FormBuilder */
import { FormBuilder } from '@angular/forms';

constructor(private fb: FormBuilder) { }

this.employeeForm = this.fb.group({
  fullName: [''],
  email: [''],
  skills: this.fb.group({
    skillName: [''],
    experienceInYears: [''],
    proficiency: ['beginner']
  }),
});
```

### reactive forms validation
- built-in validator functions
	- required:	Validate that a field has a value. Used for required fields. For example, Name is required.
	- requiredTrue:	Validate that the field value is true. This validator is commonly used on a required checkbox. For example, "I Agree to the terms" checkbox must be checked to submit the form.
	- email: Validate that the field value has a valid email pattern. For example, abc is not a valid email.
	- pattern: Validate that the field value matches the specified regex pattern.
	- min: Validate that the field value is greater than or equal to the provided number. For example, minimum age to vote is 18.
	- max: Validate that the field value is less than or equal to the provided number. For example, people over the age of 90 are not eligible for this insurance policy.
	- minLength: The number of characters in the field must be greater than or equal to the provided minimum length. For example, Full Name must be at least 3 characters.
	- maxLength: The number of characters in the field must be less than or equal to the provided maximum length. For example, Description cannot exceed 500 characters.
```js
/* component */
import { Validators } from '@angular/forms';

this.employeeForm = this.fb.group({
  fullName: ['', [Validators.required, Validators.minLength(2), Validators.maxLength(10)]],
  // OtherFields...
});

/* html */
<div class="form-group"
      [ngClass]="{'has-error': ((employeeForm.get('fullName').touched ||
				 employeeForm.get('fullName').dirty) &&
				 employeeForm.get('fullName').errors)}">
  <label class="col-sm-2 control-label" for="fullName">Full Name</label>
  <div class="col-sm-8">
    <input id="fullName" type="text" class="form-control" formControlName="fullName">
    <span class="help-block"
          *ngIf="((employeeForm.get('fullName').touched ||
                   employeeForm.get('fullName').dirty) &&
                   employeeForm.get('fullName').errors)">
      <span *ngIf="employeeForm.get('fullName').errors.required">
        Full Name is required
      </span>
      <span *ngIf="employeeForm.get('fullName').errors.minlength ||
                   employeeForm.get('fullName').errors.maxlength">
          Full Name must be greater than 2 characters and less than 10 characters
      </span>
    </span>
  </div>
</div>
```

### form control valuechanges
- valueChanges property is an observable that emits an event every time the value of the control changes
- To be able to monitor and react when the FormControl or FormGroup value changes, subscribe to the valueChanges observable 
```js
ngOnInit() {

  this.employeeForm = this.fb.group({
    fullName: ['',
      [
        Validators.required,
        Validators.minLength(2),
        Validators.maxLength(10)]
    ],
    email: [''],
    skills: this.fb.group({
      skillName: ['C#'],
      experienceInYears: [''],
      proficiency: ['beginner']
    }),
  });

  // Subscribe to valueChanges observable
  this.employeeForm.get('fullName').valueChanges.subscribe(
    value => {
      console.log(value);
    }
  );

}

// Subscribe to FormGroup valueChanges observable
this.employeeForm.valueChanges.subscribe(
  value => {
    console.log(JSON.stringify(value));
  }
);
```

### Loop through all form controls in formgroup in reactive form
```js
this.employeeForm = this.fb.group({
  fullName: [''],
  email: [''],
  skills: this.fb.group({
    skillName: [''],
    experienceInYears: [''],
    proficiency: ['beginner']
  }),
});

logKeyValuePairs(group: FormGroup): void {
  // loop through each key in the FormGroup
  Object.keys(group.controls).forEach((key: string) => {
    // Get a reference to the control using the FormGroup.get() method
    const abstractControl = group.get(key);
    // If the control is an instance of FormGroup i.e a nested FormGroup
    // then recursively call this same method (logKeyValuePairs) passing it
    // the FormGroup so we can get to the form controls in it
    if (abstractControl instanceof FormGroup) {
      this.logKeyValuePairs(abstractControl);
      // If the control is not a FormGroup then we know it's a FormControl
    } else {
      console.log('Key = ' + key + ' && Value = ' + abstractControl.value);
    }
  });
}
```

### how to move validation messages to the component class.
```js
/* component */
// This object will hold the messages to be displayed to the user
// Notice, each key in this object has the same name as the
// corresponding form control
formErrors = {
  'fullName': '',
  'email': '',
  'skillName': '',
  'experienceInYears': '',
  'proficiency': ''
};

// This object contains all the validation messages for this form
validationMessages = {
  'fullName': {
    'required': 'Full Name is required.',
    'minlength': 'Full Name must be greater than 2 characters.',
    'maxlength': 'Full Name must be less than 10 characters.'
  },
  'email': {
    'required': 'Email is required.'
  },
  'skillName': {
    'required': 'Skill Name is required.',
  },
  'experienceInYears': {
    'required': 'Experience is required.',
  },
  'proficiency': {
    'required': 'Proficiency is required.',
  },
};

ngOnInit() {
  // Modify the code to include required validators on
  // all form controls
  this.employeeForm = this.fb.group({
    fullName: ['', [Validators.required,
    Validators.minLength(2), Validators.maxLength(10)]],
    email: ['', Validators.required],
    skills: this.fb.group({
      skillName: ['', Validators.required],
      experienceInYears: ['', Validators.required],
      proficiency: ['', Validators.required]
    }),
  });

  // When any of the form control value in employee form changes
  // our validation function logValidationErrors() is called
  this.employeeForm.valueChanges.subscribe((data) => {
    this.logValidationErrors(this.employeeForm);
  });
}

logValidationErrors(group: FormGroup): void {
  // Loop through each control key in the FormGroup
  Object.keys(group.controls).forEach((key: string) => {
    // Get the control. The control can be a nested form group
    const abstractControl = group.get(key);
    // If the control is nested form group, recursively call
    // this same method
    if (abstractControl instanceof FormGroup) {
      this.logValidationErrors(abstractControl);
      // If the control is a FormControl
    } else {
      // Clear the existing validation errors
      this.formErrors[key] = '';
      if (abstractControl && !abstractControl.valid) {
        // Get all the validation messages of the form control
        // that has failed the validation
        const messages = this.validationMessages[key];
        // Find which validation has failed. For example required,
        // minlength or maxlength. Store that error message in the
        // formErrors object. The UI will bind to this object to
        // display the validation errors
        for (const errorKey in abstractControl.errors) {
          if (errorKey) {
            this.formErrors[key] += messages[errorKey] + ' ';
          }
        }
      }
    }
  });
}

onLoadDataClick(): void {
  this.logValidationErrors(this.employeeForm);
  console.log(this.formErrors);
}

/* html */
<div class="form-group" [ngClass]="{'has-error': formErrors.fullName}">
  <label class="col-sm-2 control-label" for="fullName">Full Name</label>
  <div class="col-sm-8">
    <input id="fullName" type="text" class="form-control" formControlName="fullName" (blur)="logValidationErrors()">
    <span class="help-block" *ngIf="formErrors.fullName">
      {{formErrors.fullName}}
    </span>
  </div>
</div>

<div class="form-group" [ngClass]="{'has-error': formErrors.email}">
  <label class="col-sm-2 control-label" for="email">Email</label>
  <div class="col-sm-8">
    <input id="email" type="text" class="form-control"
           formControlName="email" (blur)="logValidationErrors()">
    <span class="help-block" *ngIf="formErrors.email">
      {{formErrors.email}}
    </span>
  </div>
</div>

<div class="well">
  <div formGroupName="skills">

    <div class="form-group" [ngClass]="{'has-error': formErrors.skillName}">
      <label class="col-sm-2 control-label" for="skillName">
        Skill
      </label>
      <div class="col-sm-4">
        <input type="text" class="form-control" id="skillName" formControlName="skillName"
               (blur)="logValidationErrors()" placeholder="C#, Java, Angular etc...">
        <span class="help-block" *ngIf="formErrors.skillName">
          {{formErrors.skillName}}
        </span>
      </div>
    </div>

    <div class="form-group" [ngClass]="{'has-error': formErrors.experienceInYears}">
      <label class="col-sm-2 control-label" for="experienceInYears">
        Experience
      </label>
      <div class="col-sm-4">
        <input type="text" class="form-control" id="experienceInYears"
               formControlName="experienceInYears" placeholder="In Years"
              (blur)="logValidationErrors()">
        <span class="help-block" *ngIf="formErrors.experienceInYears">
          {{formErrors.experienceInYears}}
        </span>
      </div>
    </div>

    <div class="form-group" [ngClass]="{'has-error': formErrors.proficiency}">
      <label class="col-md-2 control-label">Proficiency</label>
      <div class="col-md-8">
        <label class="radio-inline">
          <input type="radio" value="beginner" formControlName="proficiency"
                 (blur)="logValidationErrors()">Beginner
        </label>
        <label class="radio-inline">
          <input type="radio" value="intermediate" formControlName="proficiency"
                 (blur)="logValidationErrors()">Intermediate
        </label>
        <label class="radio-inline">
          <input type="radio" value="advanced" formControlName="proficiency"
                 (blur)="logValidationErrors()">Advanced
        </label>
        <span class="help-block" *ngIf="formErrors.experienceInYears">
          {{formErrors.proficiency}}
        </span>
      </div>
    </div>
  </div>
</div>
```

### Dynamically adding or removing form control validators in reactive form
```js
/* html */
<!-- Notice the click event handler on both the radio buttons. When "Email"
radio button is clicked "email" string is passed to the event handler
function. Similarly, when "Phone" radio button is clicked "phone"
string is passed to the event handler function -->

<div class="form-group">
  <label class="col-md-2 control-label">Contact Preference</label>
  <div class="col-md-8">
    <label class="radio-inline">
      <input type="radio" value="email" formControlName="contactPreference"
              (click)="onContactPrefernceChange('email')">Email
    </label>
    <label class="radio-inline">
      <input type="radio" value="phone" formControlName="contactPreference"
              (click)="onContactPrefernceChange('phone')">Phone
    </label>
  </div>
</div>

<!-- Email input element -->
<div class="form-group" [ngClass]="{'has-error': formErrors.email}">
  <label class="col-sm-2 control-label" for="email">Email</label>
  <div class="col-sm-8">
    <input id="email" type="text" class="form-control"
            formControlName="email" (blur)="logValidationErrors()">
    <span class="help-block" *ngIf="formErrors.email">
      {{formErrors.email}}
    </span>
  </div>
</div>

<!-- Phone input element -->
<div class="form-group" [ngClass]="{'has-error': formErrors.phone}">
  <label class="col-sm-2 control-label" for="email">Phone</label>
  <div class="col-sm-8">
    <input id="phone" type="text" class="form-control"
            formControlName="phone" (blur)="logValidationErrors()">
    <span class="help-block" *ngIf="formErrors.phone">
      {{formErrors.phone}}
    </span>
  </div>
</div>

/* component */
// Include phone property
formErrors = {
  'email': '',
  'phone': '',
};

// Include required error message for phone form control
validationMessages = {
  'email': {
    'required': 'Email is required.',
    'emailDomain': 'Email domian should be prgaimtech.com'
  },
  'phone': {
    'required': 'Phone is required.'
  },
};

ngOnInit() {
  // Include FormControls for contactPreference, email & phone
  // contactPreference has email as the default value
  this.employeeForm = this.fb.group({
    contactPreference: ['email'],
    email: ['', Validators.required],
    phone: [''],
  });

  this.employeeForm.valueChanges.subscribe((data) => {
    this.logValidationErrors(this.employeeForm);
  });
}

// If the Selected Radio Button value is "phone", then add the
// required validator function otherwise remove it
onContactPrefernceChange(selectedValue: string) {
  const phoneFormControl = this.employeeForm.get('phone');
  if (selectedValue === 'phone') {
    phoneFormControl.setValidators(Validators.required);
  } else {
    phoneFormControl.clearValidators();
  }
  phoneFormControl.updateValueAndValidity();
}


/* valueChanges observable */
// We can also achieve the same thing by subscribing to the valueChanges observable of contactPreference radio button in code, instead of binding to the click event in the HTML. 
//-- html
<div class="form-group">
  <label class="col-md-2 control-label">Contact Preference</label>
  <div class="col-md-8">
    <label class="radio-inline">
      <input type="radio" value="email" formControlName="contactPreference">Email
    </label>
    <label class="radio-inline">
      <input type="radio" value="phone" formControlName="contactPreference">Phone
    </label>
  </div>
</div>

//-- component
this.employeeForm.get('contactPreference')
	.valueChanges.subscribe((data: string) => {
  this.onContactPrefernceChange(data);
});

```

### reactive form custom validator
```js
// Create the custom validator function */
function emailDomain(control: AbstractControl): { [key: string]: any } | null {
  const email: string = control.value;
  const domain = email.substring(email.lastIndexOf('@') + 1);
  if (email === '' || domain.toLowerCase() === 'pragimtech.com') {
    return null;
  } else {
    return { 'emailDomain': true };
  }
}

// Attach the custom validator function to the control that we want to validate
email: ['', [Validators.required, emailDomain]]

// Display the validation error message
<span *ngIf="employeeForm.get('email').errors.emailDomain">
  Email domian should be prgaimtech.com
</span>

// if you want the validation error message and logic in the component class
<div class="form-group" [ngClass]="{'has-error': formErrors.email}">
  <label class="col-sm-2 control-label" for="email">Email</label>
  <div class="col-sm-8">
    <input id="email" type="text" class="form-control"
            formControlName="email" (blur)="logValidationErrors()">
    <span class="help-block" *ngIf="formErrors.email">
      {{formErrors.email}}
    </span>
  </div>
</div>
```

### creating and using a custom validator with parameters.
```js
// Custom Validator with parameter
function emailDomain(domainName: string) {
  return (control: AbstractControl): { [key: string]: any } | null => {
    const email: string = control.value;
    const domain = email.substring(email.lastIndexOf('@') + 1);
    if (email === '' || domain.toLowerCase() === domainName.toLowerCase()) {
      return null;
    } else {
      return { 'emailDomain': true };
    }
  };
}

// Passing the value for the custom validator parameter
email: ['', [Validators.required, emailDomain('dell.com')]]

// validation error message
validationMessages = {
  'email': {
    'required': 'Email is required.',
    'emailDomain': 'Email domian should be dell.com'
  },
  'proficiency': {
    'required': 'Proficiency is required.',
  },
};
```

### Reusable Custom Validator
```js
/* CustomValidators */
import { AbstractControl } from '@angular/forms';

export class CustomValidators {
    static emailDomain(domainName: string) {
        return (control: AbstractControl): { [key: string]: any } | null => {
            const email: string = control.value;
            const domain = email.substring(email.lastIndexOf('@') + 1);
            if (email === '' || domain.toLowerCase() === domainName.toLowerCase()) {
                return null;
            } else {
                return { 'emailDomain': true };
            }
        };
    }
}

/* component */
import { CustomValidators } from '../shared/custom.validators';
email: ['', [CustomValidators.emailDomain('dell.com')]]

```
### cross field validation in a reactive form
```js
/* html */
<div formGroupName="emailGroup">
  <div class="form-group" [ngClass]="{'has-error': formErrors.email}">
    <label class="col-sm-2 control-label" for="email">Email</label>
    <div class="col-sm-8">
      <input id="email" type="text" class="form-control"
             formControlName="email" (blur)="logValidationErrors()">
      <span class="help-block" *ngIf="formErrors.email">
        {{formErrors.email}}
      </span>
    </div>
  </div>

  <div class="form-group" [ngClass]="{'has-error': formErrors.confirmEmail
                                                || formErrors.emailGroup}">
    <label class="col-sm-2 control-label" for="confirmEmail">
      Confirm Email
    </label>
    <div class="col-sm-8">
      <input id="confirmEmail" type="text" class="form-control"
             formControlName="confirmEmail" (blur)="logValidationErrors()">
      <span class="help-block"
            *ngIf="formErrors.confirmEmail || formErrors.emailGroup">
        {{formErrors.confirmEmail ? formErrors.confirmEmail
          : formErrors.emailGroup}}
      </span>
    </div>
  </div>
</div>

/* component */
// Group properties on the formErrors object. The UI will bind to these properties
// to display the respective validation messages
formErrors = {
  'email': '',
  'confirmEmail': '',
  'emailGroup': ''
};

// This structure stoes all the validation messages for the form Include validation
// messages for confirmEmail and emailGroup properties. Notice to store the
// validation message for the emailGroup we are using emailGroup key. This is the
// same key that the matchEmails() validation function below returns, if the email
// and confirm email do not match.
validationMessages = {
  'email': {
    'required': 'Email is required.',
    'emailDomain': 'Email domian should be dell.com'
  },
  'confirmEmail': {
    'required': 'Confirm Email is required.'
  },
  'emailGroup': {
    'emailMismatch': 'Email and Confirm Email do not match.'
  },
};

// email and confirmEmail form controls are grouped using a nested form group
// Notice, the validator is attached to the nested emailGroup using an object
// with key validator. The value is our validator function matchEmails() which
// is defined below. The important point to keep in mind is when the validation
// fails, the validation key is attached the errors collection of the emailGroup
// This is the reason we added emailGroup key both to formErrors object and
// validationMessages object.
ngOnInit() {
  this.employeeForm = this.fb.group({
    emailGroup: this.fb.group({
      email: ['', [Validators.required, emailDomain('dell.com')]],
      confirmEmail: ['', [Validators.required]],
    }, { validator: matchEmails }),
  });

  this.employeeForm.valueChanges.subscribe((data) => {
    this.logValidationErrors(this.employeeForm);
  });
}

logValidationErrors(group: FormGroup = this.employeeForm): void {
  Object.keys(group.controls).forEach((key: string) => {
    const abstractControl = group.get(key);
    this.formErrors[key] = '';
    // Loop through nested form groups and form controls to check
    // for validation errors. For the form groups and form controls
    // that have failed validation, retrieve the corresponding
    // validation message from validationMessages object and store
    // it in the formErrors object. The UI binds to the formErrors
    // object properties to display the validation errors.
    if (abstractControl && !abstractControl.valid
      && (abstractControl.touched || abstractControl.dirty)) {
      const messages = this.validationMessages[key];
      for (const errorKey in abstractControl.errors) {
        if (errorKey) {
          this.formErrors[key] += messages[errorKey] + ' ';
        }
      }
    }

    if (abstractControl instanceof FormGroup) {
      this.logValidationErrors(abstractControl);
    }
  });
}

// Nested form group (emailGroup) is passed as a parameter. Retrieve email and
// confirmEmail form controls. If the values are equal return null to indicate
// validation passed otherwise an object with emailMismatch key. Please note we
// used this same key in the validationMessages object against emailGroup
// property to store the corresponding validation error message
function matchEmails(group: AbstractControl): { [key: string]: any } | null {
  const emailControl = group.get('email');
  const confirmEmailControl = group.get('confirmEmail');

  if (emailControl.value === confirmEmailControl.value || confirmEmailControl.pristine) {
    return null;
  } else {
    return { 'emailMismatch': true };
  }
}
```

### FormArray
- push; Inserts the control at the end of the array
- insert: Inserts the control at the specified index in the array
- removeAt: Removes the control at the specified index in the array
- setControl:	Replace an existing control at the specified index in the array
- at: Return the control at the specified index in the array

```js
// Create a FormArray, using the new keyword
const formArray = new FormArray([
  new FormControl('John', Validators.required),
  new FormGroup({
    country: new FormControl('', Validators.required)
  }),
  new FormArray([])
]);

// Create a FormArray, using the array() method of the FormBuilder class
const formArray = this.fb.array([
  new FormControl('John', Validators.required),
  new FormControl('IT', Validators.required),
]);

// To programmatically find the number of elements in a FormArray
formArray.length

// To iterate over a FormArray
for (const control of formArray.controls) {
  if (control instanceof FormControl) {
    console.log('control is FormControl');
  }
  if (control instanceof FormGroup) {
    console.log('control is FormGroup');
  }
  if (control instanceof FormArray) {
    console.log('control is FormArray');
  }
}

// value property of a FormArray
// formArray.value returns ["John", "IT", ""]
const formArray = this.fb.array([
  new FormControl('John', Validators.required),
  new FormControl('IT', Validators.required),
  new FormControl('', Validators.required),
]);


```


### Creating formarray of formgroup objects
```js
/* html */
<div class="well">
  <div formArrayName="skills">
    <div formGroupName="0">
      <!-- Skill Name Label & Form Control HTML
        Experience Label & Form Control HTML
        Proficiency Label & Form Control HTML -->
    </div>
  </div>
</div>

/* component */
constructor(private fb: FormBuilder) { }

ngOnInit() {
  this.employeeForm = this.fb.group({
    fullName: ['', [Validators.required]],
    contactPreference: ['email'],
    // Other Form Controls..
    // Create skills FormArray using the injected FormBuilder
    // class array() method. At the moment, in the created
    // FormArray we only have one FormGroup instance that is
    // returned by addSkillFormGroup() method
    skills: this.fb.array([
      this.addSkillFormGroup()
    ])
  });

  // Rest of the code
}

addSkillFormGroup(): FormGroup {
  return this.fb.group({
    skillName: ['', Validators.required],
    experienceInYears: ['', Validators.required],
    proficiency: ['', Validators.required]
  });
}

logValidationErrors(group: FormGroup = this.employeeForm): void {
  Object.keys(group.controls).forEach((key: string) => {
    const abstractControl = group.get(key);

    this.formErrors[key] = '';
    if (abstractControl && !abstractControl.valid &&
      (abstractControl.touched || abstractControl.dirty)) {
      const messages = this.validationMessages[key];

      for (const errorKey in abstractControl.errors) {
        if (errorKey) {
          this.formErrors[key] += messages[errorKey] + ' ';
        }
      }
    }

    if (abstractControl instanceof FormGroup) {
      this.logValidationErrors(abstractControl);
    }

    // We need this additional check to get to the FormGroup
    // in the FormArray and then recursively call this
    // logValidationErrors() method to fix the broken validation
    if (abstractControl instanceof FormArray) {
      for (const control of abstractControl.controls) {
        if (control instanceof FormGroup) {
          this.logValidationErrors(control);
        }
      }
    }
  });
}
```

### generating FormGroups and FormControls dynamically at runtime
```js
/* html */
// Include Add Skill button
<div class="form-group">
  <div class="col-md-offset-2 col-md-4">
    <button type="button" class="btn btn-primary" (click)="addSkillButtonClick()">
      Add Skill
    </button>
  </div>
</div>

// Loop over "skills" FormArray to dynamically generate the HTML input elements.
<div formArrayName="skills"
     *ngFor="let skill of employeeForm.get('skills').controls; let i = index">
  <div [formGroupName]="i">
      <!-- Skill Name Label & Form Control HTML
      Experience Label & Form Control HTML
      Proficiency Label & Form Control HTML -->
  </div>
</div>

/* component */
addSkillButtonClick(): void {
  (<FormArray>this.employeeForm.get('skills')).push(this.addSkillFormGroup());
}

```

### Generate unique id value for dynamically created form controls
```js
/* html */
<div formArrayName="skills"
      *ngFor="let skill of employeeForm.get('skills').controls; let i = index">
  <div [formGroupName]="i">

    <div class="form-group" [ngClass]="{'has-error': formErrors.skillName}">
			<!-- [attr.for]="'skillName'+i" -->
      <label class="col-sm-2 control-label" attr.for="{{'skillName'+i}}">
        Skill
      </label>
      <div class="col-sm-4">
				<!-- [id]="'skillName'+i" -->
        <input type="text" class="form-control" id="{{'skillName'+i}}"
                formControlName="skillName" (blur)="logValidationErrors()"
                placeholder="C#, Java, Angular etc...">
        <span class="help-block" *ngIf="formErrors.skillName">
          {{formErrors.skillName}}
        </span>
      </div>
    </div>

    <!-- Experience Label & Form Control HTML
         Proficiency Label & Form Control HTML -->

  </div>
</div>
```

### dynamic forms validation
```html
<div formArrayName="skills"
    *ngFor="let skill of employeeForm.get('skills').controls; let i = index">

  <div [formGroupName]="i">
		<!-- [ngClass]="{'has-error': skill.controls.skillName.invalid && skill.controls.skillName.touched}" -->
    <div class="form-group" [ngClass]="{'has-error':
          skill.get('skillName').invalid && skill.get('skillName').touched}">
      <label class="col-sm-2 control-label" [attr.for]="'skillName'+i">
        Skill
      </label>
      <div class="col-sm-4">
        <input type="text" class="form-control" [id]="'skillName'+i"
                formControlName="skillName" placeholder="C#, Java, Angular etc...">
        <span class="help-block" *ngIf="skill.get('skillName').errors?.required && skill.get('skillName').touched">
          Skill Name is required
        </span>
      </div>
    </div>

  </div>
</div>
```

### formarray validation
- A FormArray can contain form controls, form groups or nested form arrays.
- If all the 3 form controls are valid, then the form group is valid. 
- If all the form groups are valid, then the form array is valid.
- Even if a single form control in a form group is invalid, then the form array is invalid.

```js
// we want to keep "Add Skill" button disabled. 
<button type="button" class="btn btn-primary"
	(click)="addSkillButtonClick()"
	[disabled]="employeeForm.get('skills').invalid">
  Add Skill
</button>
```

### Remove dynamically created form controls
```js
/* DELETE button */
<div class="col-sm-6" *ngIf="employeeForm.get('skills').length>1">
  <button type="button" class="btn btn-danger btn-sm pull-right"
		title="Delete Skill" (click)="removeSkillButtonClick(i)">
    <span class="glyphicon glyphicon-remove"></span>
  </button>
</div>

/* component */
removeSkillButtonClick(skillGroupIndex: number): void {
  (<FormArray>this.employeeForm.get('skills')).removeAt(skillGroupIndex);
}
```


### Creating a fake online REST API
- npm install -g json-server 
- Create db.json file in the root project folder
```js
{
	"employees": [
		{
			"id": 1,
			"fullName": "Mark",
			"contactPreference": "email",
			"email": "mark@email.com",
			"phone": "5641238971",
			"skills": [
				{
					"skillName": "C#",
					"experienceInYears": 1,
					"proficiency": "beginner"
				},
				{
					"skillName": "Java",
					"experienceInYears": 2,
					"proficiency": "intermediate"
				}
			]
		},
		{
			"id": 2,
			"fullName": "John",
			"contactPreference": "phone",
			"email": "john@email.com",
			"phone": "3242138971",
			"skills": [
				{
					"skillName": "Angular",
					"experienceInYears": 2,
					"proficiency": "beginner"
				},
				{
					"skillName": "HTML",
					"experienceInYears": 2,
					"proficiency": "intermediate"
				},
				{
					"skillName": "LINQ",
					"experienceInYears": 3,
					"proficiency": "advanced"
				}
			]
		}
	]
}
```
- command to start the server
	- json-server --watch db.json
	- http://localhost:3000/employees/

### rxjs operators
```js
/* RxJS 5 */
import { Observable } from 'rxjs/Observable';
import { Subject } from 'rxjs/Subject';

import { catchError } from 'rxjs/operators/catchError';

// to create an ErrorObservable
new ErrorObservable('Your error message'); or ErrorObservable.create('Your error message');

/* RxJS 6 */
import { Observable, Subject } from 'rxjs';

import { catchError } from 'rxjs/operators';
import { map, delay, catchError } from 'rxjs/operators';

// to create an ErrorObservable
return throwError('Your error message');
```

### Creating Angular Service
```js
/* service */
import { Injectable } from '@angular/core';
import { IEmployee } from './IEmployee';
import { HttpClient, HttpErrorResponse, HttpHeaders } from '@angular/common/http';

import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class EmployeeService {
	baseUrl = 'http://localhost:3000/employees';
	constructor(private httpClient: HttpClient) {
	}

	getEmployees(): Observable<IEmployee[]> {
			return this.httpClient.get<IEmployee[]>(this.baseUrl)
					.pipe(catchError(this.handleError));
	}

	private handleError(errorResponse: HttpErrorResponse) {
			if (errorResponse.error instanceof ErrorEvent) {
					console.error('Client Side Error :', errorResponse.error.message);
			} else {
					console.error('Server Side Error :', errorResponse);
			}
			return throwError('There is a problem with the service. We are notified & working on it. Please try again later.');
	}

	getEmployee(id: number): Observable<IEmployee> {
			return this.httpClient.get<IEmployee>(`${this.baseUrl}/${id}`)
					.pipe(catchError(this.handleError));
	}

	addEmployee(employee: IEmployee): Observable<IEmployee> {
			return this.httpClient.post<IEmployee>(this.baseUrl, employee, {
					headers: new HttpHeaders({
							'Content-Type': 'application/json'
					})
			})
			.pipe(catchError(this.handleError));
	}

	updateEmployee(employee: IEmployee): Observable<void> {
			return this.httpClient.put<void>(`${this.baseUrl}/${employee.id}`, employee, {
					headers: new HttpHeaders({
							'Content-Type': 'application/json'
					})
			})
					.pipe(catchError(this.handleError));
	}

	deleteEmployee(id: number): Observable<void> {
			return this.httpClient.delete<void>(`${this.baseUrl}/${id}`)
					.pipe(catchError(this.handleError));
	}
}


/* component */
import { Component, OnInit } from '@angular/core';
import { EmployeeService } from './employee.service';
import { IEmployee } from './IEmployee';

@Component({
  selector: 'app-list-employees',
  templateUrl: './list-employees.component.html',
  styleUrls: ['./list-employees.component.css']
})
export class ListEmployeesComponent implements OnInit {
  employees: IEmployee[];

  constructor(private _employeeService: EmployeeService) { }

  ngOnInit() {
    this._employeeService.getEmployees().subscribe(
      (employeeList) => this.employees = employeeList,
      (err) => console.log(err)
    );
  }

}

/* ISkill */
export interface ISkill {
    skillName: string;
    experienceInYears: number;
    proficiency: string;
}

import { ISkill } from './ISkill';

/* IEmployee */
export interface IEmployee {
    id: number;
    fullName: string;
    email: string;
    phone?: number;
    contactPreference: string;
    skills: ISkill[];
}

/* AppModule */
import { EmployeeService } from './employee/employee.service';
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  declarations: [
    AppComponent,
    CreateEmployeeComponent,
    ListEmployeesComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    ReactiveFormsModule
  ],
  providers: [EmployeeService],
  bootstrap: [AppComponent]
})
export class AppModule { }

/* html */
<div class="table-responsive">
  <table class="table table-bordered" *ngIf="employees && employees.length">
    <thead>
      <tr class="bg-primary">
        <th>Name</th>
        <th>Email</th>
        <th>Phone</th>
        <th>Contact Preference</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let employee of employees">
        <td>{{ employee.fullName }}</td>
        <td>{{ employee.email }}</td>
        <td>{{ employee.phone }}</td>
        <td>{{ employee.contactPreference }}</td>
        <td> <button class="btn btn-primary">Edit</button> </td>
      </tr>
    </tbody>
  </table>
</div>
```

### Implementing EDIT operation in a reactive form
```js
/* AppRoutingModule */
const appRoutes: Routes = [
  { path: 'list', component: ListEmployeesComponent },
  { path: 'create', component: CreateEmployeeComponent },
  { path: 'edit/:id', component: CreateEmployeeComponent },
  { path: '', redirectTo: '/list', pathMatch: 'full' }
];

/* list-employees.component.html */
<button class="btn btn-primary" (click)="editButtonClick(employee.id)">
	Edit
</button>

/* list-employees.component.ts */
import { Router } from '@angular/router';

constructor(private _employeeService: EmployeeService, private _router: Router) { }

editButtonClick(employeeId: number) {
  this._router.navigate(['/edit', employeeId]);
}

/* create-employee.component.ts */
import { ActivatedRoute } from '@angular/router';
import { EmployeeService } from './employee.service';
import { IEmployee } from './IEmployee';
import { ISkill } from './ISkill';

constructor(private fb: FormBuilder,
	private route: ActivatedRoute,
	private employeeService: EmployeeService) { }

ngOnInit() {
  // Other existing code...

  this.route.paramMap.subscribe(params => {
    const empId = +params.get('id');
    if (empId) {
      this.getEmployee(empId);
    }
  });
}

getEmployee(id: number) {
  this.employeeService.getEmployee(id)
    .subscribe(
      (employee: IEmployee) => this.editEmployee(employee),
      (err: any) => console.log(err)
    );
}

editEmployee(employee: IEmployee) {
  this.employeeForm.patchValue({
    fullName: employee.fullName,
    contactPreference: employee.contactPreference,
    emailGroup: {
      email: employee.email,
      confirmEmail: employee.email
    },
    phone: employee.phone
  });
}

/* logValidationErrors */
ogValidationErrors(group: FormGroup = this.employeeForm): void {
  Object.keys(group.controls).forEach((key: string) => {
    const abstractControl = group.get(key);

    this.formErrors[key] = '';
    // abstractControl.value !== '' (This condition ensures if there is a value in the
    // form control and it is not valid, then display the validation error)
    if (abstractControl && !abstractControl.valid &&
        (abstractControl.touched || abstractControl.dirty || abstractControl.value !== '')) {
      const messages = this.validationMessages[key];

      for (const errorKey in abstractControl.errors) {
        if (errorKey) {
          this.formErrors[key] += messages[errorKey] + ' ';
        }
      }
    }

    if (abstractControl instanceof FormGroup) {
      this.logValidationErrors(abstractControl);
    }
  });
}

/* matchEmail */
function matchEmail(group: AbstractControl): { [key: string]: any } | null {
  const emailControl = group.get('email');
  const confirmEmailControl = group.get('confirmEmail');
  // If confirm email control value is not an empty string, and if the value
  // does not match with email control value, then the validation fails
  if (emailControl.value === confirmEmailControl.value
    || (confirmEmailControl.pristine && confirmEmailControl.value === '')) {
    return null;
  } else {
    return { 'emailMismatch': true };
  }
}

/* Save button not disabled if the form is invalid */
<button class="btn btn-primary" type="submit" [disabled]="employeeForm.invalid">
  Save
</button>
```

### how to populate angular formarray with existing data
```js
/* create-employee.component.ts */
editEmployee(employee: IEmployee) {
  this.employeeForm.patchValue({
    fullName: employee.fullName,
    contactPreference: employee.contactPreference,
    emailGroup: {
      email: employee.email,
      confirmEmail: employee.email
    },
    phone: employee.phone
  });

  this.employeeForm.setControl('skills', this.setExistingSkills(employee.skills));
}

setExistingSkills(skillSets: ISkill[]): FormArray {
  const formArray = new FormArray([]);
  skillSets.forEach(s => {
    formArray.push(this.fb.group({
      skillName: s.skillName,
      experienceInYears: s.experienceInYears,
      proficiency: s.proficiency
    }));
  });

  return formArray;
}

// To mark the formArray as touched and dirty. we are using markAsDirty() and markAsTouched() methods.
removeSkillButtonClick(skillGroupIndex: number): void {
  const skillsFormArray = <FormArray>this.employeeForm.get('skills');
  skillsFormArray.removeAt(skillGroupIndex);
  skillsFormArray.markAsDirty();
  skillsFormArray.markAsTouched();
}
```

### Issuing a PUT request to the REST API
```js
/* getEmployee */
getEmployee(id: number) {
  this.employeeService.getEmployee(id)
    .subscribe(
      (employee: IEmployee) => {
        // Store the employee object returned by the
        // REST API in the employee property
        this.employee = employee;
        this.editEmployee(employee);
      },
      (err: any) => console.log(err)
    );
}

/* onSubmit */
onSubmit(): void {
  this.mapFormValuesToEmployeeModel();
  this.employeeService.updateEmployee(this.employee).subscribe(
    () => this.router.navigate(['list']),
    (err: any) => console.log(err)
  );
}

/* mapFormValuesToEmployeeModel */
mapFormValuesToEmployeeModel() {
  this.employee.fullName = this.employeeForm.value.fullName;
  this.employee.contactPreference = this.employeeForm.value.contactPreference;
  this.employee.email = this.employeeForm.value.emailGroup.email;
  this.employee.phone = this.employeeForm.value.phone;
  this.employee.skills = this.employeeForm.value.skills;
}

/* html */
<form [formGroup]="employeeForm" (ngSubmit)="onSubmit()" class="form-horizontal">

/* updateEmployee */
updateEmployee(employee: IEmployee): Observable<void> {
    return this.httpClient.put<void>(`${this.baseUrl}/${employee.id}`, employee, {
        headers: new HttpHeaders({
            'Content-Type': 'application/json'
        })
    })
        .pipe(catchError(this.handleError));
}
```

### Issuing a POST request to the REST API
```js
/* ngOnInit */
this.route.paramMap.subscribe(params => {
  const empId = +params.get('id');
  if (empId) {
    this.pageTitle = 'Edit Employee';
    this.getEmployee(empId);
  } else {
    this.pageTitle = 'Create Employee';
    this.employee = {
      id: null,
      fullName: '',
      contactPreference: '',
      email: '',
      phone: null,
      skills: []
    };
  }
});

/* onSubmit */
onSubmit(): void {
  this.mapFormValuesToEmployeeModel();

  if (this.employee.id) {
    this.employeeService.updateEmployee(this.employee).subscribe(
      () => this.router.navigate(['list']),
      (err: any) => console.log(err)
    );
  } else {
    this.employeeService.addEmployee(this.employee).subscribe(
      () => this.router.navigate(['list']),
      (err: any) => console.log(err)
    );
  }
}

/* addEmployee */
addEmployee(employee: IEmployee): Observable<IEmployee> {
    return this.httpClient.post<IEmployee>(this.baseUrl, employee, {
        headers: new HttpHeaders({
            'Content-Type': 'application/json'
        })
    })
    .pipe(catchError(this.handleError));
}

/* html */
<div class="panel-heading">
  <h3 class="panel-title">{{pageTitle}}</h3>
</div>
```
