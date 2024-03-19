# Architecture <!-- {docsify-ignore-all} -->

This section explains the architecture used in Boilerplate App. We use a combination of the Model-View-ViewModel (MVVM) and Business Logic Component (BLoC) patterns. These allow for separation of app logic from the user interface. This makes testing, reusing code, and adding features easier. We'll explain how these architectural patterns are implemented in the project. The goal is to simplify building Flutter apps.

## Overview 

- **View**: Represents the user interface of the application. It is responsible for displaying the data and capturing user interactions. The View observes the ViewModel for changes and updates itself accordingly.

- **BLoC**: Handles the state management and business logic of the application. It receives events from the View, processes them, and emits new states. The View observes the BLoC for state changes and updates itself accordingly.

- **Repository**: Acts as a mediator between the ViewModel(Bloc) and the Data Layer. It retrieves, manages, and transforms data, hiding implementation details from the ViewModel.

- **DataLayer**: Encompasses data access mechanisms like local database, API calls, and file I/O. It interacts with external data sources like APIs and databases.


![Alt text](../../../assets/caf_flutter_architecture.png)

## Data Flow 

1. User interacts with the View, triggering an Event.
2. View dispatches the Event to the ViewModel (Bloc).
3. ViewModel processes the Event using business logic and interacts with the Repository.
4. Repository retrieves or modifies data from the Data Layer.
5. ViewModel receives updated data or State from the Repository.
6. ViewModel emits the new State through a Stream.
7. View listens to the Stream and updates its UI based on the latest State.


## Conclusion 

The architecture used in the Boilerplate App follows proven patterns. It's designed to create Flutter apps that are easy to maintain and upgrade. By sticking to modular development and good coding practices, we build apps that can scale over time. The goal is to keep the development process smooth and sustainable.



