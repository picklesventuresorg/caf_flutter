## Hide Generated Files
To hide automatically generated files in:

### Android Studio

- Go to Preferences > Editor > File Types
- Under "Ignore files and folders" section, add:

		`*.inject.summary;*.inject.dart;*.g.dart;`

### VS Code

- Go to Preferences > Settings
- Search for "Files:Exclude"
- And add these following:
  
		`**/*.inject.summary
		 **/*.inject.dart
		 **/*.g.dart`

This will hide the generated *.inject.summary, *.inject.dart, and *.g.dart files in the project tree view. Keeping the project tree clean helps focus on application code.