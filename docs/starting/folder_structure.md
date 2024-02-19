# Folder structure

This document outlines the folder structure and purpose of each directory in the Pickles's CAF Flutter Boilerplate App.

## Overview
```
.
└── lib/
    ├── application
    ├── config
    ├── core/
    │   ├── theme
    │   └── const
    ├── data/
    │   ├── datasource/
    │   │   ├── remote
    │   │   └── local
    │   ├── mapper
    │   ├── model
    │   └── repository
    ├── di
    ├── extensions
    ├── l10n
    ├── persentation/
    │   ├── base
    │   ├── bloc
    │   ├── models
    │   ├── view/
    │   │   └── components
    │   └── widget
    └── utils
```
### Application: 
- Services like push notifications, analytics and crashlytics reporting etc.

### Core:
- **theme**: Contains app theme definitions and color schemes.
- **const**: Stores constant values throughout the app.

### Config:
- Holds configurations for routing.

### Data layer:

- **datasource**: Sets up connections to remote data sources (e.g., API clients) or local databases.
    - **remote**: Remote api client setup.
    - **local**: setup database and share perferene. 
- **mapper**: Transforms data models from the data layer to the UI layer format.
- **model**: Defines data classes representing various types of information fetched from different sources.
- **repository**: Acts as a mediator between the data layer and UI layer, interacting with datasources and mapping data, handling errors gracefully.

### Dependency Injection (DI):

- Provide dependencies used throughout the app.

### Extension:

- Contains custom extensions for widgets or other classes, enhancing reusability and adding functionality.

### Localization (l10n):
- Stores translation files for various languages, enabling internationalization and localization.

### Presentation:

- **base**: base widgets commonly used by other screens for consistency and inheritance.
- **bloc**: state management classes using Bloc organized by features.
- **models**: Defines data classes used specifically within the UI layer.
- **view**: Holds the actual UI views and screens representing different app sections.
    - **components**: Widgets related to a specific view.
- **widgets**: Stores reusable UI components used across multiple screens.

### utils:
- Contains commonly used utility functions unrelated to specific functionalities within the app.