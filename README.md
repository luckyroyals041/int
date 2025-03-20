# 1. Displaying a List of Courses

This Angular application demonstrates the creation of a simple component that displays a list of courses using the `*ngFor` structural directive.

## Steps to Create the Application

1. **Create a New Angular Application**  
    Use the Angular CLI command to create a new application:
    ```bash
    ng new app1
    cd app1
    ```

2. **Update `app.component.html`**  
    Add the following code to the `app.component.html` file:
    ```html
    <h2>Courses</h2>
    <ol>
         <li *ngFor="let course of courses">
              {{ course }}
         </li>
    </ol>
    ```

3. **Update `app.component.ts`**  
    Add the following code to the `app.component.ts` file:
    ```typescript
    import { Component } from '@angular/core';
    import { CommonModule } from '@angular/common';

    @Component({
         selector: 'app-root',
         imports: [CommonModule],
         templateUrl: './app.component.html',
         styleUrls: ['./app.component.css']
    })
    export class AppComponent {
         title = 'app1';
         courses = ['course1', 'course2', 'course3'];
    }
    ```

## Output

When you run the application, it will display a list of courses (`course1`, `course2`, `course3`) in an ordered list.


# 2. Employee Registration Form

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
    ng new app2 --no-standalone
    ```
2. Navigate to the project directory:
    ```bash
    cd app2
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


# 3. Style Encapsulation Demo

This project demonstrates the use of Angular's `ViewEncapsulation` options to control style isolation in components. It includes two components: one using Shadow DOM encapsulation and another with no encapsulation. The differences in style behavior are highlighted through a simple example.

## Project Setup

1. Create a new Angular application:
     ```bash
     $ ng new app3
     $ cd app3
     ```

2. Generate two components:
     ```bash
     ng generate component shadow-box
     ng generate component no-shadow-box
     ```

## File to be Update
1. **ShadowBox Component**
- **File**: `shadow-box.component.ts`
```typescript
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-shadow-box',
  template: `
    <div class="box">
      <h2>Shadow DOM Box</h2>
      <p>This text is blue and styles won't leak!</p>
    </div>
  `,
  styles: [`
    .box {
      border: 2px solid blue;
      padding: 20px;
      margin: 10px;
    }
    p {
      color: blue;
    }
  `],
  encapsulation: ViewEncapsulation.ShadowDom
})
export class ShadowBoxComponent { }

```
- **Encapsulation**: `ViewEncapsulation.ShadowDom`
- **Behavior**:
  - Styles are encapsulated within the component using Shadow DOM.
  - Blue styles are applied only to this component and do not affect other parts of the application.

2. **NoShadowBox Component**
- **File**: `no-shadow-box.component.ts`
```typescript
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-no-shadow-box',
  template: `
    <div class="box">
      <h2>No Encapsulation Box</h2>
      <p>This text is red and styles will leak!</p>
    </div>
  `,
  styles: [`
    .box {
      border: 2px solid red;
      padding: 20px;
      margin: 10px;
    }
    p {
      color: red;
    }
  `],
  encapsulation: ViewEncapsulation.None
})
export class NoShadowBoxComponent { }

```
- **Encapsulation**: `ViewEncapsulation.None`
- **Behavior**:
  - Styles are not encapsulated and will leak to other parts of the application.
  - Red styles affect any matching elements globally.
3. **App Component**:
     - **File**: `app.component.html`
     ```html
     <h1>Style Encapsulation Demo</h1>
     
     <app-shadow-box></app-shadow-box>
     <app-no-shadow-box></app-no-shadow-box>
     
     <!-- Test div to show style leaking -->
     <div class="box">
       <h2>Regular Box</h2>
       <p>This text will be affected by NoEncapsulation styles</p>
     </div>
     
     ```
     - Includes both `ShadowBox` and `NoShadowBox` components.
     - Contains a regular box to demonstrate style leakage from the `NoShadowBox` component.

4. **App Module**:
     - **File**: `app.module.ts`
     ```typescript
     import { NgModule } from '@angular/core';
     import { BrowserModule } from '@angular/platform-browser';
     import { AppComponent } from './app.component';
     import { ShadowBoxComponent } from './shadow-box/shadow-box.component';
     import { NoShadowBoxComponent } from './no-shadow-box/no-shadow-box.component';
     
     @NgModule({
       declarations: [
         AppComponent,
         ShadowBoxComponent,
         NoShadowBoxComponent
       ],
       imports: [
         BrowserModule
       ],
       bootstrap: [AppComponent]
     })
     export class AppModule { }
     
     ```
     - Declares and imports the components.

## Running the Application

1. Start the development server:
     ```bash
     $ ng serve
     ```

2. Open the application in your browser at `http://localhost:4200`.

## Key Observations

1. **ShadowBox Component**:
     - Uses Shadow DOM encapsulation (`ViewEncapsulation.ShadowDom`).
     - Blue styles remain isolated within the component.
     - Other components are not affected by these styles.

2. **NoShadowBox Component**:
     - Uses no encapsulation (`ViewEncapsulation.None`).
     - Red styles leak to other components and affect any matching elements in the application.

3. **Regular Box**:
     - Demonstrates the effect of style leakage from the `NoShadowBox` component.
     - The text and border turn red due to the leaked styles.

## Inspecting the DOM

- Use browser developer tools to inspect the elements.
- Observe the Shadow DOM boundary for the `ShadowBox` component.
- Notice the absence of encapsulation for the `NoShadowBox` component.

## Summary

This project highlights the differences between Angular's style encapsulation options:
- **Shadow DOM (`ViewEncapsulation.ShadowDom`)**: Ensures style isolation within the component.
- **No Encapsulation (`ViewEncapsulation.None`)**: Allows styles to leak and affect other components.

By comparing the behavior of these two components, you can better understand how to manage style isolation in Angular applications.
