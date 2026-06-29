# Angular Architecture

## 📖 What is Angular Architecture?

Angular follows a **component-based architecture**, where an application is divided into reusable building blocks.

These building blocks include:

- Components
- Services
- Directives
- Pipes
- Dependency Injection
- Routing
- Modules / Standalone Components

Together, they help build scalable, maintainable, and testable Single Page Applications (SPA).

---

## 🏗️ High-Level Architecture

```text
Browser
   │
   ▼
index.html
   │
   ▼
main.ts
   │
   ▼
bootstrapApplication() / AppModule
   │
   ▼
AppComponent
   │
   ├──────────────┬──────────────┐
   ▼              ▼              ▼
Home         Dashboard      Login
   │
   ▼
Service
   │
   ▼
HttpClient
   │
   ▼
Backend API
```

---

## 🧩 Angular Building Blocks

| Building Block | Purpose |
|---------------|---------|
| Component | Builds UI |
| Service | Business logic & API calls |
| Directive | Changes DOM behavior |
| Pipe | Formats data |
| Routing | Navigation |
| Dependency Injection | Provides objects automatically |
| Module | Groups related functionality |

---

# Components

## What is a Component?

A component is the basic building block of Angular.

Every screen in Angular consists of one or more components.

Example:

```
App
│
├── Header
├── Sidebar
├── Product List
│      ├── Product Card
│      ├── Product Card
│      └── Product Card
└── Footer
```

Each component contains:

- HTML
- CSS / SCSS
- TypeScript
- `@Component` decorator

Example

```typescript
@Component({
  selector: 'app-home',
  templateUrl: './home.component.html'
})
export class HomeComponent {}
```

### Why Components?

- Reusable
- Easy to maintain
- Independent
- Easier testing

---

# Services

## Purpose

Services contain business logic.

❌ Bad

```
Component
   │
HTTP Call
```

✅ Good

```
Component
   │
Service
   │
HttpClient
   │
Backend
```

Example

```typescript
@Injectable({
  providedIn:'root'
})
export class EmployeeService {

   getEmployees(){
      return this.http.get('/employees');
   }

}
```

---

# Directives

Directives change the behavior of HTML.

## Structural Directives

- *ngIf
- *ngFor
- *ngSwitch

They add or remove DOM elements.

Example

```html
<div *ngIf="isLoggedIn">
```

---

## Attribute Directives

- ngClass
- ngStyle

They modify an existing element.

Example

```html
<!-- Legacy approach -->
<div [ngClass]="{ 'is-active': active }"></div>
<div [ngStyle]="{ 'background-color': statusColor }"></div>

<!-- Modern approach -->
<div [class.is-active]="active"></div>
<div [style.background-color]="statusColor"></div>
```

---

# Pipes

Pipes transform data before displaying it.

Example

```html
{{ salary | currency }}
```

Common Pipes

- date
- uppercase
- lowercase
- currency
- percent
- json

---

# Dependency Injection

Instead of creating objects manually,

```java
EmployeeService service = new EmployeeService();
```

Angular automatically injects them.

```typescript
constructor(private employeeService: EmployeeService){}
```

Benefits

- Loose coupling
- Easy testing
- Reusability

---

# Routing

Angular is a Single Page Application.

Routing maps URLs to components.

```
/home
     ↓
HomeComponent

/dashboard
      ↓
DashboardComponent
```

---

# Angular Application Flow

```text
User opens application
        │
        ▼
index.html
        │
        ▼
main.ts
        │
        ▼
Bootstrap Angular
        │
        ▼
AppComponent
        │
        ▼
Router
        │
        ▼
Component
        │
        ▼
Dependency Injection
        │
        ▼
Service
        │
        ▼
HttpClient
        │
        ▼
Backend API
        │
        ▼
Observable Response
        │
        ▼
Change Detection
        │
        ▼
UI Updated
```

---

# Interview Questions

### Explain Angular Architecture.

### Why is Angular component-based?

### Why should API calls be in Services?

### Explain the Angular application flow.

### Difference between Components and Services.

### Difference between Modules and Standalone Components.

---

# 2-Minute Interview Answer

> Angular follows a component-based architecture where the UI is divided into reusable components. Components are responsible for presentation, while services contain business logic and API communication. Angular uses Dependency Injection to provide services automatically. Routing enables navigation between views without page reloads. Modules or Standalone Components organize the application, while directives and pipes extend HTML and format data. The application starts from `index.html`, executes `main.ts`, bootstraps Angular, loads the root component, fetches data through services, and updates the UI using change detection.

---

# Key Takeaways

- Angular is component-based.
- Components build UI.
- Services contain business logic.
- Dependency Injection manages object creation.
- Routing enables SPA navigation.
- Directives modify HTML.
- Pipes transform data.
- Change Detection keeps the UI synchronized with data.