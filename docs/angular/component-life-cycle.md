# Component Lifecycle Hooks

## 📖 What are Lifecycle Hooks?

A component lifecycle is the sequence of stages an Angular component goes through from its creation to its destruction.

Angular provides **Lifecycle Hooks**, which are special methods that allow developers to execute custom logic at different stages of a component's life.

Common use cases include:

- Initializing data
- Responding to input changes
- Accessing child components
- Performing custom change detection
- Cleaning up subscriptions

---

# Component Lifecycle Flow

```text
Component Created
       │
       ▼
constructor()
       │
       ▼
ngOnChanges()
       │
       ▼
ngOnInit()
       │
       ▼
ngDoCheck()
       │
       ▼
ngAfterContentInit()
       │
       ▼
ngAfterContentChecked()
       │
       ▼
ngAfterViewInit()
       │
       ▼
ngAfterViewChecked()
       │
       ▼
(Repeated Change Detection)
       │
       ▼
ngOnDestroy()
```

---

# Complete Lifecycle Order

| Order | Hook | Called Once? | Purpose |
|--------|------|--------------|---------|
| 1 | constructor | ✅ | Create component instance |
| 2 | ngOnChanges | ❌ | Detect @Input changes |
| 3 | ngOnInit | ✅ | Initialize component |
| 4 | ngDoCheck | ❌ | Custom change detection |
| 5 | ngAfterContentInit | ✅ | Content projection initialized |
| 6 | ngAfterContentChecked | ❌ | Projected content checked |
| 7 | ngAfterViewInit | ✅ | View initialized |
| 8 | ngAfterViewChecked | ❌ | View checked |
| 9 | ngOnDestroy | ✅ | Cleanup resources |

---

# 1. constructor()

## Purpose

The constructor is a TypeScript feature, **not an Angular lifecycle hook**.

Angular calls it when creating the component instance.

```typescript
constructor(private employeeService: EmployeeService) {}
```

## Use Cases

- Dependency Injection
- Simple property initialization

## Avoid

- API calls
- DOM manipulation
- Business logic

### Interview Tip

> The constructor is used only for object creation and dependency injection. Initialization logic should be placed in `ngOnInit()`.

---

# 2. ngOnChanges()

## Purpose

Called whenever an `@Input()` property changes.

It is also called before `ngOnInit()` during the first initialization.

```typescript
@Input() employee!: Employee;

ngOnChanges(changes: SimpleChanges) {
    console.log(changes);
}
```

## Use Cases

- Refresh child component data
- Compare previous and current values
- Trigger calculations

### Interview Question

**When is ngOnChanges called?**

**Answer**

- Before `ngOnInit()` (initial binding)
- Every time an `@Input()` value changes

---

# 3. ngOnInit()

## Purpose

Called once after Angular initializes all input properties.

```typescript
ngOnInit() {

}
```

## Common Uses

- API calls
- Initialize forms
- Load dropdown data
- Subscribe to Observables
- Set default values

### Real Project Example

```typescript
ngOnInit() {
   this.loadProjects();
   this.loadEnergyTypes();
}
```

### Interview Tip

Most initialization code belongs here.

---

# 4. ngDoCheck()

## Purpose

Called during every Angular change detection cycle.

Allows developers to implement custom change detection.

```typescript
ngDoCheck() {

}
```

## Use Cases

- Detect deep object changes
- Manual comparison

### Avoid

Heavy computations because this hook executes frequently.

### Interview Question

When would you use ngDoCheck?

Answer:

Only when Angular's default change detection is insufficient.

---

# 5. ngAfterContentInit()

## Purpose

Called after Angular projects external content using `<ng-content>`.

```html
<app-card>
    <h2>Hello</h2>
</app-card>
```

```html
<!-- app-card -->
<ng-content></ng-content>
```

## Use Cases

- Access projected content
- Initialize projected templates

---

# 6. ngAfterContentChecked()

Called every time projected content is checked.

Usually not used unless custom monitoring is required.

---

# 7. ngAfterViewInit()

## Purpose

Called after Angular initializes the component's view and child views.

Useful with:

- ViewChild
- ViewChildren

```typescript
@ViewChild('inputBox')
input!: ElementRef;

ngAfterViewInit() {
   this.input.nativeElement.focus();
}
```

## Use Cases

- Focus an input
- Initialize charts
- Access child components
- Integrate third-party libraries

### Interview Question

Why not access ViewChild inside ngOnInit?

Answer:

Because the view has not been created yet.

---

# 8. ngAfterViewChecked()

Called after every change detection cycle for the component's view.

Avoid heavy logic because it executes frequently.

---

# 9. ngOnDestroy()

## Purpose

Called just before Angular destroys the component.

```typescript
ngOnDestroy() {

}
```

## Common Uses

- Unsubscribe Observables
- Remove event listeners
- Stop timers
- Disconnect WebSockets

Example

```typescript
subscription!: Subscription;

ngOnDestroy() {
   this.subscription.unsubscribe();
}
```

### Interview Question

Why is ngOnDestroy important?

Answer:

To prevent memory leaks by cleaning up resources.

---

# Lifecycle Timeline

```text
User opens page
      │
      ▼
constructor()
      │
      ▼
ngOnChanges()
      │
      ▼
ngOnInit()
      │
      ▼
ngDoCheck()
      │
      ▼
ngAfterContentInit()
      │
      ▼
ngAfterContentChecked()
      │
      ▼
ngAfterViewInit()
      │
      ▼
ngAfterViewChecked()
      │
      ▼
User interacts with page
      │
      ▼
Change Detection
      │
      ▼
ngDoCheck()
ngAfterContentChecked()
ngAfterViewChecked()
      │
      ▼
User navigates away
      │
      ▼
ngOnDestroy()
```

---

# Real Project Example

Suppose your application has an **Employee List** page.

### constructor()

Inject services.

```typescript
constructor(private employeeService: EmployeeService) {}
```

### ngOnInit()

Load employee data.

```typescript
ngOnInit() {
   this.loadEmployees();
}
```

### ngOnChanges()

Reload data when selected department changes.

### ngAfterViewInit()

Focus the search box automatically.

### ngOnDestroy()

Unsubscribe from WebSocket updates.

---

# Frequently Asked Interview Questions

## What is the order of Angular Lifecycle Hooks?

```
constructor
↓
ngOnChanges
↓
ngOnInit
↓
ngDoCheck
↓
ngAfterContentInit
↓
ngAfterContentChecked
↓
ngAfterViewInit
↓
ngAfterViewChecked
↓
ngOnDestroy
```

---

## Difference between constructor and ngOnInit

| constructor | ngOnInit |
|-------------|----------|
| TypeScript feature | Angular lifecycle hook |
| Dependency Injection | Initialization logic |
| Called during object creation | Called after input properties are initialized |
| Avoid API calls | Best place for API calls |

---

## Difference between ngOnChanges and ngOnInit

| ngOnChanges | ngOnInit |
|--------------|----------|
| Runs whenever an `@Input()` changes | Runs only once |
| Receives `SimpleChanges` | No parameters |
| Can run multiple times | Runs once after first `ngOnChanges` |

---

## Which hooks run multiple times?

- ngOnChanges
- ngDoCheck
- ngAfterContentChecked
- ngAfterViewChecked

---

## Which hooks run only once?

- constructor
- ngOnInit
- ngAfterContentInit
- ngAfterViewInit
- ngOnDestroy

---

# Common Interview Mistakes

❌ Calling APIs inside the constructor.

❌ Accessing `@ViewChild` in `ngOnInit()`.

❌ Forgetting to unsubscribe in `ngOnDestroy()`.

❌ Performing expensive computations in `ngDoCheck()`.

❌ Confusing `ngOnChanges()` with `ngOnInit()`.

---

# Key Takeaways

- The constructor is for dependency injection and object creation.
- `ngOnInit()` is the preferred place for initialization logic and API calls.
- `ngOnChanges()` responds to changes in `@Input()` properties.
- `ngAfterViewInit()` is the right place to access `@ViewChild`.
- `ngOnDestroy()` prevents memory leaks by cleaning up resources.
- Understanding the order and purpose of lifecycle hooks is a common interview expectation for Angular developers.