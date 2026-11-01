# Logs from the phone

Real logs pulled over USB (`fastboot oem log` / `get_staged`, see `../tools/run.sh`). Each holds the kernel's
messages and the iOS system log the kernel forwards, up to the last 1.4 MB (the buffer wraps on long runs).

**Scrubbed before publishing:** MAC addresses are replaced with `02:00:00:00:xx:yy` (consistently within a
file, so you can still follow one device), Wi-Fi network names with `<ssid>`, public IPs with `<ip>`, local paths
with `~`. The Bluetooth placeholder 00:00:00:00:5a:ad is left as it is.

| File | Image | What it shows |
|---|---|---|
| `daily1.log` | `xnu-daily1.img` | the daily configuration: Wi-Fi, BT, speakers, HUD, CPU ceiling 2400 MHz |
| `full7.log` | `xnu-full7.img` | first clean iOS audio through the internal speakers (`sound=4 sound_gain=0`), Wi-Fi scan/join |
| `flip1.log` | `xnu-flip1.img` | the double-buffered page flip (`fb_flip=1`) that fixed tearing |
| `cool1.log` | `xnu-cool1.img` | thermal governor and `cpu_mhz` ceiling under load |
| `gpu645.log` | `xnu-gpu645.img` | GPU at 645 MHz, Wi-Fi association details |
| `island2.log` | `xnu-island2.img` | a failed Dynamic Island attempt (SpringBoard crash loop) — what not to do |
| `android-amp-comparison.log` | — | the Android session that dumped AW88261 registers while playing (led to the TDM fix) |

Useful filters:

```sh
grep -a 'sound: \|speaker: '  full7.log     # audio graph and amplifier states
grep -a 'lx: \|qca6490'        daily1.log    # Wi-Fi bring-up
grep -a 'qcabt'                daily1.log    # Bluetooth
grep -a 'adreno7'              gpu645.log    # GPU steps
```
