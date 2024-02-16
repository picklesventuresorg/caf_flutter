## Router

**[Introduction](#heading--1)**

**[Basic](#heading--2)**
  * [Configuration](#heading--2-1)
  * [Define & Set Initial Route](#heading--2-2)
  * [Navigation between pages](#heading--2-3)
  * [Navigation back](#heading--2-4)

**[Features](#heading--3)**
  * [Parameter Handling](#heading--3-1)
  * [ShellRoute](#heading--3-2)
  * [DeepLinking](#heading--3-3)
  * [Error Handling](#heading--3-4)
  * [Page Transition](#heading--3-5)


<a id="heading--1"></a>

### <ins>Introduction</ins>

`GoRouter` is a powerful and flexible routing library for Flutter applications, designed to simplify navigation and deep linking within your mobile app. With GoRouter, you can efficiently manage navigation stacks, handle deep links, and structure your app's routes in a clean and maintainable way.

<a id="heading--2"></a>

### <ins>Basics</ins>

<a id="heading--2-1"></a>

#### Configuration

1. Add it as a dependency in pubspec.yaml:
    ```
    dependencies:  
    go_router: ^5.0.0
    ```

2. Import it in files you use routing
    ```
    import 'package:go_router/go_router.dart';
    ```
    This makes all of the router classes and methods available to configure routing and navigate.

<a id="heading--2-2"></a>

#### Define & Set Initial Route

- When setting up navigation, you need to define a map of routes for the router. These are path strings that correspond to different UI pages:
    ```
    const String _splash = '/splash';
    const String _login = '/login';
    ```

- Define routes in GoRouter:
    ```
    GoRoute(
        path: _splash,
        builder: (context, state) => SplashPage(),
    );
    ```

- To establish the initial route for your application, configure the initialLocation parameter in the GoRouter instance. This sets the starting point for your app's navigation.
    ```
    final router = GoRouter(
        initialLocation: splash,
        // ... (other configurations)
    );
    ```

<a id="heading--2-3"></a>

#### Navigation between pages

- Pushing to a new page

    Use the `push` method to navigate to a new page, adding it to the navigation stack:
    ```
    context.push(login);
    ```
- Replacing current route

    Utilize the `go` method to replace the current route with a new one:
    ```
    context.go(login);
    ```
- Animate the transition

    You can animate the transitions by specifying a Duration:
    ```
    context.push(login, duration: Duration(milliseconds: 500))
    ```

<a id="heading--2-4"></a>

#### Navigation back
- For navigating back in your application, rely on the standard Navigator methods. To `pop` the current route off the stack:
    ```
    context.pop();
    ```
- You can navigate back and convey a result to the previous screen using the pop method.
    ```
    context.pop(result: true); // Indicates success or acceptance
    // or
    context.pop(result: false); // Indicates cancellation or rejection
    ```
    The result parameter allows you to send a boolean value back to the screen that initiated the navigation. This is helpful when you want to communicate specific information or status, such as whether a user submitted a form successfully or canceled an action.

<a id="heading--3"></a>

### <ins>Features</ins> 


<a id="heading--3-1"></a>

#### Parameter Handling

- Path Parameters

    For dynamic data that can change per route access, using path and query parameters may be preferable.

    Some examples for path parameters:

    ```
    // Route path with parameter
    GoRoute(
    path: '/post/:postId',
    builder: (context, state) {
        final postId = state.parameters['postId']; 
        }
    )

    // Navigate with value  
    context.go('/post/12345');
    ```

    The parameter in the path gets populated into the state map automatically, no casting needed. 
    
- Query parameter:
    
    Accessing the URI directly allows getting current query values.

    ```
    context.go('/products?category=shoes&sort=desc');

    GoRoute(  
    builder: (context, state) {
        final category = state.uri.queryParameters['category'];
        final sort = state.uri.queryParameters['sort'];
        }
    )
    ```


- Extra Parameters

    The state.extra property allows passing an arbitrary map of data directly through context.go() that will be accessible in route builders.

    ```
    // Pass map when navigating 
    context.go('/product', extra: {
    'id': 123,
    'name': 'Product 1'  
    });

    // Access data in builder
    GoRoute(
    path: '/product',
    builder: (context, state) {
        final id = state.extra['id'] as int; 
        final name = state.extra['name'] as String;
        
        // Use data to build page dynamically 
        return ProductPage(id: id, name: name); 
        }  
    )
    ```

    So state.extra is useful for directly passing static data rather than dynamic parameters. It reduces parsing since you get the map directly. Downsides are that it does not allow updating data dynamically, and you need to cast values to intended types.

<a id="heading--3-2"></a>

#### ShellRoute

`ShellRoute` is a powerful feature in `go_router` that allows you to create a base structure for your app, providing a common scaffold for nested routes. Here's an example:

```
ShellRoute(
  builder: (_) => MainShell(), // shell 
  routes: [
    GoRoute(
      path: 'home', 
      builder: (_) => HomeScreen(),
      routes: [
        GoRoute(
          path: 'post/:id',
          builder: (_) => PostScreen();  
        )
     ]
    ),
  ]
);
```

Here `MainShell` acts as the application shell that surrounds inner page routes like `HomeScreen` and `PostScreen`.

Some advantages:
- Maintains persistent shell around content
- Lets you split main app UI from sub screens
- Nested navigation control

The shell builder is only rebuilt when switching between shell routes rather than individual inner routes for efficiency.

<a id="heading--3-3"></a>

#### DeepLinking

Deep linking in `go_router` involves defining meaningful paths for each screen/page and handling them appropriately in your route configurations. To support deep linking into nested screens:

1. Define nested route paths matching app pages

    ```
    ShellRoute(
        builder: (context, state, child) => AppShell(), 
        routes: [
          GoRoute(
            path: 'home',
            builder: (context, state) => HomeScreen(),
            routes: [
              GoRoute(
                path: 'posts/:postId',
                builder: (context, state) {
                  final postId = state.params['postId'];
                  return PostDetailScreen(id: postId); 
                }
              )  
            ]  
          ),
        ]
    );
    ```
2. Redirect to inner/child screen on deep link
  
    ```
    var router = GoRouter(
        // ... (other configurations)
        redirect: (state) {
        final postPath = state.location.contains('home/post');
        if (postPath) return null; // Allow
            return '/'; // Default redirect
        }
    );
    ```
3. Sample deeplink method

    ```
    context.go('/home/posts/123'); 
    ```



<a id="heading--3-4"></a>

#### Error Handling

- When something goes wrong during routing, `go_router` provides an errorBuilder callback:

    ```
    final router = GoRouter(
        errorBuilder: (context, state) => ErrorView()
        // ... (other configurations)
    );
    ```

- This builder gets called if:
  - A path does not match any defined routes
  - There's an exception during route building
- Within the builder, you can access:
  - `state.error`: The error object that occurred
  - `state.location`: The location it failed on

Use this to show fallback UI with messages for the user. It allows handling routing errors gracefully!


<a id="heading--3-5"></a>

#### Page Transition

- By default, it uses the platform's default page transition (slide on iOS, fade on Android). 
- You can override with your own custom transition animations using a transitionBuilder callbacks

    ```
    GoRouter(
        transitionBuilder: (context, animation, secondar  yAnimation, child) =>  
            FadeTransition(
            opacity: animation,
            child: child,
            ),
    )
    ```
- This transitions with a fade animation on all platforms.
- The animation and secondaryAnimation arguments represent the beginning and ending transitions that occur when leaving one route to another.

#### Reference 
- https://docs.page/csells/go_router/navigation
- https://pub.dev/packages/go_router