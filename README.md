# Envied

[![CI](https://github.com/petercinibulk/envied/actions/workflows/ci.yaml/badge.svg)](https://github.com/petercinibulk/envied/actions/workflows/ci.yaml)
[![codecov](https://codecov.io/gh/petercinibulk/envied/branch/main/graph/badge.svg?token=uIX88zsd9c)](https://codecov.io/gh/petercinibulk/envied)

A cleaner way to handle your environment variables in Dart/Flutter (GREATLY inspired by [Envify](https://pub.dev/packages/envied))

<br>

## Table of Contents

-   [Overview](#overview)
-   [Install](#install)
-   [Usage](#usage)
-   [License](#license)

<br>

## Overview

Using a `.env` file such as:

```.env
KEY=VALUE
```

and a dart class:

```dart
import 'package:envied/envied.dart';

part 'env.g.dart'

@Envied()
abstract class Env {
    @EnviedField(envName: 'KEY')
    static const key = _Env.key;
}
```

Envied will generate the part file which contains the values from your `.env` file using build_runner

You can then use the Env class to access your environment variable:

```dart
print(Env.key); // "VALUE"
```

<br>

## Install

Add both `envied` and `envied_generator` as dependencies,

If you are using creating a Flutter project:

```sh
$ flutter pub add envied
$ flutter pub add --dev envied_generator
$ flutter pub add --dev build_runner
```

If you are using creating a Dart project:

```sh
$ dart pub add envied
$ dart pub add --dev envied_generator
$ dart pub add --dev build_runner
```

This installs three packages:

-   [build_runner](https://pub.dev/packages/build_runner), the tool to run code-generators
-   envied_generator, the code generator
-   envied, a package containing the annotations.

<br>

## Usage

Add a `.env` file at the root of the project. The name of this file can be specified in your Envied class if you call it something else such as `.env.dev`.

```.env
KEY1=VALUE1
KEY2=VALUE2
```

Create a class to ingest the environment variables (`lib/env/env.dart`). Add the annotations for Envied on the class and EnviedField for any environment variables you want to be pulled from your `.env` file.

> IMPORTANT! Add both `.env` and `env.g.dart` files to your `.gitignore` file, otherwise, you might expose your environment variables.

```dart
// lib/env/env.dart
import 'package:envied/envied.dart';

part 'env.g.dart';

@Envied(path: '.env.dev')
abstract class Env {
    @EnviedField(envName: 'KEY1')
    static const key1 = _Env.key1;
    @EnviedField()
    static const KEY2 = _Env.KEY2;
}
```

Then run the generator:

```sh
# dart
dart run build_runner build
# flutter
flutter pub run build_runner build
```

You can then use the Env class to access your environment variables:

```dart
print(Env.key1); // "VALUE1"
print(Env.KEY2); // "VALUE2"
```

<br>

## License

MIT © Peter Cinibulk
