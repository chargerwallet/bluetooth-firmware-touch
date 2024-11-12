# ChargerWallet Touch Bluetooth Firmware

This repo contains bluetooth firmware for ChargerWallet Touch

The firmware is based on NRF5 SDK 16.0.0, and build with Makefile

## How to build

```shell
# make sure you have cmake, Python 3, amd aarm-none-eabi toolchain available in PATH

# export your OWN key for firmware signing
export BT_SIG_PK=$(cat <<EOF
-----BEGIN EC PRIVATE KEY-----
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
-----END EC PRIVATE KEY-----
EOF
)

# build binaries first
./make_binaries.sh

# then convert binaries to images with signature
./make_images.sh
```

## How to verify firmware hash

Install Python 3.x

Download latest `ota.bin`, open a terminal in the same folder, invoke python, then run following code

```python
import struct, hashlib

with open("ota.bin", mode="br") as f:
    f.seek(0x0C)
    codelen = struct.unpack("i", f.read(4))[0] - 512
    f.seek(0x600)
    print("".join(format(x, "02x") for x in hashlib.sha256(f.read(codelen)).digest()))
```

Single line version

```python
exec("""\nimport struct, hashlib\nwith open("ota.bin", mode="br") as f:\n    f.seek(0x0C)\n    codelen = struct.unpack("i", f.read(4))[0] - 512\n    f.seek(0x600)\n    print("".join(format(x, "02x") for x in hashlib.sha256(f.read(codelen)).digest()))\n""")
```

## License

Please check License.md for details
