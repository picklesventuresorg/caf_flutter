## CAF Dependencies Overview

### Pickles Design System
- The `caf_ui` serves as the foundation for consistent and visually appealing user interfaces across all Flutter applications developed within CAF. It offers predefined UI components, standardized styles, and theming capabilities, ensuring a seamless and unified user experience.
```
    caf_flutter_ui:
        git:
            url: "git@github.com:picklesventuresorg/caf_flutter_ui.git"
            ref: 1.0.0

    caf_ui_responsive:
      git:
        url: "git@github.com:picklesventuresorg/caf_flutter_ui.git"
        path: ./caf_ui_responsive
        ref: 1.0.0
```

### Utilities & Helper
- The `caf_utilities` is a versatile toolkit designed to streamline common tasks and improve development efficiency. It encompasses a collection of utility functions and tools, fostering code reusability, and providing essential functionalities for optimizing various aspects of Flutter app development.
```
  caf_flutter_utilities:
    git:
      url: "git@github.com:picklesventuresorg/caf_flutter_utilities.git"
      ref: 1.0.0
```

### Notifications
- The `caf_notification` empowers developers to manage notifications effectively in their Flutter applications. Offering flexibility and customization options, it facilitates the implementation of both local & push notifications and deep linking ensuring a robust communication channel with users.
```
  caf_flutter_notification:
    git:
      url: "git@github.com:picklesventuresorg/caf_flutter_notification.git"
      ref: 1.0.0
```

### Media
- The `caf_media` simplifies media-related tasks within Flutter applications. With features tailored for gallery management, image picking, and basic image editing, it provides developers with a comprehensive solution for handling and presenting media content seamlessly.

```
  caf_media_gallery:
    git:
      url: "git@github.com:picklesventuresorg/caf_flutter_media.git"
      ref: caf_media_gallery-v1.1.1
      path: packages/caf_media_gallery

  caf_media_edit:
    git:
      url: "git@github.com:picklesventuresorg/caf_flutter_media.git"
      ref: caf_media_edit-v1.0.0
      path: packages/caf_media_edit

  caf_media_picker:
    git:
      url: "git@github.com:picklesventuresorg/caf_flutter_media.git"
      ref: caf_media_picker-v1.0.2
      path: packages/caf_media_picker
```

Explore this guide to gain insights into each dependency, understand their purpose, and leverage them to elevate the development experience within the CAF Flutter ecosystem.
