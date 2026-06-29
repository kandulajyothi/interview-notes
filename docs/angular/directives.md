# Directives

## 📖 What are Directives?

Directives are classes in Angular that allow us to **change the behavior or appearance of DOM elements**.

They are one of Angular's core building blocks.

Without directives, HTML remains static. Directives make HTML dynamic by adding behavior, modifying styles, or manipulating the DOM.

Angular provides built-in directives and also allows developers to create custom directives.

---

# Why Do We Need Directives?

Suppose you want to:

- Show or hide a section
- Loop through a list
- Apply dynamic styles
- Highlight a field
- Disable an element
- Add hover effects

Without directives, you would manually manipulate the DOM using JavaScript.

Angular directives make these tasks declarative and reusable.

---

# Types of Directives

Angular has **three types** of directives.

| Directive Type | Purpose | Example |
|---------------|---------|---------|
| Component Directive | Creates a view | `@Component` |
| Structural Directive | Adds or removes elements | `*ngIf`, `*ngFor`, `*ngSwitch` |
| Attribute Directive | Changes appearance or behavior | `ngClass`, `ngStyle`, Custom Directive |

---

# Directive Hierarchy

```text
Directive
     │
     ├──────────────┐
     │              │
Component     Attribute Directive
     │
Structural Directive
```

Every Component is actually a Directive with a template.

---

# 1. Component Directive

A Component is a special type of directive.

Unlike other directives, it has:

- HTML Template
- CSS
- TypeScript
- View

Example

```typescript
@Component({
    selector:'app-home',
    templateUrl:'./home.component.html'
})
export class HomeComponent{}
```

---

# 2. Structural Directives

## What are Structural Directives?

Structural directives **change the DOM structure**.

They can:

- Create elements
- Remove elements
- Repeat elements

Structural directives always begin with **`*`**.

---

## Why `*`?

The `*` is syntactic sugar.

Angular internally converts:

```html
<div *ngIf="isAdmin">
```

into

```html
<ng-template [ngIf]="isAdmin">
    <div></div>
</ng-template>
```

---

# ngIf

## Purpose

Displays an element only if a condition is true.

Example

```typescript
isLoggedIn = true;
```

```html
<div *ngIf="isLoggedIn">
    Welcome User
</div>
```

Output

```
Welcome User
```

If false

The element is completely removed from the DOM.

---

## ngIf with else

```html
<div *ngIf="isAdmin; else guest">

Admin

</div>

<ng-template #guest>

Guest User

</ng-template>
```

---

## Interview Question

Difference between

```
*ngIf
```

and

```
display:none
```

### ngIf

- Removes DOM element

### display:none

- Element remains in DOM
- Only hidden

---

# ngFor

## Purpose

Repeats an element for every item in a collection.

Example

```typescript
employees = [
    'John',
    'Mary',
    'David'
];
```

```html
<li *ngFor="let emp of employees">

{{emp}}

</li>
```

Output

```
John

Mary

David
```

---

## Useful Variables

```html
<li *ngFor="let emp of employees;
             index as i;
             first as isFirst;
             last as isLast;
             even as isEven;
             odd as isOdd">
```

---

## trackBy (Very Important)

Without trackBy

Angular recreates every DOM element.

With trackBy

Angular updates only changed rows.

Example

```typescript
trackById(index:number,item:any){

return item.id;

}
```

```html
<tr *ngFor="let emp of employees;trackBy:trackById">
```

---

## Interview Question

Why use trackBy?

Answer

Improves performance by preventing unnecessary DOM recreation.

---

# ngSwitch

Used when multiple conditions exist.

```typescript
status='Approved';
```

```html
<div [ngSwitch]="status">

<div *ngSwitchCase="'Approved'">

Approved

</div>

<div *ngSwitchCase="'Rejected'">

Rejected

</div>

<div *ngSwitchDefault>

Pending

</div>

</div>
```

---

# Attribute Directives

Attribute directives modify an existing HTML element.

They **do not create or remove elements**.

---

# ngClass

Applies CSS classes dynamically.

Example

```typescript
isActive=true;
```

```html
<div [ngClass]="{'active':isActive}">

```

Multiple Classes

```html
<div [ngClass]="{

'active':isActive,

'disabled':isDisabled

}">
```

---

# ngStyle

Applies styles dynamically.

```typescript
color='red';
```

```html
<div [ngStyle]="{

'color':color,

'font-size':'20px'

}">
```

---

# Custom Directives

## Why Create Custom Directives?

Suppose every input field should:

- Highlight on hover
- Change border color
- Display a shadow

Instead of repeating code everywhere, create a reusable directive.

---

# Creating a Custom Directive

## Requirement

When the mouse enters an element:

- Background becomes yellow
- Text becomes black

When the mouse leaves:

- Original style returns

---

## Step 1

Generate Directive

```bash
ng generate directive directives/highlight
```

Angular creates

```
highlight.directive.ts
```

---

## Step 2

Directive Code

```typescript
import {
    Directive,
    ElementRef,
    HostListener,
    Renderer2
} from '@angular/core';

@Directive({
    selector:'[appHighlight]'
})

export class HighlightDirective{

constructor(

private element:ElementRef,

private renderer:Renderer2

){}

@HostListener('mouseenter')

onMouseEnter(){

this.renderer.setStyle(

this.element.nativeElement,

'background',

'yellow'

);

this.renderer.setStyle(

this.element.nativeElement,

'color',

'black'

);

}

@HostListener('mouseleave')

onMouseLeave(){

this.renderer.removeStyle(

this.element.nativeElement,

'background'

);

this.renderer.removeStyle(

this.element.nativeElement,

'color'

);

}

}
```

---

## Step 3

Use Directive

```html
<h2 appHighlight>

Angular Interview Notes

</h2>

<button appHighlight>

Save

</button>
```

Hovering over any element automatically applies the styles.

---

# How It Works

```text
User moves mouse
       │
       ▼
HostListener detects event
       │
       ▼
Directive executes
       │
       ▼
Renderer2 updates DOM
       │
       ▼
UI changes
```

---

# Better Custom Directive Example

Let's make it reusable by allowing the highlight color to be passed as an input.

```typescript
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {

  @Input() appHighlight = 'yellow';

  constructor(
    private element: ElementRef,
    private renderer: Renderer2
  ) {}

  @HostListener('mouseenter')
  onMouseEnter() {
    this.renderer.setStyle(
      this.element.nativeElement,
      'backgroundColor',
      this.appHighlight
    );
  }

  @HostListener('mouseleave')
  onMouseLeave() {
    this.renderer.removeStyle(
      this.element.nativeElement,
      'backgroundColor'
    );
  }
}
```

Usage

```html
<p appHighlight>
  Default Yellow
</p>

<p [appHighlight]="'lightgreen'">
  Green Highlight
</p>

<p [appHighlight]="'lightblue'">
  Blue Highlight
</p>
```

This is much more reusable than hardcoding the color.

---

# Why Renderer2?

Instead of

```typescript
element.nativeElement.style.background='yellow';
```

Angular recommends

```typescript
Renderer2
```

because:

- Safe DOM manipulation
- Better security
- Works with Server-Side Rendering (SSR)
- Platform independent

---

# Real Project Example

In an Employee Management System:

Create a directive that highlights overdue records.

```html
<tr
    [appHighlight]="employee.isExpired ? 'salmon' : 'white'">
```

Or create a directive to disable buttons based on permissions:

```html
<button appRolePermission="ADMIN">
    Delete Employee
</button>
```

The directive can check the user's role and enable or disable the button automatically. This avoids repeating permission logic across multiple components.

---

# Frequently Asked Interview Questions

## What are Directives?

Directives are classes that modify the behavior or appearance of DOM elements.

---

## How many types of directives are there?

- Component Directive
- Structural Directive
- Attribute Directive

---

## Difference between Structural and Attribute Directives

| Structural | Attribute |
|------------|-----------|
| Changes DOM structure | Changes appearance or behavior |
| Uses `*` | No `*` |
| Creates/removes elements | Modifies existing elements |

---

## Why do Structural Directives use `*`?

Because Angular rewrites them into an `<ng-template>` behind the scenes.

---

## Difference between ngIf and hidden/display:none

| ngIf | display:none |
|------|---------------|
| Removes element from DOM | Keeps element in DOM |
| Better for conditional rendering | Only hides the element |

---

## Why use trackBy with ngFor?

To improve rendering performance by reusing existing DOM elements instead of recreating them.

---

## Why use Renderer2 instead of ElementRef?

Renderer2 provides a safe and platform-independent way to manipulate the DOM.

---

# Common Interview Mistakes

❌ Using both `*ngIf` and `*ngFor` on the same HTML element.

```html
<!-- Invalid -->
<div *ngIf="show" *ngFor="let emp of employees"></div>
```

Instead, wrap one directive in an `<ng-container>`.

```html
<ng-container *ngIf="show">
  <div *ngFor="let emp of employees">
    {{ emp.name }}
  </div>
</ng-container>
```

---

❌ Forgetting `trackBy` for large lists.

---

❌ Manipulating the DOM directly using `nativeElement.style`.

Prefer `Renderer2`.

---

# Coding Practice (Highly Recommended)

Implement these custom directives yourself:

1. **Highlight Directive** – Change background color on hover.
2. **Auto Focus Directive** – Automatically focus an input field.
3. **Uppercase Directive** – Convert input text to uppercase while typing.
4. **Only Numbers Directive** – Allow only numeric input.
5. **Permission Directive** – Show or hide elements based on user roles.
6. **Tooltip Directive** – Display a custom tooltip on hover.

These are excellent interview coding exercises and closely resemble tasks you may encounter in real Angular projects.

---

# Key Takeaways

- Directives extend HTML functionality.
- Angular provides three directive types.
- Structural directives modify the DOM.
- Attribute directives modify existing elements.
- Components are specialized directives with templates.
- Use `trackBy` with `*ngFor` for better performance.
- Use `Renderer2` instead of directly manipulating the DOM.
- Custom directives help eliminate duplicate UI behavior and improve code reusability.