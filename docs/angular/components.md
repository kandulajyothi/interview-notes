# Angular Components

Components are the basic building blocks of an Angular application.

---

## What is a Component?

A component controls a part of the user interface.

Example:

```typescript
@Component({
  selector: 'app-home',
  standalone: true,
  templateUrl: './home.component.html'
})
export class HomeComponent {}
```

---

## Important Interview Questions

### What are Components?

Components are reusable UI blocks consisting of:

- HTML Template
- TypeScript Class
- CSS Styles

---

## Lifecycle

- constructor()
- ngOnInit()
- ngOnChanges()
- ngOnDestroy()

!!! note

    ngOnInit executes once after Angular initializes the component.

---

## Key Points

- Components are reusable.
- One component should have one responsibility.
- Parent components communicate with child components using @Input and @Output.

