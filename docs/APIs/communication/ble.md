# Bluetooth Low Energy (BLE)

Bluetooth low energy (BLE) is a very low power wireless technology standard
for personal area networks. Typical applications that uses BLE are health care,
fitness trackers, beacons, smart home, security, entertainment, proximity
sensors, Industrial and automotive.

mbed BLE, also called BLE_API, is the Bluetooth Low Energy software solution for
mbed. This thin abstraction runs on many targets and allows developers to easily
create new BLE applications.

## Getting started

1. Choose a [mbed platform which support BLE](https://developer.mbed.org/platforms/?mbed-enabled=15&connectivity=3).
For example the [NRF51-DK](https://developer.mbed.org/platforms/Nordic-nRF51-DK/).
If your platform doesn't support BLE but is compatible with the Arduino UNO R3
connector you can use an extension board like the [X-NUCLEO-IDB05A1](https://developer.mbed.org/components/X-NUCLEO-IDB05A1-Bluetooth-Low-Energy/)
to add BLE capabilities to your development board.

1. Install a ble scanner on your phone. It will allows you to track all BLE
activities from your embedded application. This is a mandatory tool for every
software development using BLE.

    * [nRF Master Control Panel](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp) for Android.

    * [LightBlue](https://itunes.apple.com/gb/app/lightblue-bluetooth-low-energy/id557428110?mt=8) for iPhone.

1. Compile and run our BLE samples.

    * **mbed os 5 samples** are available on [developer.mbed.org](https://developer.mbed.org/teams/mbed-os-examples/)
      and [Github](https://github.com/ARMmbed/mbed-os-example-ble):
        * The [Beacon](https://developer.mbed.org/teams/mbed-os-examples/code/mbed-os-example-ble-Beacon/)
          example is a good starting point, it demonstrate how you can create a
          BLE Beacon with few lines of code.  
        * The [heart rate monitor](https://developer.mbed.org/teams/mbed-os-examples/code/mbed-os-example-ble-HeartRate/)
          example demonstrate how to build an heart rate sensor which can be
          connected and monitored by a ble client like your phone.
        * The [LED](https://developer.mbed.org/teams/mbed-os-examples/code/mbed-os-example-ble-LED/)
          and [LED blinker](https://developer.mbed.org/teams/mbed-os-examples/code/mbed-os-example-ble-LEDBlinker/)
          are indeed a single examples which demonstrate how client (LED) and
          server (LED blinker) work together in BLE.

        All those examples can be imported in the web IDE or be built locally
        with [mbed-cli](https://github.com/ARMmbed/mbed-cli).

    * **mbed os 3 samples** are available on [Github](https://github.com/ARMmbed/ble-examples) .

    * **mbed os 2 samples** are available on the [Bluetooth Low Energy](https://developer.mbed.org/teams/Bluetooth-Low-Energy/)
      page on developer.mbed.org .

    Despite the differences between mbed os versions, there is only **one** version
    of mbed BLE and it is easy to move code from one version of the os to another.

    For instance all BLE samples running on mbed os 2 are compatible, as is, with
    mbed os 5.

    Choose the samples you will use according to the version of mbed os supported
    by your development platform.

## Going further

* [mbed BLE intros](https://docs.mbed.com/docs/ble-intros/en/latest/) is a wonderful
  resources for developers new to BLE and mbed BLE. It explains in great detail
  how to build BLE embedded applications with mbed.

* The Bluetooth Low Energy main [page](https://developer.mbed.org/teams/Bluetooth-Low-Energy/)
  on developer.mbed.org also provide samples and resources around BLE.

* The mbed BLE [API reference](https://docs.mbed.com/docs/ble-api/en/master/api/index.html).

* The mbed BLE [online community](https://developer.mbed.org/teams/Bluetooth-Low-Energy/community/)
  is also a great place to ask questions and share your knowledge.

## Contributing

We gratefully accept bug reports and contributions from the community.

To be able to accept contributions of code, to be able to accept a contribution
we require a Contributor’s Licence Agreement (CLA) to be signed or authorized
by the author of the work or by whoever has the authority to do so.

The easiest way to do this is to create an mbed account and
[approve the agreement here with a click through](https://developer.mbed.org/contributor_agreement/)
if this is a personal contribution.

If this is a contribution from a company, or if you do not want to create a
mbed account, you can
[find an alternative agreement here](https://www.mbed.com/en/about-mbed/contributor-license-agreements/),
which can be signed and returned to ARM.

We require a CLA for all contributions, whether large or small.

If you would like to contribute, please:

* Check for open issues or start a discussion around a feature idea or a bug
in our [tracker](https://github.com/ARMmbed/ble/issues).

* Fork the [mbed BLE repository](https://github.com/ARMmbed/ble) on GitHub to
start making your changes. As a general rule, you should use the
"development" branch as a basis.

* Send a pull request and nag us until it gets merged and published.
