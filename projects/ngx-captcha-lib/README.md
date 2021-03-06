[![npm version](https://badge.fury.io/js/ngx-captcha.svg)](https://badge.fury.io/js/ngx-captcha)
[![Build Status](https://api.travis-ci.org/niteshpurohit/ngx-captcha.svg?branch=master)](https://travis-ci.org/niteshpurohit/ngx-captcha)
[![NPM](https://nodei.co/npm/ngx-captcha.png?mini=true)](https://nodei.co/npm/ngx-captcha/)

## Angular Captcha

Google reCaptcha implementation for Angular 6 and beyond.

Features:

1. reCaptcha v2
2. reCaptcha v3 (beta)
3. invisible reCaptcha

Live examples: [https://niteshpurohit.github.io/ngx-captcha/](https://niteshpurohit.github.io/ngx-captcha/)

## Supported versions

1. For Angular 6 use ngx-captcha on version `5.0.4`
2. For Angular 7 use ngx-captcha on version `>= 6.0.0`

## Installation

```javascript
npm install ngx-captcha
```

Import `NgxCaptchaModule ` to your module (i.e. `AppModule`).

### Use with Angular forms

Depending on whether you want to use [reactive forms](https://angular.io/guide/reactive-forms) or [template-driven forms](https://angular.io/guide/forms) you need to include the appropriate modules, too.
Add `ReactiveFormsModule` to your imports in case you want to use reactive forms. If you prefer the the template-driven approach simple add the `FormsModule` to your module.

Both can be imported from `@angular/forms`. In the demo project you can check out the _normal_ demo to see how to use reactive forms or the _invisible_ demo to see what the template-driven approach looks like. With the template-driven approach you don't necessarily need to use a from element to wrap the component, you can instead use the `[ngModelOptions]="{ standalone: true }"`.
However, using it with the standalone option is not recommended but can be used if needed.

```javascript
import { NgModule } from '@angular/core';
import { ReactiveFormsModule } from '@angular/forms';
import { NgxCaptchaModule } from 'ngx-captcha';

@NgModule({
  imports: [
    ReactiveFormsModule,
    NgxCaptchaModule
  })

export class AppModule { }
```

## Usage

The configuration properties are a copy of the official ones that google provides. For explanation of what these properties do and how to use them, please refer to [https://developers.google.com/recaptcha/docs/display](https://developers.google.com/recaptcha/docs/display) and [https://developers.google.com/recaptcha/docs/invisible](https://developers.google.com/recaptcha/docs/invisible).

### reCaptcha v2

your.component.ts

```typescript
export class YourComponent implements OnInit {
  protected aFormGroup: FormGroup;

  constructor(private formBuilder: FormBuilder) {}

  ngOnInit() {
    this.aFormGroup = this.formBuilder.group({
      recaptcha: ["", Validators.required],
    });
  }
}
```

your.template.html

```html
<form [formGroup]="aFormGroup">
  <ngx-recaptcha2
    #captchaElem
    [siteKey]="siteKey"
    (reset)="handleReset()"
    (expire)="handleExpire()"
    (load)="handleLoad()"
    (success)="handleSuccess($event)"
    [useGlobalDomain]="false"
    [size]="size"
    [hl]="lang"
    [theme]="theme"
    [type]="type"
    formControlName="recaptcha"
  >
  </ngx-recaptcha2>
</form>
```

### reCaptcha v3

This is the implementation of <em>beta</em> version of google reCaptcha v3 as per following documentation["https://developers.google.com/recaptcha/docs/v3"](https://developers.google.com/recaptcha/docs/v3).

First you need to inject the <em></em> class in your component / service and then use <em>Execute</em> method with your action. Once you have the token, you need to verify it on your backend to get meaningful results. See official google documentation for more details.

```typescript
import { ReCaptchaV3Service } from 'ngx-captcha';

 constructor(
   private reCaptchaV3Service: ReCaptchaV3Service
 ) { }

 ....

 this.reCaptchaV3Service.execute(this.siteKey, 'homepage', (token) => {
   console.log('This is your token: ', token);
 }, {
     useGlobalDomain: false
 });
```

### Invisible reCaptcha

```html
<form [formGroup]="aFormGroup">
  <ngx-invisible-recaptcha
    #captchaElem
    [siteKey]="siteKey"
    (reset)="handleReset()"
    (ready)="handleReady()"
    (load)="handleLoad()"
    (success)="handleSuccess($event)"
    [useGlobalDomain]="false"
    [type]="type"
    [badge]="badge"
    [ngModel]="recaptcha"
    [ngModelOptions]="{ standalone: true }"
  >
  </ngx-invisible-recaptcha>
</form>
```

### Publishing lib

Under `projects\ngx-captcha-lib` run

```
npm run publish-lib
```

### Publishing demo app

Under root, generate demo app with

```
npm run build-demo-gh-pages
npx ngh --dir=dist-demo
```
