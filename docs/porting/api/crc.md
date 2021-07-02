# Hardware CRC

The hardware CRC HAL API provides a low-level interface to the hardware CRC module of a hardware platform. Implementing the hardware CRC API allows you to gain the performance benefits of using hardware acceleration for CRC calculations. For platforms without hardware CRC capabilities, the API falls back to using the table and bitwise CRC implementations.

---

**Note:** The hardware CRC APIs are not thread safe.

---
## Assumptions

### Defined behavior

- Calling `hal_crc_compute_partial_start()` multiple times overrides the current CRC configuration and calculation.
- The current intermediate calculation is lost if the module is reconfigured with `hal_crc_compute_partial_start()`.
- `hal_crc_compute_partial()` does nothing if the buffer is undefined or the size is equal to 0.
- `hal_crc_compute_partial()` is safe to call multiple times. The new data is appended to the current calculation.
- `hal_crc_is_supported` must return false if the platform cannot support the initial or final values or data reflection in or out that the input polynomial requires.

### Undefined behavior

- Calling the `hal_crc_get_result()` function multiple times is undefined. The contents of the result register after it has been read is platform-specific. The HAL API makes no assumptions about what the register contains, so it is not safe to call this multiple times.
- Calling the `hal_crc_partial_start()` function with a polynomial unsupported by the current platform is undefined. Polynomial support should be checked before this function is called.
- Calling the `hal_crc_get_result()` function before calling `hal_crc_partial_start()` is undefined. The module must be initialized before the get result function can return meaningful values.
- Calling the `hal_crc_compute_partial()` function before calling `hal_crc_partial_start()` is undefined. The hardware CRC module must be configured correctly using the start function before writing data to it.

## Dependency

The hardware CRC module in the MCU that supports at least one of the following defined polynomials: `POLY_8BIT_CCITT, POLY_7BIT_SD, POLY_16BIT_CCITT, POLY_16BIT_IBM, POLY_32BIT_ANSI `

## Implementing the CRC API

You can find the API and specification for the hardware CRC API in the following header file:

[![View code](../../images/view_library_button.png)](https://mcu-driver-hal.github.io/MCU-Driver-HAL/doxygen/html/group__hal__crc.html)

## Testing

The MCU-Driver-HAL API provides a set of conformance tests for hardware CRC. You can use these tests to validate the correctness of your implementation.

To run the hardware CRC HAL tests, follow the generic testing instructions in your vendor's driver implementation.
