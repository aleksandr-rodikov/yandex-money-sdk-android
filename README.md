# Yandex.Money SDK for Android

## Overview

This library provides tools for performing payments on devices running Android 4.0 (API 14) or
higher.

Features:

* activity for payments with bank cards
* handling of application's *instance id*
* storing of saved cards

The library uses [Yandex.Money SDK for Java][1].

## Usage

### Gradle Dependency (jCenter)

[![Download](https://api.bintray.com/packages/yandex-money/maven/yandex-money-sdk-android/images/download.svg)]
(https://bintray.com/yandex-money/maven/yandex-money-sdk-android/_latestVersion)

To use the library in your project write this code to you build.gradle:

```groovy
allprojects {
    repositories {
        jcenter()
    }
}

dependencies {
    compile 'com.yandex.money.api:yandex-money-sdk-android:4.1.0'
}
```

### Payments

The simpliest way of using this library is to show `PaymentActivity` passing corresponding payment
parameters using `PaymentActivity.getBuilder`. Payment process will be
handled by the library. When done calling activity will receive `RESULT_OK` or `RESULT_CANCELED`
depending on operation success.

For example, if you want to pay for a phone, you can do it like this:

```Java
public class MyActivity extends Activity implement View.OnClickListener {

    private static final String CLIENT_ID = "[your_client_id]";
    private static final String HOST = "https://demomoney.yandex.ru";
    private static final int REQUEST_CODE = 1;

    ...

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == REQUEST_CODE && resultCode == RESULT_OK) {
            // payment was successful
        }
    }

    ...

    @Override
    public void onClickListener(View v) {
        PaymentParams phoneParams = PhoneParams.newInstance("79012345678", new BigDecimal(100.0));
        Intent intent = PaymentActivity.getBuilder(this)
                .setPaymentParams(phoneParams)
                .setClientId(CLIENT_ID)
                .setHost(HOST)
                .build();
        startActivityForResult(intent, REQUEST_CODE);
    }
}
```

The `setHost` call is optional. By default the production host(https://money.yandex.ru) will be
used.

## Sample Project

The library contains sample project in module `sample`.

It's self-contained and uses a demo environment configuration(see
`src/main/assets/demo_app.properties`). Feel free to create you own `app.properties` in the same
folder with appropriate production settings.

To obtain *client id* please visit [this page][2] (also available in [Russian][3]).

Keep in mind that the application data should be cleaned by you after switching from one host to
another.

## Links

1. Yandex.Money API (in [English][4], in [Russian][5])
2. [Yandex.Money SDK for Java][1]

[1]: https://github.com/yandex-money/yandex-money-sdk-java
[2]: http://api.yandex.com/money/doc/dg/tasks/register-client.xml
[3]: http://api.yandex.ru/money/doc/dg/tasks/register-client.xml
[4]: http://api.yandex.com/money/
[5]: http://api.yandex.ru/money/
