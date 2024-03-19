## Theming


- **[Pickles Design System](#heading--1)**
- **[Customization Design System](#heading--2)**
- **[Multi-Tenancy](#heading--3)**


<a id="heading--1"></a>

### Pickles Design System

Our app leverages the `caf_flutter_ui` package, providing a well-crafted default **Pickles design system**. This includes predefined color schemes, typography, and overall aesthetic choices for a consistent and delightful user interface. The Pickles design system not only ensures a visually pleasing experience but also enhances the usability and accessibility of the app.


<a id="heading--2"></a>

### Customization ThemeData

Developers have the flexibility to tailor the app's appearance according to their project requirements. By creating their custom ThemeData, they can seamlessly integrate a different design system, aligning with their brand or unique preferences.

```
    ThemeData(
        useMaterial3: false,
        canvasColor: colors.colorScheme!.background,
        scaffoldBackgroundColor: colors.colorScheme!.background,
        fontFamily: colors.colorScheme!.fontFamily,
        disabledColor: colors.colorScheme!.disabled,
        outlinedButtonTheme: ThemeStyle.outlinedButtonThemeData(colors),
        textButtonTheme: ThemeStyle.textButtonThemeData(colors),
        elevatedButtonTheme: ThemeStyle.elevatedButtonThemeData(colors),
        textTheme: ThemeStyle.textTheme(colors),
        cardTheme: ThemeStyle.cardTheme(colors),
        appBarTheme: ThemeStyle.appBarTheme(colors),
        inputDecorationTheme: ThemeStyle.inputDecorationTheme(colors),
        switchTheme: ThemeStyle.switchThemeData(colors),
        colorScheme: ColorScheme.fromSwatch(
        backgroundColor: colors.colorScheme!.background),
    );
```

And customise their own `styling` for their widget components. Example as below:

```
  static TextButtonThemeData textButtonThemeData(TenantColor colors) {
    return TextButtonThemeData(
        style: TextButton.styleFrom(
            foregroundColor: colors.colorScheme!.primary,
            textStyle: ThemeStyle.textTheme(colors).labelLarge));
  }
```

<a id="heading--3"></a>

### Multi Tenancy

#### Overview

Create different themes for each tenant (user or group of users) and apply the appropriate theme when the tenant logs in. This allows you to provide a customized user experience, including different color schemes and styles, for each tenant within a single application.

#### Components 

- `ThemeScheme`
    - Encapsulates various theme-related properties for pre-defined colors or sizing that is not available in `themeData`. 
    - Create a different `themeScheme` for each tenant so when tenant logs in, you could load their `ThemeScheme` and apply it to the application,
- `TenantColor`: 
    - Abstract class that represents the color scheme for a tenant in a multi-tenant application. 
    - It's designed to handle both light and dark themes, which can be switched based on the `currentBrightness ` property.
- `TenantTheme`
    - Concrete class that represents the theme for a tenant in a multi-tenant application. It's designed to handle different themes for different tenants, which can be switched based on the brightness property.
    - You could create a new `TenantTheme` object for each tenant when they log in, ie `TenantABColor` and `TenantABThemeData`.
- `TenantThemeData`
    - Abstract class that represents the theme data for a tenant in a multi-tenant application. 
    - It's designed to generate a `ThemeData` object based on the color scheme of the tenant.
- `ThemeStyle`
    - The `ThemeStyle` class is a utility class that provides static methods to generate various theme-related objects based on a given `TenantColor` object. 
