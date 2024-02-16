## Dependencies Overview

Production dependencies are packages needed to run your app. Without them, the app would not function properly.

Unlike dev dependencies used only during development, production dependencies are bundled with the app in release builds. They include packages implementing core app capabilities and business logic.

| Dependency                         | Version      | Purpose                                              |
|------------------------------------|--------------|------------------------------------------------------|
| flutter                            | sdk: flutter | Flutter SDK                                          |
| flutter_localizations              | sdk: flutter | Flutter localization support                        |
| flutter_web_plugins                | sdk: flutter | Flutter web plugins                                  |
| shared_preferences                 | ^2.2.2       | Local storage for key-value pairs                    |
| flutter_bloc                       | ^8.1.3       | State management using the BLoC pattern             |
| bloc                               | ^8.1.2       | BLoC library for Flutter                             |
| get_it                             | ^7.6.6       | Dependency injection                                 |
| dio                                | ^5.4.0       | HTTP requests and networking                         |
| retrofit                           | ^4.0.3       | Type-safe HTTP client                                |
| json_annotation                    | ^4.8.1       | Code generation for JSON serialization/deserialization|
| equatable                          | ^2.0.5       | Simplifies equality comparisons for objects          |
| logger                             | ^2.0.2+1     | Logging library                                      |
| floor                              | ^1.4.2       | SQLite abstraction for Flutter                       |
| sqflite                            | ^2.3.0       | SQLite plugin for Flutter                            |
| internet_connection_checker_plus   | ^2.2.0       | Checks network connectivity                          |
| analyzer                           | ^5.13.0      | Analyzing code                                       |
| go_router                          | ^13.0.1      | Routing/Navigator for Flutter                        |
| intl                               | ^0.18.1      | Internationalization and localization support        |
| package_info_plus                  | ^5.0.1       | Access to package information at runtime             |
| flutter_svg                        | ^2.0.9       | SVG rendering for Flutter                            |
| local_auth                         | ^2.1.8       | Local authentication                                 |
| google_sign_in                     | ^6.2.1       | Google Sign-In authentication                        |
| firebase_auth                      | ^4.16.0      | Firebase authentication                              |
| firebase_core                      | ^2.15.1      | Firebase Core                                        |
| firebase_auth_mocks                | ^0.13.0      | Mocks for Firebase authentication (for testing)     |
| firebase_messaging                 | ^14.7.10     | Firebase Cloud Messaging                             |
| webview_flutter_web                | ^0.2.2+4     | Flutter web support for webview                       |
| webview_flutter                    | ^4.4.4       | Flutter plugin for WebView on Android and iOS        |
