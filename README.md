# sap-commerce-apple-silicon
Run [SAP (Hybris) Commerce Cloud (TM)](https://www.sap.com/products/crm/commerce-cloud.html) on Apple Silicon (M1/M2) natively, without Rosetta emulation for maximum performances.

## Project without dependency on `sapcorejco`
Project without requirement to connect to SAP ECC/S4 RFC (JCO): you don't need to perform anything special and can use directly the [SAPMachine 17 JDK](https://sap.github.io/SapMachine/) for `MacOS aarch64` in place of `MacOS x64`.

<img width="660" alt="image" src="https://user-images.githubusercontent.com/2743637/222374610-22d01de8-0cc6-4857-a7a1-3a8d47b07392.png">

## Project with `sapcorejco` dependency
Project with requirement to connect to SAP ECC/S4 RFC (JCO). In february 2023 [JCO 3.1.7](https://launchpad.support.sap.com/#/notes/3276799) has been released with Mac ARM64 (Apple Silicon) support. In order to leverage this:
1. download JCO 3.1.7 for `Apple MacOS 64-bit ARM` from https://support.sap.com/en/product/connectors/jco.html#Download 
2. replace the content of the JCO zip (inside the zip) in `core-customize/hybris/bin/modules/sap-framework-core/sapcorejco/lib/darwinintel64` 
<img width="326" alt="image" src="https://user-images.githubusercontent.com/2743637/222375200-977dc3ad-665a-45e4-9603-72f8e42da860.png">

3. download SAPMachine 17 for `MacOS aarch64` in place of `MacOS x64` from https://sap.github.io/SapMachine/ <img width="660" alt="image" src="https://user-images.githubusercontent.com/2743637/222375258-16e0c04e-40c8-41fe-a138-bc86fc99f2c9.png">
4. extract ZIP and move to `/Library/Java/JavaVirtualMachines/sapmachine-jdk-17.0.6.jdk-aarch64` 
5. change at beginning of `hybris/bin/platform/setantenv.sh`:

    `export JAVA_HOME=/Library/Java/JavaVirtualMachines/sapmachine-jdk-17.0.6.jdk-aarch64/Contents/Home`


## Performance benchmark
*5 times faster* when running without Rosetta directly with `MacOS aarch64` JVM

before

> Server startup in 240040 ms

after

> Server startup in 50872 ms

## Troubleshooting
### Tanuki wrapper
Older SAP Commerce Cloud releases do not provide Tanuki wrapper for ARM64. Make sure you have this file:
* `core-customize/hybris/bin/platform/resources/tanukiwrapper/bin/wrapper-macosx-arm-64`

along with the old (MacOS Intel x64) ones
* `core-customize/hybris/bin/platform/resources/tanukiwrapper/bin/wrapper-macosx-universal-32`
* `core-customize/hybris/bin/platform/resources/tanukiwrapper/bin/wrapper-macosx-universal-64`

### JCO libraries blocked by Mac OSX
`libsapjco3.dylib` blocked by Mac OSX Gatekeeper (ref. https://disable-gatekeeper.github.io/) - they can be whitelisted from OSX Settings

<img width="292" alt="image" src="https://github.com/nicolabeghin/sap-commerce-apple-silicon/assets/2743637/30c981a7-683f-4ce9-9953-d048e8b2b99d">

<img width="803" alt="image" src="https://github.com/nicolabeghin/sap-commerce-apple-silicon/assets/2743637/65dc5c78-cedf-4d6b-8a12-d7fef5d9aa22">

