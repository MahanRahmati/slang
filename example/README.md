# Example

## Step 1: Add dependencies

```yaml
dependencies:
  fast_i18n: ^1.4.0

dev_dependencies:
  build_runner: any
```

## Step 2: Create JSON files

Create these files inside your `lib` directory. Preferably in one common package like `lib/i18n`.

`strings.i18n.json`

```json
{
  "hello": "Hello $name",
  "save": "Save",
  "login": {
    "success": "Logged in successfully",
    "fail": "Logged in failed"
  }
}
```

`strings_de.i18n.json`

```json
{
  "hello": "Hallo $name",
  "save": "Speichern",
  "login": {
    "success": "Login erfolgreich",
    "fail": "Login fehlgeschlagen"
  }
}
```

`config.i18n.json (optional but recommended)`

```json
{
  "baseLocale": "en"
}
```

## Step 3: Generate the dart code

```
flutter packages pub run build_runner build
```

## Step 4: Initialize

```dart
@override
void initState() {
  super.initState();

  // a: use device locale
  LocaleSettings.useDeviceLocale().whenComplete(() {
    setState((){});
  });

  // b: use specific locale
  LocaleSettings.setLocale('de');

  // c: use default locale (default json locale)
  // *do nothing*
}
```

### Step 4b: iOS-only

```
File: ios/Runner/Info.plist

<key>CFBundleLocalizations</key>
<array>
   <string>en</string>
   <string>de</string>
</array>
```

## Step 5: Use your translations

```dart
// raw string
String translated = t.hello(name: 'Tom');

// inside component
Text(t.login.success)
```

## API

When the dart code has been generated, you will see some useful classes and functions

`t` - the most important translate variable

`LocaleSettings.useDeviceLocale()` - use the locale of the device

`LocaleSettings.setLocale('de')` - change the locale

`LocaleSettings.currentLocale` - get the current locale

`LocaleSettings.locales` - get the supported locales

## Additional features

**Maps**

Sometimes you need to access the translations via keys.
Define the maps in your `config.i18n.json`.
Keep in mind that all nice features like autocompletion are gone.

`strings.i18n.json`
```json
{
  "welcome": "Welcome",
  "thisIsAMap": {
    "hello world": "hello"
  },
  "classicClass": {
    "hello": "hello",
    "aMapInClass": {
      "hi": "hi"
    }
  }
}
```

`config.i18n.json`
```json
{
  "baseLocale": "en",
  "maps": [
    "thisIsAMap",
    "classicClass.aMapInClass"
  ]
}
```

Now you can access this via key:

```dart
String a = t.thisIsAMap['hello world'];
String b = t.classicClass.hello; // the "classical" way
String c = t.classicClass.aMapInClass['hi']; // nested
```

**Lists**

Lists are fully supported.

```json
{
  "niceList": [
    "hello",
    "nice",
    [
      "nestedList"
    ],
    {
      "wow": "wow"
    },
    {
      "a map entry": "cool"
    }
  ]
}
```

```dart
String a = t.niceList[1];
String b = t.niceList[2][0];
String c = t.niceList[3].wow;
String d = t.niceList[4]['a map entry'];
```