# Bloc

This document introduces Flutter Bloc, our chosen state management solution for the XYZ boilerplate project. Bloc empowers you to manage application state effectively, ensuring a clean, predictable, and testable architecture.

## Overview

Flutter Bloc, built upon the BLoC (Business Logic Component) pattern, separates concerns between UI, data, and business logic. This approach promotes maintainability, scalability, and simplifies communication within the app.

## Key Components

- **Bloc**:  The core component, it manages application state and handles events. It receives events, processes them based on business logic, and emits new states.
- **Cubit**: A simpler alternative to Bloc, suitable for smaller state management needs. It directly emits states in response to events, without the complexity of Bloc's transition functions.
- **Event**: Represents user actions or internal triggers that initiate state changes. Events are immutable data objects.
- **State**: Represents the current data state of your application. It's also immutable and reflects the information displayed in the UI.


| Feature |  Bloc  | Cubit |
|:-----| -------- |------|
| Complexity | More complex | Simpler |
| Flexibility | More flexible for complex state management | Suitable for simpler state needs |
| Transition functions | Processes events based on your defined business logic in `mapEventToState` | Processes business logic through function calls. |

## Cubit
1. **Define State**: Start by defining a class representing the data your Cubit will manage. 
```dart 
    part of 'dashboard_cubit.dart';

    sealed class DashboardState {
        const DashboardState();
    }

    class DashboardInit extends DashboardState {
        const DashboardInit();
    }

    class DashboardLoading extends DashboardState {
        const DashboardLoading();
    }

    class DashboardSuccess extends DashboardState {
        final DashboardModel result;

        const DashboardSuccess(this.result) : super();
    }

    class DashboardError extends DashboardState {
        final DataErrorMsg errorMsg;

        const DashboardError(this.errorMsg) : super();
    }
```

2. **Create Cubit** : Create a Cubit class extending `Cubit<StateType>`, where StateType refers to the state class you defined previously. The Cubit class handles events and emits new states accordingly.
```dart
    part 'dashboard_state.dart';

    class DashboardCubit extends Cubit<DashboardState> {
        final DashboardService dashboardService;
        final Repository repository;

        DashboardCubit({
            required this.dashboardService,
            required this.repository,
        }) : super(const DashboardInit());

        Future<void> getDashboard() async {
            emit(const DashboardLoading());
            try {
                final response = await dashboardService.dashboard();
                emit(DashboardSuccess(response));
            } on DataException catch (e) {
                emit(DashboardError(e.message));
            }
        }
    }
```

3. **Using the Cubic**: 
```dart
    context.read<DashboardCubit>().getDashboard();
```



## Bloc
1. **Define State**: Same as above

2. **Define Event**: 
```dart
    part of 'dashboard_bloc.dart';

    abstract class DashboardEvent {}

    class GetDashboardEvent extends DashboardEvent {}
```

3. **Create Bloc**: Create a class extending `Bloc<EventType,StateType>`, `on<EventType>` method is used to map event to state changes.
```dart
    part of 'dashboard_bloc.dart';

    class DashboardBloc extends Bloc<DashboardEvent, DashboardState> {
        final DashboardService dashboardService;
        final Repository repository;

        DashboardBloc({
            required this.dashboardService,
            required this.repository,
        }) : super(const DashboardInit()) {
            on<GetDashboardEvent>(_getDashboard);
        }

        Future<void> _getDashboard(GetDashboardEvent event, Emitter<DashboardState> emit) async {
            emit(const DashboardLoading());
            try {
            final response = await dashboardService.dashboard();
            emit(DashboardSuccess(response));
        } on DataException catch (e) {
            emit(DashboardError(e.message));
        }
      }
    }
```

4. **Using the Bloc**: 
```dart
    context.read<DashboardBloc>().add(GetDashboardEvent());
```


## Bloc Widgets

### BlocBuilder

Renders UI based on the current state, simplifying conditional rendering. It automatically rebuilds the widget tree whenever the state emitted by the Bloc changes.

```dart
BlocBuilder<DashboardBloc, DashboardState>(
  builder: (context, state) {
    if (state is DashboardLoading) {
      return CircularProgressIndicator();
    } else if (state is DashboardSuccess) {
      return Text('${state.result.count}');
    } else {
      return Text('Error: ${state.errorMsg}');
    }
  },
),
```


### BlocListener

Perform side effects or navigation based on state changes.Suitable for reacting to specific state changes without rebuilding the entire UI. It listens to state changes and triggers actions like showing snackbars, navigating, or triggering API calls.

```dart
BlocListener<DashboardBloc, DashboardState>(
  listener: (context, state) {
    if (state is DashboardError) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(state.errorMsg)),
      );
    }
  },
),
```

### BlocConsumer

Combine functionality of BlocBuilder and BlocListener.Suitable for situations where both UI updates and side effects are needed based on state changes. It acts as a combination of BlocBuilder and BlocListener, reducing boilerplate code.

```dart
BlocConsumer<DashboardBloc, DashboardState>(
  builder: (context, state) {
    if (state is DashboardLoading) {
      return CircularProgressIndicator();
    } else if (state is DashboardSuccess) {
      return Text('${state.result.count}');
    } else {
      return Text('Error: ${state.errorMsg}');
    }
  },
  listener: (context, state) {
    if (state is DashboardError) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(state.errorMsg)),
      );
    }
  },
),
```
### Bloc Provider
Use to inject Bloc instances into the widget tree, making them accessible to descendant widgets.It simplifies the process of managing Bloc lifecycle and ensures Bloc instances are shared effectively within the subtree.

In this case, since BlocProvider is responsible for creating the bloc, it will automatically handle closing the bloc.

By default, `BlocProvider` will create the bloc lazily, meaning create will get executed when the bloc is looked up via `BlocProvider.of<DashboardBloc>(context)`.
```dart
BlocProvider<DashboardBloc>(
  create: (context) => DashboardBloc(),
  child: YourWidgetTree(),
),
```

To override this behavior and force create to be run immediately, lazy can be set to false.

```dart
BlocProvider<DashboardBloc>(
  lazy: false,
  create: (BuildContext context) => DashboardBloc(),
  child: YourWidgetTree(),
);
```

#### BlocProvider.value
Helps share existing Bloc instances without automatic closing, ideal for using same Bloc across different routes. If the Bloc instance is initialized above the `MaterialApp` widget in the widget tree, it's accessible throughout the app and there's no need to use `BlocProvider.value` for navigation. However, if a Bloc instance is specific to a single screen and you want to pass it to another screen, you'll need to use `BlocProvider.value`.
```dart
BlocProvider.value(
  value: BlocProvider.of<BlocA>(context),
  child: ScreenA(),
);
```
then we can access in Other Screen.

Accessing the Bloc: Use `BlocProvider.of<DashboardBloc>(context)` in descendant widgets to get the Bloc instance and interact with it:
```dart
    context.read<DashboardBloc>().add(GetDashboardEvent());
```
or 
```dart
     context.read<DashboardCubit>().getDashboard();
```


#### Choosing the Right tool:
- **BlocBuilder**: Use for UI that needs to stay in sync with the current state.
- **BlocListener**: Use for side effects or navigation triggers based on state changes.
- **BlocConsuler**: Use when you need both UI updates and side effects within one widget.