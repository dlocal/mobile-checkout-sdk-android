# Mobile Checkout Android SDK

The dLocal Mobile Checkout SDK allows easy integration with dLocal API providing a customizable 
Android native UI form to collect payment card information and tokenize.

## Requirements

- Supports API versions from 24 (Android 7.0 Nougat) and higher.
- App permission `android.permission.INTERNET`

## Installation

New releases of the dLocal Mobile Checkout SDK are published via [Maven Repository](https://mvnrepository.com/artifact/com.dlocal.android/mobile-checkout).  
The latest version is available via `mavenCentral()`.

Add `mavenCentral()` to the project level [build.gradle](https://bitbucket.org/dlocal-public/mobile-checkout-sdk-android/src/master/build.gradle#lines-5) file's repositories section, if you don't have it already:

```groovy
repositories {
   mavenCentral()
   ...
}
```

Add dLocal Direct SDK dependency to the application's [build.gradle](https://bitbucket.org/dlocal-public/mobile-checkout-sdk-android/src/master/app/build.gradle#lines-38) file:

```groovy
dependencies {
   ... 
   implementation 'com.dlocal.android:mobile-checkout:0.0.1'
   ...
}    
```

## Report Issues

If you have a problem or find an issue with the SDK please contact us at [mobile-dev@dlocal.com](mailto:mobile-dev@dlocal.com).

## License

```text
    MIT License

    Copyright (c) 2022 DLOCAL

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
    SOFTWARE.
```