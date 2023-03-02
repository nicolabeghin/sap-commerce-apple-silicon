# sap-commerce-apple-silicon
Run [SAP (Hybris) Commerce Cloud (TM)](https://www.sap.com/products/crm/commerce-cloud.html) on Apple Silicon (M1/M2) without Rosetta emulation, for maximum performances.

## Project without dependency on `sapcorejco`
Project without requirement to connect to SAP ECC/S4 RFC (JCO): you don't need to perform anything special and can use directly the [SAPMachine 17 JDK](https://sap.github.io/SapMachine/) for `MacOS aarch64`

<img width="660" alt="image" src="https://user-images.githubusercontent.com/2743637/222374610-22d01de8-0cc6-4857-a7a1-3a8d47b07392.png">

## Project with `sapcorejco` dependency
Project with requirement to connect to SAP ECC/S4 RFC (JCO). In february 2023 [JCO 3.1.7](https://launchpad.support.sap.com/#/notes/3276799) has been released with Mac ARM64 (Apple Silicon) support. In order to leverage this:
1. download JCO 3.1.7 for `Apple MacOS 64-bit ARM` from https://support.sap.com/en/product/connectors/jco.html#Download 
2. replace the content of the JCO zip (inside the zip) in `hybris/bin/modules/core-customize/hybris/bin/modules/sap-framework-core/sapcorejco/lib/darwinintel64` 
<img width="326" alt="image" src="https://user-images.githubusercontent.com/2743637/222375200-977dc3ad-665a-45e4-9603-72f8e42da860.png">

3. download SAPMachine 17 for `MacOS aarch64` from https://sap.github.io/SapMachine/ <img width="660" alt="image" src="https://user-images.githubusercontent.com/2743637/222375258-16e0c04e-40c8-41fe-a138-bc86fc99f2c9.png">
4. extract ZIP and move to `/Library/Java/JavaVirtualMachines/sapmachine-jdk-17.0.6.jdk-aarch64` 
5. change at beginning of `hybris/bin/platform/setantenv.sh`:

    `export JAVA_HOME=/Library/Java/JavaVirtualMachines/sapmachine-jdk-17.0.6.jdk-aarch64/Contents/Home`


## Performance benchmark
*5 times faster* when running without Rosetta directly with `MacOS aarch64` JVM

before

> Server startup in 240040 ms

after

> Server startup in 50872 ms
