<base target="_blank">
# Angular - Ionic
[Ionic](https://ionicframework.com/) is an open source mobile UI toolkit for building modern, high quality cross-platform mobile apps from a single code base in Angular and React. This document focuses on [Angular](https://angular.io/guide/what-is-angular) which is a TypeScript-based, free and open-source web application framework. 
>[TypeScript](https://www.typescriptlang.org/) adds additional syntax to JavaScript to support a **tighter integration with your editor**. TypeScript code converts to JavaScript, which **runs anywhere JavaScript runs**: In a browser, on Node.js and other things not in this document :) 

## Setting up the Environment 
1. Install [node.js](https://nodejs.org/en) 
> Download and install the LTS (Long Term Support) version as it's the most stable and is least likely to cause problems. 
2. Open Command prompt.
3. Run `npm install -g @angular/cli` to install Angular.
4. Run `npm install -g @ionic/cli` to install Ionic.
5. Create a new folder that will contain the project.
6. Open the project folder in Visual Studio Code. 
7. Open a New Terminal in Visual Studio Code. 
8. Add a command prompt terminal using the `+` icon on top right side of the terminal window.
9. Run `$ ionic start [name] [template] [options]` to create your PWA (Progressive Web Application).
>name - Name of your project.

|Templates    | description |
| ----- | ----------- |
| tabs         | A starting project with a simple tabbed interface |
| sidemenu     | A starting project with a side menu with navigation in the content area |
| blank        | A blank starter project |
| list         | A starting project with a list |
| my-first-app | A template for the "Build Your First App" tutorial |

Personal Preference.
> `ionic start myApp blank`.

10. Create a page or service.
> `ionic -g [option] [pagePath/pageName]`.

| -g Options   | description |
| ----- | ----------- |
| page         | A page of your choice |
| component    | A component that can be used anywhere in the project |
| directive    | A component but modified (I need to do more research on this) |
| service      | A service for functionality or using APIs |

## Using pre-created Environment 
A pre-created Environment should have a `package.json` file in the root folder. This file contains the project dependencies, make sure it exists.
1. Open a New Terminal in Visual Studio Code.
2. Add a command prompt terminal using the `+` icon on top right side of the terminal window.
3. Navigate to the project root using `cd ./example-root-folder/` 
4. Run `npm install` to install the project dependencies.
5. Run `ionic build` to build the ionic framework.
6. Run `ionic serve` to run the project in your browser using localhost.

## Building Android App
**THIS SECTION IS STILL IN PROGRESS**
Make sure you have a working Environment.


# Tips I learned along the way

## FormBuilder | FormGroup

Import `ReactiveFormsModule` into the [pageName].module.ts of the page you want to use the forms on.

<u>Example:</u>
```
import { FormsModule, ReactiveFormsModule } from '@angular/forms';
...
@NgModule({
  imports: [
    ...
    FormsModule,
    ReactiveFormsModule,
    ...
  ],
  declarations: [RegisterPage]
})

export class RegisterPageModule {}
```

pageName.page.ts

<u>Example:</u>
```
Import { FormBuilder, FormGroup, Validators } from '@angular/forms';
...
protected form: FormGroup;
protected formSubmitted = false;

constructor(private formBuilder: FormBuilder,) { 

  this.form = this.formBuilder.group({
    userName: [undefined, [Validators.required]],
    password: [undefined, [Validators.required]]
  });
}

get f() {
  return this.form.controls;
}

get formValid() {
  return this.form.valid;
}

get email() {
  return this.f.email;
}

get password() {
  return this.f.password;
}

async ionViewWillEnter() {
  this.form.reset();
}
```

If dot notation doesn't work, setting `"noPropertyAccessFromIndexSignature"` to false in `tsconfig.json` will fix that, otherwise if this makes you uncomfortable - doing the following will work.

```
get password() {
  return this.f[password];
}
```

Here is a quick form snippet

```
<form [formGroup]="form">
    <ion-grid>
      <ion-row class="ion-align-items-center ion-justify-content-center">
        <ion-col size-sm="12" size-md="8" size-lg="6">
          <div style="text-align: center">
            <h2>Login</h2>
          </div>
          <ion-item>
            <ion-label position="floating">Email</ion-label>
            <ion-input class="form-control" name="email" type="text" formControlName="email" required>
            </ion-input>
          </ion-item>
          <span *ngIf="(email.touched) && email.errors" class="validation-errors">
            Please provide a valid email.
          </span>
          <ion-item>
            <ion-label position="floating">Password</ion-label>
            <ion-input class="form-control" name="password" type="text" formControlName="password" required>
            </ion-input>
          </ion-item>
          <span *ngIf="(password.touched) && password.errors?.minlength" class="validation-errors">Password is
            required</span>
        </ion-col>
      </ion-row>
      <ion-row class="ion-align-items-center ion-justify-content-center">
        <ion-col size-sm="12" size-md="8" size-lg="6">
          <ion-button expand="block" [disabled]="!formValid || formSubmitting" color="primary"
            (click)="login()">Login</ion-button>
        </ion-col>
      </ion-row>
  </ion-grid>
</form>  
```
