# Repository

## Overview

The Repository pattern acts as an intermediary between the data layer and the rest of your application. It abstracts away the details of data access, retrieval, and manipulation, providing a unified interface for interacting with various data sources (e.g., databases, APIs, local storage). This separation offers several benefits.

## Purpose
- **Abstraction**: Hides data access implementation details (like API calls, local storage) from the business logic (Blocs,Cubit).
- **Testability**: Facilitates isolated testing of data access logic, independent of the UI and specific data sources.
- **Maintainability**: Promotes loose coupling, making it easier to modify or replace data sources without affecting business logic.
- **Data caching (optional)**: Act as a bridge to store remote data in local db. 

## Implementation:
1. **Concrete Implementation**: Create concrete implementations of the Repository for each data source you use (e.g., DatabaseRepository, ApiRepository). These classes handle the specific details of data access for each source.
2. **Usage in Bloc/Cubit**: Inject the appropriate `Repository` into your Bloc/Cubit instances using dependency injection.Interact with the data layer through the Repository methods, hiding the implementation details from your business logic.

Here's a typical Repository implementation:
```dart

class AuthRepo {
    @override
    Future<User> login(){
        /// Implementation goes here...
    }
}
```
