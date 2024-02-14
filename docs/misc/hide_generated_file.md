## Hide Generated Files
Via Android Studio > Preferences > Editor > File Types and paste the below lines under ignore files and folders section:

		`*.inject.summary;*.inject.dart;*.g.dart;`

Via VSC > Preferences >Settings > Search for Files:Exclude and add following lines

		`**/*.inject.summary
		 **/*.inject.dart
		 **/*.g.dart`