# Localization

This section guides you through the process of adding new languages to your app.

## Steps

- **Creating `ARB` files**: Add `[country_code].arb` file under `l10n` folder inside `lib`
    - `lib/l10n/app_en.arb` for English
    - `lib/l10n/app_ms.arb` for Malay

Each key-value pair in the ARB file represents a text ID and its translation. 
```arb
{
    "login_title": "Login",
    "login_title_google": "SignIn With Google",
}
```
- **Generate**: run `flutter pub get` to let flutter generate localizations files for us.

- **Using Translations** : `context.translate?.login_title`


## Placeholders
- In your ARB files, you can define placeholders using curly braces `{}`. For example:
```
  "greet_username":"Hello {username}",
    "@greet_username":{
        "description": "A message with a single parameter",
        "placeholders": {
            "username": {
              "type": "String"
          }
        }
    },
```
- In your Dart code, you can pass the dynamic value to the localized string like this:
```dart
context.translate?.greet_username("John")
```
## Plurals

- In your `ARB` files, you can define plurals like this:
```
{
  "numberOfItems": "{count, plural, =0{No items} =1{1 item} other{{count} items}}"
}
```
- In your Dart code, you can use the plural localized string like this:
```dart
  context.translate?.numberOfItems(0)
  // Output : No Items

  context.translate?.numberOfItems(1)
  // Output: 1 item

  context.translate?.numberOfItems(10)
  // 10 items
```

Fore more detailed information about localization in Flutter, please refer to the offical Flutter documentation: [Flutter Localizations](https://docs.flutter.dev/ui/accessibility-and-internationalization/internationalization#placeholders-plurals-and-selects).