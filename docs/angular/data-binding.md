# Data Binding

## 📖 What is Data Binding?

Data Binding is the mechanism that synchronizes data between the **Component (TypeScript)** and the **Template (HTML)**.

It allows Angular to automatically update the UI when the component data changes and update the component when the user interacts with the UI.

Without Data Binding, developers would have to manually update the DOM using JavaScript.

---

# Why Do We Need Data Binding?

Suppose we have:

```typescript
username = "Jyothi";
```

Without Angular Data Binding:

```javascript
document.getElementById("name").innerText = username;
```

Every time `username` changes, we must manually update the DOM.

With Angular:

```html
<h2>{{ username }}</h2>
```

Angular automatically updates the UI whenever `username` changes.

---

# How Data Binding Works

```text
              Component (TypeScript)
                    │
         -------------------------
         │                       │
         ▼                       ▼
    Component Data          Component Events
         │                       ▲
         ▼                       │
            Angular Data Binding
         ▲                       │
         │                       ▼
      HTML Template (View)
```

Angular keeps the Component and Template synchronized.

---

# Types of Data Binding

Angular supports **four types of Data Binding**.

| Binding Type | Direction | Syntax |
|--------------|-----------|--------|
| Interpolation | Component ➜ View | `{{ value }}` |
| Property Binding | Component ➜ View | `[property]="value"` |
| Event Binding | View ➜ Component | `(event)="method()"` |
| Two-Way Binding | Component ⇄ View | `[(ngModel)]="value"` |

---

# 1. Interpolation

## Purpose

Interpolation displays data from the component in the template.

### Syntax

```html
{{ expression }}
```

Example

```typescript
name = "Jyothi";
```

```html
<h1>{{ name }}</h1>
```

Output

```
Jyothi
```

---

## What Can We Use?

Variables

```html
{{ name }}
```

Expressions

```html
{{ 10 + 20 }}
```

Function calls

```html
{{ getFullName() }}
```

Ternary operators

```html
{{ isAdmin ? 'Admin' : 'User' }}
```

---

## Interview Question

### Can we write loops inside interpolation?

No.

Interpolation only supports expressions.

This is invalid:

```html
{{ for(...) }}
```

---

## Best Practices

✔ Simple expressions only

❌ Heavy calculations

❌ API calls

❌ Long-running functions

---

# 2. Property Binding

## Purpose

Property Binding sends data from the component to an HTML element property.

### Syntax

```html
[property]="expression"
```

Example

```typescript
imageUrl = "assets/profile.png";
```

```html
<img [src]="imageUrl">
```

Angular updates the src property whenever imageUrl changes.

---

## More Examples

Disable button

```typescript
isDisabled = true;
```

```html
<button [disabled]="isDisabled">
```

Set placeholder

```html
<input [placeholder]="username">
```

Dynamic class

```html
<div [class.active]="isActive">
```

Dynamic style

```html
<div [style.color]="textColor">
```

---

## Attribute Binding vs Property Binding

Property Binding

```html
<input [disabled]="true">
```

Attribute Binding

```html
<input [attr.colspan]="2">
```

### Interview Tip

Property Binding updates DOM properties.

Attribute Binding updates HTML attributes.

---

# 3. Event Binding

## Purpose

Event Binding allows the template to notify the component when an event occurs.

### Syntax

```html
(event)="method()"
```

Example

```html
<button (click)="save()">
```

Component

```typescript
save(){
   console.log("Saved");
}
```

---

## Common Events

- click
- input
- change
- blur
- focus
- keyup
- keydown
- submit

---

## Passing Event Object

```html
<input (input)="onInput($event)">
```

```typescript
onInput(event:any){
   console.log(event.target.value);
}
```

---

## Real Project Example

Search box

```html
<input
    (keyup)="searchEmployees($event)">
```

---

# 4. Two-Way Data Binding

## Purpose

Synchronizes data between the component and the view.

Changes in either place automatically update the other.

### Syntax

```html
[(ngModel)]="username"
```

Example

```typescript
username = "";
```

```html
<input [(ngModel)]="username">

<h3>{{ username }}</h3>
```

Typing inside the input automatically updates the component.

Updating the component automatically updates the input.

---

## Internal Working

Two-Way Binding is a combination of:

Property Binding

```html
[value]="username"
```

+

Event Binding

```html
(input)="username=$event.target.value"
```

Equivalent to

```html
[(ngModel)]="username"
```

---

## Requirement

Must import

```typescript
FormsModule
```

Otherwise Angular throws:

```
Can't bind to 'ngModel'
```

---

# One-Way vs Two-Way Binding

| One-Way | Two-Way |
|----------|----------|
| Component → View | Component ⇄ View |
| Faster | Slightly more overhead |
| Preferred for display | Used in forms |

---

# Real Project Example

Employee Search

```typescript
searchText = "";
```

```html
<input [(ngModel)]="searchText">

<table>

<tr *ngFor="let emp of employees">

```

User types

↓

Component updates

↓

Filter runs

↓

Table refreshes automatically

---

# Data Binding Flow

```text
Component
    │
    ▼
Data Binding
    │
    ▼
HTML View

User Interaction
    │
    ▼
Event Binding
    │
    ▼
Component Updated
    │
    ▼
Change Detection
    │
    ▼
Updated UI
```

---

# Frequently Asked Interview Questions

## What is Data Binding?

Data Binding synchronizes data between the Component and the View.

---

## How many types of Data Binding are there?

Four

- Interpolation
- Property Binding
- Event Binding
- Two-Way Binding

---

## Difference between Interpolation and Property Binding

| Interpolation | Property Binding |
|--------------|------------------|
| Displays text | Sets DOM properties |
| `{{ }}` | `[ ]` |
| Mostly for text | Used for HTML properties |

---

## Difference between Property Binding and Event Binding

| Property Binding | Event Binding |
|------------------|--------------|
| Component → View | View → Component |
| `[ ]` | `( )` |

---

## What is Two-Way Binding?

Two-Way Binding keeps the component property and UI synchronized.

Implemented using:

```
[(ngModel)]
```

---

## How does Two-Way Binding work internally?

It combines:

```
[value]
```

and

```
(input)
```

---

## Why is FormsModule required?

Because `ngModel` belongs to FormsModule.

---

# Common Interview Mistakes

❌ Using interpolation inside HTML attributes

```html
<img src="{{image}}">
```

Preferred

```html
<img [src]="image">
```

---

❌ Forgetting FormsModule

---

❌ Writing heavy functions inside interpolation

```html
{{ calculateSalary() }}
```

Runs during every change detection cycle.

---

❌ Confusing Property Binding with Attribute Binding

---

# Key Takeaways

- Data Binding synchronizes the Component and Template.
- Angular provides four types of Data Binding.
- Interpolation displays text.
- Property Binding updates DOM properties.
- Event Binding handles user interactions.
- Two-Way Binding synchronizes component and view.
- `[(ngModel)]` combines Property Binding and Event Binding.
- Avoid expensive function calls inside templates because they execute during every change detection cycle.
- Always default to property binding `[property]`. Only switch to attribute binding `[attr.attribute]` if Angular throws an error stating that the property is not recognized