# Installation


## Install
You can either download [here]() or clone the git repository using the below command.
```bash

git clone git@github.com:picklesventuresorg/caf_flutter_boilerplate.git

```

* Launch the project via your preference IDE
* Open up your choice of emulator either Android or iOS
* Run this following command & compile the app
    ```bash
    flutter run
    ```
    ```bash
    flutter pub get
    ```
## Create Code Generation for DI (Dependency Injection)
Run these scripts via the Terminal

		`flutter packages pub run build_runner build --delete-conflicting-outputs`

Or use the watch  command to keep the source updated automatically

		`flutter packages pub run build_runner watch`
