# Change Log

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

## [Unreleased]

### Changed

- [breaking-change] Updated synopsys-usb-otg dependency to v0.2.0.
- Cleanups to the Sdio driver, some hw independent functionality moved to the new sdio-host library.
- [breaking-change] Sdio is disabled by default, enable with the `sdio` feature flag.
- Move SDIO card power handling to its own function.
- [breaking-change] Add a 2 ms delay after changing SDIO card power setting.
- [breaking-change] Changed sdio::{read, write}_block buf argument to &[u8; 512].
- Voltage regulator overdrive is enabled where supported and required for selected HCLK.
- I2C driver updated to detect and clear all error condition flags.
- Allow for skipping an ongoing DMA transfer if not using double buffering.
- Change DMA traits to `embedded-dma`.
- Use bitbanding during clock enabling and peripheral reset to avoid data races.
- Add missing `Write` implementation for `Serial` and implemented better error handling.
- [breaking-change] ADC2 and ADC3 no longer allow access to VREF, VBAT, or the internal
  temperature measurement (ADC2 and ADC3 do not have an internal connection for these channels)
- Improved Serial baudrate calculation to be correct for higher baudrates or lower PCLKs
- Added `SysCfg` wrapper to enforce clock enable for `SYSCFG`
- [breaking-change] gpio::ExtiPin now uses `SysCfg` wrapper instead of `SYSCFG`
- Change `WriteBuffer + 'static` to `StaticWriteBuffer`in the DMA module.
- Fixed a race condition where SPI writes could get stuck in an error state forever (PR #269).
- Implement generics on the serial module.

### Added

- Reexport PAC as `pac` for consistency with other crates, consider `stm32` virtually deprecated
- Added external interrupt (EXTI) support for output pins
- Added `check_interrupt` method for GPIO pins
- Basic support for DAC
- Add initial DMA support
- Allow specification of ADC reference voltage in ADC configuraton structure
- Added support for hardware-based CRC32 functionality
- Add `MonoTimer` and `Instant` structs for basic time measurement.
- Added support for I2S and SAI clocks
- Added support for canbus with the bxcan crate (0.4.0).
- Added a `freeze_unchecked` method [#231]
- Added support for the Real Time Clock (RTC)
- Added option to bypass the HSE oscillator and use a clock input [#263]
- Added support for CAN on additional models: STM32F412, STM32F413, STM32F415,
  STM32F417, STM32F423, STM32F427, STM32F429, STM32F437, STM32F439, STM32F469,
  and STM32F479 [#262]

[#231]: https://github.com/stm32-rs/stm32f4xx-hal/pull/231
[#262]: https://github.com/stm32-rs/stm32f4xx-hal/pull/262
[#263]: https://github.com/stm32-rs/stm32f4xx-hal/pull/263

### Fixed

- Stability fixes related to SD card write
- Fixed issue where timer generated a spurious interrupt after start
- Allow implementations for DMASet from outside the crate [#237]
- DMA: Make it possible to create the wrapper types for the timers [#237]
- DMA: Fix some `compiler_fences` [#237]
- DMA: Fix docs [#237]
- RCC for F412, F413, F423, F446: Add missing configuration of PLLI2SCFGR.PLLI2SN [#261]
- RCC for F411: Add missing configuration of PLLI2SCFGR.PLLI2SM [#264]

[#237]: https://github.com/stm32-rs/stm32f4xx-hal/pull/237
[#261]: https://github.com/stm32-rs/stm32f4xx-hal/pull/261
[#264]: https://github.com/stm32-rs/stm32f4xx-hal/pull/264

## [v0.8.3] - 2020-06-12

### Fixed

- Make sure that I2C writes are concluded with a STOP condition

## [v0.8.2] - 2020-05-29

### Added

- Added sdio driver
- Allow specifying the desired SDIO bus speed during initialization

## [v0.8.1] - 2020-05-10

### Added

- Implement `timer::Cancel` trait for `Timer<TIM>`.
- Added DWT cycle counter based delay and stopwatch, including an example.

## [v0.8.0] - 2020-04-30

### Changed

- [breaking-change] Updated stm32f4 dependency to v0.11.
- Wait 16 cycles after setting prescalers for some clock domains to follow manual.
- Fixed `TIM9` `pclk` and `ppre`.

### Added

- Implement `timer::Cancel` trait for `Timer<SYST>`.
- Added PWM support and example.

## [v0.7.0] - 2020-03-07

### Changed

- Added more type states for open drain AF modes so we can prevent (potential fatal) I2C misuse
- [breaking-change] Updated stm32f4 dependency to v0.10.0.

### Added

- Added examples in the examples folder.
- Added USB driver.
- PLL48Clk configuration.
- Added bit-banding implementation.
- Added support for RNG peripheral and rand_core, and an example that uses it.

## [v0.6.0] - 2019-10-19

### Changed

- [breaking-change] Updated embedded-hal dependency to v0.2.3 and switched
  to digtial::v2 traits.

### Added

- Implemented `Qei` trait
- Implemented `clear_interrupt()` method for TIM timers

## [v0.5.0] - 2019-04-27

### Changed

- [breaking-change] Updated stm32f4 dependency to v0.7.0.
- Replace macro by generic impl over spi1::RegisterBlock in SPI.

### Fixed

- Properly terminate I2C read with a NACK then a STOP.

## [v0.4.0] - 2019-04-12

### Added

- API to enable and disable SPI interrupts

- Hal ADC supporting one-shot and sequence conversion of regular channels.

- Implement IndependentWatchdog for the IWDG peripheral

- Implement reading the device electronic signature from flash

### Changed

- [breaking-change] Updated cortex-m dependency to v0.6.0.

## [v0.3.0] - 2019-01-14

### Added

- Support ToggleableOutputPin trait in GPIO to allow using toggle().

- Possibility to configure GPIO pins into analog input mode.

- Possibility to configure GPIO pins to generate external interrupts.

- Support NoTx and NoRx in Serial to allow setting up a Rx only or Tx only port.

- Support for stm32f405, stm32f410, stm32f413, stm32f415, stm32f417, stm32f423,
  stm32f427, stm32f437, stm32f439, stm32f446, stm32f469 and stm32f479.

- Support for I2C2 and I2C3 in addition to I2C1.

- Read and Write implementations for Serial.

### Changed

- More versatile RCC clocks configuration.

- Allow using any pair of Pins for I2C rather than only a few hardcoded ones.

- Allow using any pair of Pins for Serial rather than only a few hardcoded ones.

- [breaking-change] Updated stm32f4 dependency to v0.6.0.

### Fixed

- Serial baud rate divisor fractional overflow.

## [v0.2.8] - 2018-12-16

### Fixed

- Documentation generation.

## [v0.2.7] - 2018-12-16

### Changed

- Switched to Rust 2018 edition.

## [v0.2.6] - 2018-12-06

### Added

- Support for GPIOH PH0 and PH1 on stm32f401.

- Support for serial port Idle interrupt.

- Basic `memory.x` linker script and default linking options.

### Changed

- Changed repository URL.

### Fixed

- I2C clock setting.

- GPIO `set_open_drain` is now setting the `otyper` register correctly.

## [v0.2.5] - 2018-11-17

### Added

- Support for stm32f411.

- Spi trait implementation.

### Changed

- Updated stm32f4 dependency to v0.4.0.

- Simplified and improved RCC parameters selection.

## [v0.2.4] - 2018-11-05

### Added

- Support for Serial interrupt enable and disable.

### Changed

- Made the `rt` feature of stm32f4 optionnal.

### Fixed

- Avoid overwritting the cache bits in `flash.acr`.

## [v0.2.3] - 2018-11-04

### Added

- Support for stm32f412.

- Gpio InputPin implementation for output pins to allow reading current
  value of open drain output pins.

- Timer trait implementation.

### Changed

- No default features selected for the stm32f4 crate. User must specify its
  specific device.

## [v0.2.2] - 2018-10-27

### Added

- Gpio `set_speed`.

## [v0.2.1] - 2018-10-13

### Fixed

- Removed unnecessary feature gates.

## [v0.2.0] - 2018-10-11

### Added

- Support for stm32f401.

- [breaking-change]. Support for setting serial port word length, parity and
  stop bits.

### Changed

- Support longuer Delay without asserting.

## v0.1.0 - 2018-10-02

### Added

- Support for stm32f407 and stm32f429.

[Unreleased]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.8.3...HEAD
[v0.8.3]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.8.2...v0.8.3
[v0.8.2]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.8.1...v0.8.2
[v0.8.1]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.8.0...v0.8.1
[v0.8.0]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.7.0...v0.8.0
[v0.7.0]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.6.0...v0.7.0
[v0.6.0]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.5.0...v0.6.0
[v0.5.0]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.4.0...v0.5.0
[v0.4.0]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.3.0...v0.4.0
[v0.3.0]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.8...v0.3.0
[v0.2.8]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.7...v0.2.8
[v0.2.7]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.6...v0.2.7
[v0.2.6]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.5...v0.2.6
[v0.2.5]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.4...v0.2.5
[v0.2.4]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.3...v0.2.4
[v0.2.3]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.2...v0.2.3
[v0.2.2]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.1...v0.2.2
[v0.2.1]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.2.0...v0.2.1
[v0.2.0]: https://github.com/stm32-rs/stm32f4xx-hal/compare/v0.1.0...v0.2.0
