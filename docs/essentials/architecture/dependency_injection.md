# Dependency Injection

**`GetIt`** handles dependency injection in Flutter apps. It is lightweight and flexible. This overview explains the key things you need to know to use GetIt effectively in the Boilerplate App. It focuses on the basic concepts and functions to manage dependencies well.

## Overview
- **Single Source of Truth**: `GetIt` acts as a centralized registry for all dependencies, making it easier to manage and track them throughout your app.
- **Loose Coupling**: Promote loose coupling between components, allowing them to work independently and reducing reliance on specific implementations.
- **Testability**: Simplify unit testing by easily injecting mock dependencies for isolation.

## Essential Methods
- **`registerFactory<T>(() => createInstance<T>())`**: 
    - Creates a new instance of `T` each time it's requested. 
    - Ideal for short-lived dependencies or those with custom logic for creation.
- **`registerSingleton<T>(T instance)`**: 
    - Creates a single instance of `T` and shares it throughout the app.
    - Ideal for global state management or singletons with well-defined lifecycles.
- **`registerLazySingleton<T>(() => createInstance<T>())`**: 
    - Creates a single instance of `T` only when first requested.
    - Similar to `registerSingleton`, but avoids unnecessary upfront instance creation.
- **`registerSingletonWithDependencies<T>(() => createInstance<T>(dependencies))`**: 
    - Creates a single instance of `T` with provided dependencies in the factory function.
    - Useful when a singleton instance depends on other registered dependencies.
- **`registerSingletonAsync<T>(Future<T> Function() createInstanceAsync):`**: 
    - Asynchronously creates a single instance of `T`.
    - Useful for fetching data from APIs or other asynchronous operations before providing the instance.

## Usage 
```dart
final getIt = GetIt.instance;

Future<void> setupLocator(FlavorConfig? config) async {
    getIt.registerSingletonAsync<SharedPreferences>(
      () => SharedPreferences.getInstance());
    
    getIt.registerLazySingleton(() => GoogleSignIn());
    
    getIt.registerSingletonWithDependencies<ConfigDAO>(
    () => getIt<AppDatabase>().configDao,
    dependsOn: [AppDatabase]);

    getIt.registerFactory(() => AuthService(
        client: getIt(),
      ));
}
```

In `main.dart`
```dart
await setupLocator(config);
```

Use `getIt<T>()` to retrieve registered dependencies anywhere in your app.

This is the minimal foundation of using `GetIt` in Flutter Applications.