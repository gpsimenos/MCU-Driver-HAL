# Testing your port

It's important to test your port at the end of each module porting, rather than only once when you've imported all modules. 

## Testing with the Greentea framework

### Prerequisites

#### Minimum HAL module support

To run the built-in tests, you need to have ported and verified at least these HAL modules:

- Serial port (synchronous transfer). To verify that it works, load a test binary with `printf()`. Verify debug messages can be seen on your serial terminal program.
- Microsecond ticker.


You'll also need to have ported DAPLink or compatible interface firmware to your interface chip.
    <span class="notes">If DAPLink is still under development, you can still run tests manually.</span>

#### mbed-tools

Some tools from Mbed, including `mbedhtrun`, are required to run Greentea tests.

See [docs](../../user/README.md) about tools setup.
### Compiling tests

1. Go to the directory for the example containing the `CMakeLists.txt` file of the test you want to run. This is implementation specific.
2. Generate the build system files
   ```
   cmake -S . -B cmake_build
   ```
   The default toolchain is GCC_ARM. You can use a different toolchain by passing `-D MBED_TOOLCHAIN=<TOOLCHAIN>`. If you need to debug the binary, build with `-D CMAKE_BUILD_TYPE=debug`.
4. Compile the test
   ```
   cmake --build cmake_build -j
   ```

### Running the tests

Use `mbedhtrun` with the following options to run a test:
* `-f <path/to/test/binary.bin>` This is the path to the binary built above.
* `-e <path/to/host_tests/directory>` This will usually be `/MCU-Driver-HAL/tests/host_tests`.
* `-d <path/to/DAPLINK/mount/point>` This is the place where you usually drag-and-drop binaries when you flash your hardware via DAPLINK.
* `-p <path/to/tty/port>:115200` This is the serial interface that Greentea will use to talk to your hardware.

Example command line:
```
mbedhtrun -f ./sdfx_st/tests/mbed_hal/echo/cmake_build/sdfx-st-test-echo.bin -e ./MCU-Driver-HAL/tests/host_tests -d /Volumes/DIS_L4IOT -p /dev/tty.usbmodem145103:115200
```

If your hardware doesn't have a DAPLINK interface, you must flash the test binary manually via other means, then manually reset the hardware. Then, run `mbedhtrun` with the `--skip-reset --skip-flashing` options, omitting the `-f` and `-d` arguments.

Example command line (without DAPLINK):
```
mbedhtrun -e ./MCU-Driver-HAL/tests/host_tests -p /dev/tty.usbmodem145103:115200 --skip-reset --skip-flashing
```

#### Expected output

Greentea output for a test that ran successfully looks like this:

```
mbedhtrun -f ./sdfx_st/tests/mbed_hal/echo/cmake_build/sdfx-st-test-echo.bin -e ./MCU-Driver-HAL/tests/host_tests -d /Volumes/DIS_L4IOT -p /dev/tty.usbmodem145103:115200

[1626879922.99][HTST][INF] host test executor ver. 0.0.12
[1626879922.99][HTST][INF] copy image onto target...
[1626879928.12][MBED][WRN] Target ID not found: Skipping flash check and retry
[1626879928.13][HTST][INF] starting host test process...
[1626879928.13][CONN][INF] starting connection process...
[1626879928.14][CONN][INF] notify event queue about extra 60 sec timeout for serial port pooling
[1626879928.14][CONN][INF] initializing serial port listener... 
[1626879928.14][SERI][INF] serial(port=/dev/tty.usbmodem145103, baudrate=115200, read_timeout=0.01, write_timeout=5)
[1626879928.14][HTST][INF] setting timeout to: 60 sec
[1626879928.14][SERI][INF] reset device using 'default' plugin...
[1626879928.54][SERI][INF] waiting 1.00 sec after reset
[1626879929.54][SERI][INF] wait for it...
[1626879929.55][SERI][TXD] mbedmbedmbedmbedmbedmbedmbedmbedmbedmbed
[1626879929.55][CONN][INF] sending up to 2 __sync packets (specified with --sync=2)
[1626879929.55][CONN][INF] sending preamble '6d857b5d-7a15-4a79-b552-41eeef505818'
[1626879929.55][SERI][TXD] {{__sync;6d857b5d-7a15-4a79-b552-41eeef505818}}
[1626879929.56][CONN][RXD] mbedmbedmbedmbedmbedmbedmbedmbed
[1626879929.57][CONN][INF] found SYNC in stream: {{__sync;6d857b5d-7a15-4a79-b552-41eeef505818}} it is #0 sent, queued...
[1626879929.57][CONN][INF] found KV pair in stream: {{__version;1.3.0}}, queued...
[1626879929.57][CONN][INF] found KV pair in stream: {{__timeout;30}}, queued...
[1626879929.57][HTST][INF] sync KV found, uuid=6d857b5d-7a15-4a79-b552-41eeef505818, timestamp=1626879929.569591
[1626879929.57][CONN][INF] found KV pair in stream: {{__host_test_name;device_echo}}, queued...
[1626879929.57][HTST][INF] DUT greentea-client version: 1.3.0
[1626879929.57][HTST][INF] setting timeout to: 30 sec
[1626879929.57][HTST][INF] host test class: '<class 'device_echo.Device_Echo'>'
[1626879929.57][HTST][INF] host test setup() call...
[1626879929.57][HTST][INF] CALLBACKs updated
[1626879929.57][HTST][INF] host test detected: device_echo
[1626879929.58][CONN][RXD] >>> Running 1 test cases...
[1626879929.58][CONN][RXD] 
[1626879929.58][CONN][RXD] >>> Running case #1: 'Echo server: x16'...
[1626879929.58][CONN][INF] found KV pair in stream: {{__testcase_count;1}}, queued...
[1626879929.58][CONN][INF] found KV pair in stream: {{__testcase_name;Echo server: x16}}, queued...
[1626879929.59][CONN][INF] found KV pair in stream: {{__testcase_start;Echo server: x16}}, queued...
[1626879929.59][CONN][INF] found KV pair in stream: {{echo_count;16}}, queued...
[1626879929.61][SERI][TXD] {{echo_count;16}}
[1626879929.62][CONN][INF] found KV pair in stream: {{echo;abcdefghijklmnopqrstuvwxyzabcdefghi}}, queued...
[1626879929.63][SERI][TXD] {{echo;abcdefghijklmnopqrstuvwxyzabcdefghi}}
[1626879929.64][CONN][INF] found KV pair in stream: {{echo;klmnopqrstuvwxyzabcdefghijklmnopqrs}}, queued...
[1626879929.65][SERI][TXD] {{echo;klmnopqrstuvwxyzabcdefghijklmnopqrs}}
[1626879929.66][CONN][INF] found KV pair in stream: {{echo;uvwxyzabcdefghijklmnopqrstuvwxyzabc}}, queued...
[1626879929.67][SERI][TXD] {{echo;uvwxyzabcdefghijklmnopqrstuvwxyzabc}}
[1626879929.69][CONN][INF] found KV pair in stream: {{echo;efghijklmnopqrstuvwxyzabcdefghijklm}}, queued...
[1626879929.70][SERI][TXD] {{echo;efghijklmnopqrstuvwxyzabcdefghijklm}}
[1626879929.71][CONN][INF] found KV pair in stream: {{echo;opqrstuvwxyzabcdefghijklmnopqrstuvw}}, queued...
[1626879929.72][SERI][TXD] {{echo;opqrstuvwxyzabcdefghijklmnopqrstuvw}}
[1626879929.73][CONN][INF] found KV pair in stream: {{echo;yzabcdefghijklmnopqrstuvwxyzabcdefg}}, queued...
[1626879929.74][SERI][TXD] {{echo;yzabcdefghijklmnopqrstuvwxyzabcdefg}}
[1626879929.75][CONN][INF] found KV pair in stream: {{echo;ijklmnopqrstuvwxyzabcdefghijklmnopq}}, queued...
[1626879929.77][SERI][TXD] {{echo;ijklmnopqrstuvwxyzabcdefghijklmnopq}}
[1626879929.78][CONN][INF] found KV pair in stream: {{echo;stuvwxyzabcdefghijklmnopqrstuvwxyza}}, queued...
[1626879929.79][SERI][TXD] {{echo;stuvwxyzabcdefghijklmnopqrstuvwxyza}}
[1626879929.80][CONN][INF] found KV pair in stream: {{echo;cdefghijklmnopqrstuvwxyzabcdefghijk}}, queued...
[1626879929.81][SERI][TXD] {{echo;cdefghijklmnopqrstuvwxyzabcdefghijk}}
[1626879929.82][CONN][INF] found KV pair in stream: {{echo;mnopqrstuvwxyzabcdefghijklmnopqrstu}}, queued...
[1626879929.83][SERI][TXD] {{echo;mnopqrstuvwxyzabcdefghijklmnopqrstu}}
[1626879929.85][CONN][INF] found KV pair in stream: {{echo;wxyzabcdefghijklmnopqrstuvwxyzabcde}}, queued...
[1626879929.86][SERI][TXD] {{echo;wxyzabcdefghijklmnopqrstuvwxyzabcde}}
[1626879929.87][CONN][INF] found KV pair in stream: {{echo;ghijklmnopqrstuvwxyzabcdefghijklmno}}, queued...
[1626879929.88][SERI][TXD] {{echo;ghijklmnopqrstuvwxyzabcdefghijklmno}}
[1626879929.89][CONN][INF] found KV pair in stream: {{echo;qrstuvwxyzabcdefghijklmnopqrstuvwxy}}, queued...
[1626879929.90][SERI][TXD] {{echo;qrstuvwxyzabcdefghijklmnopqrstuvwxy}}
[1626879929.91][CONN][INF] found KV pair in stream: {{echo;abcdefghijklmnopqrstuvwxyzabcdefghi}}, queued...
[1626879929.93][SERI][TXD] {{echo;abcdefghijklmnopqrstuvwxyzabcdefghi}}
[1626879929.94][CONN][INF] found KV pair in stream: {{echo;klmnopqrstuvwxyzabcdefghijklmnopqrs}}, queued...
[1626879929.95][SERI][TXD] {{echo;klmnopqrstuvwxyzabcdefghijklmnopqrs}}
[1626879929.96][CONN][INF] found KV pair in stream: {{echo;uvwxyzabcdefghijklmnopqrstuvwxyzabc}}, queued...
[1626879929.97][SERI][TXD] {{echo;uvwxyzabcdefghijklmnopqrstuvwxyzabc}}
[1626879929.98][CONN][INF] found KV pair in stream: {{__testcase_finish;Echo server: x16;1;0}}, queued...
[1626879929.99][CONN][RXD] >>> 'Echo server: x16': 1 passed, 0 failed
[1626879929.99][CONN][RXD] 
[1626879929.99][CONN][RXD] >>> Test cases: 1 passed, 0 failed
[1626879929.99][CONN][INF] found KV pair in stream: {{__testcase_summary;1;0}}, queued...
[1626879929.99][CONN][INF] found KV pair in stream: {{end;success}}, queued...
[1626879929.99][CONN][INF] found KV pair in stream: {{__exit;0}}, queued...
[1626879929.99][HTST][INF] __exit(0)
[1626879930.00][HTST][INF] __notify_complete(True)
[1626879930.00][HTST][INF] __exit_event_queue received
[1626879930.00][HTST][INF] test suite run finished after 0.42 sec...
[1626879930.00][CONN][INF] received special event '__host_test_finished' value='True', finishing
[1626879930.01][HTST][INF] CONN exited with code: 0
[1626879930.01][HTST][INF] Some events in queue
[1626879930.01][HTST][INF] stopped consuming events
[1626879930.01][HTST][INF] host test result() call skipped, received: True
[1626879930.01][HTST][INF] calling blocking teardown()
[1626879930.01][HTST][INF] teardown() finished
[1626879930.01][HTST][INF] {{result;success}}
```
