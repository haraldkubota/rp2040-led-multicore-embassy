# Embedded Rust and Embassy

While looking for a simple and up-to-date example of Rust using [Embassy](https://embassy.dev/), it took me way longer than I thought to find it. A lot of examples were using old crate versions, crate features have changed etc.

So here is one simple example which:

* works on RP2040
* works using the Raspberry Pi Pico Probe (highly recommended if you still use U2F downloads)
* uses Embassy
* prints out messages via SWD (no UART/USB needed)
* uses 2 cores

The example itself is [multicore.rs](https://github.com/embassy-rs/embassy/blob/main/examples/rp/src/bin/multicore.rs) from the Embassy examples.

## Running it

Note that I use GPIO 26 for the LED in my example. RPi Pico has its LED on GPIO 25.

```
$ cargo run
   Compiling led-multicore v0.1.0 (/home/harald/git/rp2040-led-multicore-embassy)
    Finished `dev` profile [optimized + debuginfo] target(s) in 0.82s
     Running `probe-rs run --chip RP2040 --protocol swd target/thumbv6m-none-eabi/debug/led-multicore`
      Erasing ✔ [00:00:00] [#########################################################################] 20.00 KiB/20.00 KiB @ 50.74 KiB/s (eta 0s )
  Programming ✔ [00:00:01] [#########################################################################] 20.00 KiB/20.00 KiB @ 19.73 KiB/s (eta 0s )    Finished in 1.45s
INFO  Hello from core 0
└─ led_multicore::__core0_task_task::{async_fn#0} @ src/main.rs:48  
INFO  Hello from core 1
└─ led_multicore::__core1_task_task::{async_fn#0} @ src/main.rs:59
```

Use ^C to stop, but the program will continue to run on the RPi Pico.

## Requirements

Install target compiler:

```
$ rustup target add thumbv6m-none-eabi
```

To use the Pico probe:

```
cargo binstall probe-rs
```

To use various tools, e.g. to see the code size of the generated ELF (e.g. `cargo size`):

```
$ cargo binstall cargo-binutils
$ cargo size
    Finished `dev` profile [optimized + debuginfo] target(s) in 0.04s
   text    data     bss     dec     hex filename
  16832       0  103960  120792   1d7d8 led-multicore
```

## Notable Facts

* memory.x and build.rs are not optional
* .cargo/config.toml is needed too
