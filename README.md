# Mobile Checkout Android SDK

The dLocal Mobile Checkout SDK allows easy integration with dLocal API providing a customizable 
Android native UI form to collect payment card information and tokenize.

## Requirements

- Supports API versions from 24 (Android 7.0 Nougat) and higher.
- App permission `android.permission.INTERNET`

## Installation

New releases of the dLocal Mobile Checkout SDK are published via [Maven Repository](https://mvnrepository.com/artifact/com.dlocal.android/mobile-checkout).  
The latest version is available via `mavenCentral()`.

Add `mavenCentral()` to the project level [build.gradle]() file's repositories section, if you don't have it already:

```groovy
repositories {
   mavenCentral()
   ...
}
```

Add dLocal Direct SDK dependency to the application's [build.gradle]() file:

```groovy
dependencies {
   ... 
   implementation 'com.dlocal.android:mobile-checkout:0.0.6'
   ...
}    
```

# Getting started

## Initialize the SDK

Create an instance of DLMobileCheckout using the Builder and passing your API key and the two letter [ISO 3166](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code:
```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkout = DLMobileCheckout
    .Builder(apiKey = "API KEY", countryCode = "COUNTRY CODE", testMode = true)
    .build()
```
> NOTE: To initialize the dLocal Mobile Checkout you will need an API Key. Please contact your Technical Account Manager to obtain one.

Replace `apiKey` with your dLocal Api Key and `countryCode` with the two letter country code, for example "UY" for "Uruguay", or "US" for "United States". You can find full list of country codes [here](https://documentation.dlocal.com/reference/country-reference).

Use `testMode` parameter to specify whether you are going to be doing testing in sandbox environment or if you are going to be performing real transactions.

## Testing the integration

We strongly **recommend that you use the `SANDBOX` environment when testing**, and only use `PRODUCTION` in production ready builds.

You can specify to use a different environment with the `testMode` param, which is false by default, i.e:

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkout = if (BuildConfig.DEBUG) {
    DLMobileCheckout.Builder(apiKey = "SBX API KEY", countryCode = "US", testMode = true)
} else {
    DLMobileCheckout.Builder(apiKey = "PROD API KEY", countryCode = "US", testMode = false)
}.build()
```

Replacing the `apiKey` with yours for each environment.

See the [SampleApplication]() for a detailed example.


## DLMobileCheckout BottomSheetDialog

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkout = DLMobileCheckout
    .Builder(apiKey = "API KEY", countryCode = "COUNTRY CODE", testMode = true)
    .onResultSuccess { result ->
        println("Card token successfully generated: ${result.cardToken} to charge ${result.totalAmount}")
        if (result.installmentsId != null) {
            print("Payment will be processed in ${result.installments} installments, each installments with a value of ${result.installmentsAmount}")
        } else {
            println("Installments not allowed, payment will be processed in a single transaction for total amount")
        }
    }
    .onResultError { error ->
        println("Checkout process failed: $error")
    }
    .build()

    // Open the card form bottom sheet
    checkout.show(supportFragmentManager, DLMobileCheckout.TAG)
```

To present the BottomSheetDialogFragment from you activity/fragment just call `show` and pass your FragmentManager and a Tag.

## Allow installments

Use `installments` function in the Builder to specify whether user will be able to select installments as part of the checkout process, passing the total amount and currency code of the payment.

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkout = DLMobileCheckout
    .Builder(apiKey = "API KEY", countryCode = "COUNTRY CODE", testMode = true)
    .installments(amount = 500.0, currency = "USD")
    .build()
```

## Form Styles

You can choose one of the three predefined form styles using the Builder function `style`:

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkout = DLMobileCheckout
    .Builder(apiKey = "API KEY", countryCode = "COUNTRY CODE", testMode = true)
    .style(FormStyle.FILLED) // OUTLINED, FILLED, PILL
    .build()
```

You can also customize the individual elements of the form using the DLMobileCheckout.Builder

```kotlin
textColor(lightColor: Int, darkColor: Int)

titleColor(lightColor: Int, darkColor: Int)

labelColor(lightColor: Int, darkColor: Int)

hintColor(lightColor: Int, darkColor: Int)

outlineColor(lightColor: Int, darkColor: Int)

fillColor(lightColor: Int, darkColor: Int)

errorColor(lightColor: Int, darkColor: Int)

buttonTextColor(lightColor: Int, darkColor: Int)

buttonBackgroundColor(lightColor: Int, darkColor: Int)

backgroundColor(lightColor: Int, darkColor: Int)

scanColor(lightColor: Int, darkColor: Int)

buttonText(text: String)

titleSize(fontSize: Int)

scanTextSize(fontSize: Int)

buttonTextSize(fontSize: Int)

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
