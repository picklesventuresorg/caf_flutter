## Get Started

* **Clone the Boilerplate App**
* **Running Scripts Using Makefile**
  * [Get Dependencies](#heading--2-1)
  * [Build the Project](#heading--2-2)
  * [Watch the Changes](#heading--2-3)
  * [Run the App](#heading--2-4)


### Clone the Boilerplate App

Using the below command: 

```
git clone git@github.com:picklesventuresorg/caf_flutter_boilerplate.git

```

* Launch the project via your preference IDE
* Open up your choice of emulator either Android, iOS or Chrome


### Running Scripts Using Makefile

The template app comes with a `Makefile`, which is a file used to simplify the execution of common tasks. 


<a id="heading--2-1"></a>

#### Get Dependencies

Run this following command:

```
make get 
// or
flutter pub get
```

To download and install the dependencies specified in the project's `pubspec.yaml` file. It ensures that all the required packages are available for Boilerplate App to build and run correctly.


<a id="heading--2-2"></a>

#### Build the Project

Run this following command:

```
make build 
// or
flutter pub run build_runner build --delete-conflicting-outputs
```

The `build_runner` is a package that provides a way to generate code automatically, such as serialization code for data models or view models for Boilerplate App's UI. The `--delete-conflicting-outputs` flag ensures that any previously generated files that conflict with the new ones are deleted before the new files are generated.


<a id="heading--2-3"></a>

#### Watch the Changes

Run this following command:

```
make watch
// or
flutter pub run build_runner watch
```

It watches for changes in Boilerplate App's source code and automatically regenerates the code when changes are detected. This is useful during development, as it saves you from manually running the build command every time you make a change to your code.


<a id="heading--2-4"></a>

#### Run the App

Run the app with a specific flavor using the following command:

```
make run-dev
// or
flutter run --flavor dev --target lib/main_dev.dart
```

Replace `dev` with the desired flavor name (`dev`, `stg`, and `prod`). The `--target` flag specifies the entry point file for the flavor (`main_dev.dart`, `main_stg.dart`, and `main.dart`).