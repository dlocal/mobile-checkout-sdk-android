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
}
```

Add dLocal Direct SDK dependency to the application's [build.gradle]() file:

```groovy
dependencies {
   implementation 'com.dlocal.android:mobile-checkout:0.1.1'
}    
```

# Getting started

## Initialize the SDK

First call `configure` static method form DLMobileCheckout to initialize the SDK. 

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

DLMobileCheckout.configure(apiKey = "API KEY", countryCode = "COUNTRY CODE")
```
> NOTE: To initialize the dLocal Mobile Checkout you will need an API Key. Please contact your Technical Account Manager to obtain one.

Replace `apiKey` with your dLocal Api Key and `countryCode` with the two letter [ISO 3166](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code: for example "UY" for "Uruguay", or "US" for "United States". 

You can find full list of country codes [here](https://documentation.dlocal.com/reference/country-reference).

Use `testMode` parameter to specify whether you are going to be doing testing in sandbox environment or if you are going to be performing real transactions.

## Allow installments

Use `allowInstallments` parameter to specify whether user will be able to select installments as part of the checkout process.

```kotlin
DLMobileCheckout.configure(
    apiKey = "API KEY", 
    countryCode = "COUNTRY CODE",
    allowInstallments = true // defaults to `false` if not specified
)
```

## Testing the integration

We strongly **recommend that you use the `SANDBOX` environment when testing**, and only use `PRODUCTION` in production ready builds.

You can specify to use a different environment with the `testMode` param, which is false by default, i.e:

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkout = if (BuildConfig.DEBUG) {
    DLMobileCheckout.configure(apiKey = "API KEY", countryCode = "COUNTRY CODE", testMode = true)
} else {
    DLMobileCheckout.configure(apiKey = "API KEY", countryCode = "COUNTRY CODE", testMode = false)
}
```

Replacing the `apiKey` with yours for each environment.

See the [SampleApplication]() for a detailed example.


## DLMobileCheckout BottomSheetDialog

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkoutBottomSheet = DLMobileCheckout.getDLMobileCheckoutBottomSheet(
    amount = 500, // Amount to charge
    currencyCode = "USD", // Currency of the amount
    onSuccess = { result ->
        println("Card token successfully generated: ${result.cardToken} to charge ${result.totalAmount}")
        if (result.installmentsId != null) {
            print("Payment will be processed in ${result.installments} installments, each installments with a value of ${result.installmentsAmount}")
        } else {
            println("Installments not allowed, payment will be processed in a single transaction for total amount")
        }
    },
    onError = { error ->
        println("Checkout process failed: $error")
    }
)

// Open the card form bottom sheet
checkoutBottomSheet.show(supportFragmentManager, "DLMobileCheckout_TAG")
```

To present the BottomSheetDialogFragment from your activity/fragment just call `show` and pass your FragmentManager and a Tag.


## Result Handling

In order to handle the result of the operation you can make use of

```kotlin
var onSuccess: ((DLCheckoutResult) -> Unit)? = null
var onError: ((String) -> Unit)? = null
```

`onSuccess` expects a function whose argument is a DLCheckoutResult object. This DLCheckoutResult is going to contain the result data of the card tokenization.

`onError` expects a function whose argument is a string. Is used to return an error message informing the nature of the error itself.

Detail of the `DLCheckoutResult` data class.

```kotlin
data class DLCheckoutResult(
    val cardToken: String,
    val totalAmount: Double,
    val installments: Int,
    val installmentsId: String?,
    val installmentsAmount: Double
)
```

## Form Styles

You can customize the look and feel of the checkout interface using a `DLMobileCheckoutTheme` as follows:

```kotlin
import com.dlocal.mobilecheckout.DLMobileCheckout

val checkoutTheme = DLMobileCheckoutTheme(
    buttonText = "Add card",
    titleSize = 22,
)

val checkoutBottomSheet = DLMobileCheckout.getDLMobileCheckoutBottomSheet(
    theme = checkoutTheme, // Apply a theme with your custom values
    amount = 500, // Amount to charge
    currencyCode = "USD", // Currency of the amount
    onSuccess = { },
    onError = { }
)
```

`formStyle` changes the look on the text fields
You can choose one of the three predefined form styles using the Builder function `style`:

```kotlin
var formStyle: DLFormStyle = DLFormStyle.DEFAULT
```

OUTLINED: Utilizes the Material3 TextInputLayout OutlinedBox style in the text fields.

FILLED: Utilizes the Material3 TextInputLayout FilledBox style in the text fields.

PILL: Utilizes the Material3 TextInputLayout OutlinedBox style with corners rounded at 100% in the text fields.

DEFAULT: Utilizes the Material3 TextInputLayout OutlinedBox style in the text fields.


Detail of the FormStyle enum class

```kotlin
enum class DLFormStyle {
    DEFAULT, OUTLINED, FILLED, PILL
}
```

You can also customize the individual elements of the form using the DLMobileCheckout variables.

```kotlin
var formStyle: DLFormStyle = DLFormStyle.DEFAULT
var textColor: Pair<Int, Int>? = null
var titleColor: Pair<Int, Int>? = null
var labelColor: Pair<Int, Int>? = null
var hintColor: Pair<Int, Int>? = null
var outlineColor: Pair<Int, Int>? = null
var fillColor: Pair<Int, Int>? = null
var errorColor: Pair<Int, Int>? = null
var buttonTextColor: Pair<Int, Int>? = null
var buttonBackgroundColor: Pair<Int, Int>? = null
var backgroundColor: Pair<Int, Int>? = null
var scanColor: Pair<Int, Int>? = null
var buttonText: String? = null
var buttonTextSize: Int? = null
var titleSize: Int? = null
var scanTextSize: Int? = null
var installmentsAmount: Double? = null
var installmentsCurrency: String? = null
var onResultSuccess: ((DLCheckoutResult) -> Unit)? = null
var onResultError: ((String) -> Unit)? = null
```

Colors can be passed using a resource or and hexadecimal value.

Color resource
```kotlin
textColor = Pair(
    lightColor = resources.getColor(R.color.textLight, null),
    darkColor = resources.getColor(R.color.textDark, null)
)
```

Hex Value
```kotlin
textColor = Pair(
    lightColor = "#0A0A0A",
    darkColor = "#8F8F8F"
)
```

# API Reference

[View documentation](https://dlocal.github.io/mobile-checkout-sdk-android/)

# Report Issues

If you have a problem or find an issue with the SDK please contact us at [mobile-dev@dlocal.com](mailto:mobile-dev@dlocal.com).

# License

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
