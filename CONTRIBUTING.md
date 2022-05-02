# Contributing

## Structure

```
bin/fast_i18n.dart -- command line tool
lib/src/
    builder/** -- builds model
    generator/** -- generates output of .g.dart file using model objects
    model/** -- the model used as an intermediate 
    utils/** -- helper functions
```

## How it works

This is very simplified.

* Step 1. `bin/fast_i18n.dart` or `lib/src/builder.dart` - Entry point
* Step 2.  From JSON / YAML to model
    * Step 2.1. `builder/translation_map_builder` - Deserialize raw strings to common data types like `List` or `Map`
    * Step 2.2. `builder/translation_model_builder` - Use these common data structures to create the model
* Step 3. `generator/generator` - Generate `.g.dart` content using `model/**` generated by step 2