# Grinder

> Dart workflows, automated.

Grinder consists of a library to define project tasks (e.g. test, build, doc),
and a command-line tool to run them.

[![pub package](https://img.shields.io/pub/v/grinder.svg)](https://pub.dartlang.org/packages/grinder)
[![Build Status](https://travis-ci.org/google/grinder.dart.svg?branch=master)](https://travis-ci.org/google/grinder.dart)
[![Build status](https://ci.appveyor.com/api/projects/status/rxskyfnov8evqwib/branch/master?svg=true)](https://ci.appveyor.com/project/devoncarew/grinder-dart/branch/master)
[![Coverage Status](https://img.shields.io/coveralls/google/grinder.dart.svg)](https://coveralls.io/r/google/grinder.dart)

## Getting Started

To start using `grinder`, add it to your [dev_dependencies](https://www.dartlang.org/tools/pub/dependencies.html#dev-dependencies).

### Defining Tasks

Tasks are defined entirely by Dart code allowing you to take advantage of
the whole Dart ecosystem to write and debug them.  Task definitions reside
in a `tool/grind.dart` script. To create a simple grinder script, run:

    pub run grinder:init

In general, grinder scripts look something like this:

```dart
import 'package:grinder/grinder.dart';

main(args) => grind(args);

@Task('Test stuff.')
test() {
  new PubApp.local('test').run([]);
}

@DefaultTask('Build the project.')
@Depends(test)
build() {
  log("Building...");
}

@Task('Generate docs.')
@Depends(test)
doc() {
  log("Generating docs...");
}
```

Any task dependencies (see `@Depends` above), are run before the dependent task.

Grinder contains a variety of convenience APIs for common task definitions, such as
`PubApp` referenced above.  See the [API Documentation](http://www.dartdocs.org/documentation/grinder/latest) for full details.

### Running Tasks

First install the `grind` executable:

    pub global activate grinder

then use it to run desired tasks:

    grind test
    grind build doc

or to run a default task (see `@DefaultTask` above):

    grind

or to display a list of available tasks and their dependencies:

    grind -h

You can also bypass installing `grind` and replace it in any of the above
with `pub run grinder:grinder`, or [in Dart SDK 1.10](http://dartbug.com/22129), simply `pub run grinder`.

## Disclaimer

This is not an official Google product.
