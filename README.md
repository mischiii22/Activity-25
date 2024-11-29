# SOLID Principles in Angular

## S - Single Responsibility Principle (SRP)
**Definition**: A class should have one and only one reason to change. This principle ensures each class focuses on a single functionality.

```typescript
// UserService: Handles user-related operations
@Injectable({
  providedIn: 'root',
})
export class UserService {
  private apiUrl = 'https://api.example.com/users';

  constructor(private http: HttpClient) {}

  getUsers() {
    return this.http.get(this.apiUrl);
  }

  addUser(user: any) {
    return this.http.post(this.apiUrl, user);
  }
}

// UserComponent: Focuses on UI logic
@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
})
export class UserComponent {
  users: any[] = [];

  constructor(private userService: UserService) {}

  ngOnInit() {
    this.userService.getUsers().subscribe((data: any) => (this.users = data));
  }
}
```
## O - Open/Closed Principle (OCP)
**Definition**: A class should be open for extension but closed for modification.
```typescript
// PaymentStrategy: Define an interface for payment methods
export interface PaymentStrategy {
  processPayment(amount: number): void;
}

// CreditCardPayment: Implements PaymentStrategy
export class CreditCardPayment implements PaymentStrategy {
  processPayment(amount: number): void {
    console.log(`Processing credit card payment of $${amount}`);
  }
}

// PaymentService: Uses PaymentStrategy
@Injectable({
  providedIn: 'root',
})
export class PaymentService {
  constructor(private paymentStrategy: PaymentStrategy) {}

  makePayment(amount: number) {
    this.paymentStrategy.processPayment(amount);
  }
}
```
## L - Liskov Substitution Principle (LSP)
**Definition**: Subtypes should be substitutable for their base types. This ensure components or services extend functionality without altering behavior.
```typescript
// BaseService: Base class for APIs
export abstract class BaseService {
  abstract fetchData(): any;
}

// UserService: Extends BaseService
@Injectable({
  providedIn: 'root',
})
export class UserService extends BaseService {
  fetchData() {
    return { id: 1, name: 'John Doe' };
  }
}
```
## I - Interface Segregation Principle (ISP)
**Definition**: A class should not be forced to implement methods it does not use. Uses smaller interfaces to segregate functionalities.
```typescript
export interface CanEdit {
  edit(): void;
}

export interface CanDelete {
  delete(): void;
}

export class UserManagement implements CanEdit, CanDelete {
  edit() {
    console.log('Editing user...');
  }

  delete() {
    console.log('Deleting user...');
  }
}
```
## D - Dependency Inversion Principle (DIP)
```typescript
// Logger Interface
export interface Logger {
  log(message: string): void;
}

// ConsoleLogger Implementation
export class ConsoleLogger implements Logger {
  log(message: string): void {
    console.log(message);
  }
}

// AppModule: Provide ConsoleLogger as Logger
@NgModule({
  providers: [{ provide: Logger, useClass: ConsoleLogger }],
})
export class AppModule {}
```
These states the benefits of applying SOLID principles in Angular development. 
## Read the Full Article on Hashnode
[Activity 25: SOLID PRINCIPLES in Angular](https://mischii.hashnode.dev/activity-25-solid-principles-in-angular)
