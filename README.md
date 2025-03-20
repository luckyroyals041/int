# Employee Registration Form in Angular

This documentation provides step-by-step instructions for creating an Angular application with an employee registration form using reactive forms.

## Prerequisites

Ensure you have Angular CLI installed. If not, install it using:

```bash
npm install -g @angular/cli
```

## Steps to Create the Application

### 1. Project Setup

1. Create a new Angular project:
    ```bash
    ng new <app_name> --no-standalone
    ```
2. Navigate to the project directory:
    ```bash
    cd <app_name>
    ```
3. Generate a new component named `employee-registration`:
    ```bash
    ng generate component employee-registration
    ```
    or use the shorthand:
    ```bash
    ng g c employee-registration
    ```

### 2. Component Implementation

1. Open the `employee-registration.component.ts` file and add the following code:

    ```typescript
    import { Component } from '@angular/core';
    import { FormBuilder, FormGroup, Validators, AbstractControl } from '@angular/forms';

    @Component({
      selector: 'app-employee-registration',
      standalone: false,
      templateUrl: './employee-registration.component.html',
      styleUrls: ['./employee-registration.component.css']
    })
    export class EmployeeRegistrationComponent {
      employeeForm: FormGroup;

      constructor(private fb: FormBuilder) {
         this.employeeForm = this.fb.group({
            email: ['', [Validators.required, this.emailValidator]]
         });
      }

      emailValidator(control: AbstractControl) {
         if (!control.value.includes('@')) {
            return { invalidEmail: true };
         }
         return null;
      }

      onSubmit() {
         if (this.employeeForm.valid) {
            console.log(this.employeeForm.value);
         }
      }
    }
    ```

### 3. Template Setup

1. Open the `employee-registration.component.html` file and add the following code:

    ```html
    <form [formGroup]="employeeForm" (ngSubmit)="onSubmit()">
      <div>
         <label for="email">Email:</label>
         <input type="email" id="email" formControlName="email">
         
         <div *ngIf="employeeForm.get('email')?.touched">
            <div *ngIf="employeeForm.get('email')?.errors?.['required']">
              Email is required
            </div>
            <div *ngIf="employeeForm.get('email')?.errors?.['invalidEmail']">
              Please include &#64; in email
            </div>
         </div>
      </div>
    
      <button type="submit" [disabled]="!employeeForm.valid">Submit</button>
    </form>
    ```

### 4. Module Configuration

1. Open the `app.module.ts` file and add the following code:

    ```typescript
    import { NgModule } from '@angular/core';
    import { BrowserModule, provideClientHydration, withEventReplay } from '@angular/platform-browser';
    import { ReactiveFormsModule } from '@angular/forms';
    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { EmployeeRegistrationComponent } from './employee-registration/employee-registration.component';

    @NgModule({
      declarations: [
         AppComponent,
         EmployeeRegistrationComponent
      ],
      imports: [
         BrowserModule,
         AppRoutingModule,
         ReactiveFormsModule
      ],
      providers: [
         provideClientHydration(withEventReplay())
      ],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

### 5. Application Integration

1. Open the `app.component.html` file and add the following code:

    ```html
    <app-employee-registration></app-employee-registration>
    ```

## Summary

By following these steps, you will create a functional employee registration form with email validation in an Angular application. This form uses reactive forms to handle validation and submission effectively.
