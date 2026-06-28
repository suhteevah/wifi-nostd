# wifi-nostd

`no_std` Intel WiFi driver for bare-metal Rust — an iwlwifi equivalent.

Supports Intel AX201, AX200, AX210, and AC 9260 families. These are PCIe devices
that share the same command/response transport (TFD/RXQ rings) and require firmware
microcode loaded into device SRAM before any 802.11 operations.

## Features

- PCI device detection (vendor 0x8086, device ID matching)
- Firmware `.ucode` parsing and BSM upload
- Active/passive WiFi scanning with signal strength
- WPA2-PSK: PBKDF2-SHA1 key derivation, 4-way EAPOL handshake
- AES-128-CCMP encryption/decryption (pure Rust, no_std)
- IEEE 802.11 frame parsing and construction (beacon, auth, assoc, QoS data)
- TFD/RXQ DMA descriptor ring management

## Architecture

```text
WifiController (driver.rs)        -- high-level API: scan, connect, disconnect
    |
WPA2 handshake (wpa.rs)          -- 4-way handshake, PBKDF2-SHA1, AES-CCMP
    |
Scanning (scan.rs)               -- active/passive scan, SSID matching
    |
Commands (commands.rs)           -- NVM access, PHY config, scan cmd, assoc
    |
TX/RX queues (tx_rx.rs)         -- TFD ring (transmit), RXQ (receive)
    |
Firmware loader (firmware.rs)    -- microcode upload to device SRAM
    |
PCI detection (pci.rs)          -- vendor 0x8086, device ID matching
    |
802.11 frames (ieee80211.rs)    -- beacon, probe, auth, assoc, data, QoS
```

## Usage

```rust,ignore
use wifi_nostd::driver::WifiController;

let mut wifi = unsafe { WifiController::init(bar0_base, irq_line, phys_mem_offset)? };
wifi.load_firmware(fw_data)?;

let networks = wifi.scan_networks()?;
for net in &networks {
    log::info!("SSID={} signal={}dBm", net.ssid, net.signal_dbm);
}

wifi.connect("MyNetwork", "hunter2")?;
```

## Firmware Requirement

Intel WiFi hardware is inert without firmware. Place the appropriate `.ucode` file
on your storage medium. See `WifiVariant::firmware_name()` for expected filenames.

Firmware images are available from the
[linux-firmware](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git) repository.

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE))
- MIT License ([LICENSE-MIT](LICENSE-MIT))

at your option.

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

---

## Support This Project

If you find this project useful, consider buying me a coffee! Your support helps me keep building and sharing open-source tools.

[![Donate via PayPal](https://img.shields.io/badge/Donate-PayPal-blue.svg?logo=paypal)](https://www.paypal.me/baal_hosting)

**PayPal:** [baal_hosting@live.com](https://paypal.me/baal_hosting)

Every donation, no matter how small, is greatly appreciated and motivates continued development. Thank you!
