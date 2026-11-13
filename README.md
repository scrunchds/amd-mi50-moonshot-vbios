# AMD Instinct MI50 16GB — HPE Moonshot VBIOS

VBIOS ROM dump from a genuine AMD Instinct MI50 16GB (HPE EdgLine / Moonshot variant) that presents itself as a Radeon VII (`PCI 1002:66af`, Vega 20). Useful as a restore backup or as a donor VBIOS for MI50 flash conversions.

## ROM details

| Field | Value |
|---|---|
| File | `113-D3600200-106.rom` |
| Size | 102400 bytes (image size as exposed by the PCI option ROM BAR) |
| Board ID | `113-D3600200-106` — "Vega20 A1 XT MOONSHOT D36002 16GB 1000m" |
| ATOMBIOS version | `016.004.000.038.011717` |
| Option ROM header device ID | `0x66af` |
| Memory | 16 GB HBM2 (Hynix), 1 GHz |
| MD5 | `7dbbfc818fe79594937a5acc255c91c8` |
| SHA-256 | `3fb484417de156c7500510c8059963bd0ea1b6724b4631f08b70085cda4a07d2` |

Dumped via the PCI option ROM BAR (`/sys/bus/pci/devices/…/rom`) on Linux with `amdgpu`, verified byte-identical across two dumps and matching the running version reported in `vbios_version`.

## Notes on device IDs

The card's PCI device ID comes from a hardware strap, not from the VBIOS: this Moonshot board straps to `66af` (Radeon VII) even though it carries MI50 firmware. Flashing this ROM will not change the ID a PCI scan reports. Moonshot-family MI50s are commonly spotted as "Radeon VII" in `lspci` for exactly this reason.

## Backup / restore

Always save your card's current VBIOS before flashing anything:

```sh
D=/sys/bus/pci/devices/0000:86:00.0   # your bus:dev.fn
echo 1 | sudo tee $D/rom
sudo cat $D/rom > my-card-backup.rom
echo 0 | sudo tee $D/rom
```

Flashing is done with AMD's `amdvbflash` (VBFlash) with the GPU driver unbound. Flashing the wrong image or losing power mid-flash can brick the card; recovery then requires an external SPI programmer. You do this at your own risk.

## License / provenance

AMD firmware; redistributed as-is for archival and repair purposes.
