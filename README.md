# Whippet Gitian signatures

Detached Gitian build signatures for [Whippet](https://github.com/chromatic/whippet-experimental-blockchain) releases.
Each signature records the SHA-256 of every build artifact, so independent builders can confirm
they reproduced the same binaries.

## Layout

```
<version>-<os>/<signer>/whippet-<os>-<series>-build.assert
<version>-<os>/<signer>/whippet-<os>-<series>-build.assert.sig
```

- `<version>` is the release, for example `1.4.0`.
- `<os>` is `linux`, `win-unsigned` or `osx-unsigned` (plus `win-signed` and `osx-signed` for the signed packages).
- `<signer>` is the name you used with `--signer` / `--sign`.
- `<series>` comes from the descriptor `name:`. The `v1.4.0` tag still says `1.3`; later descriptors say `1.4`.

## Verifying

Import the signers' keys from `contrib/gitian-keys` in the main repository, then from a
`gitian-builder` checkout:

```
gpg --import ../whippet/contrib/gitian-keys/*.pgp
./bin/gverify -v -d ../whippet-gitian-sigs/ -r 1.4.0-linux ../whippet/contrib/gitian-descriptors/gitian-linux.yml
```

Repeat with `1.4.0-win-unsigned` and `1.4.0-osx-unsigned` and the matching descriptors.
All signers should report identical hashes. If they do not, do not use the release.

## Contributing signatures

1. Build with `contrib/gitian-build.sh --docker --build --sign <KEYID> <version>`.
2. Copy `gitian-output/sigs/<version>-<os>/<signer>/` into this repository.
3. Make sure your public key is in `contrib/gitian-keys` in the main repository, or open a PR there adding it.
4. Open a pull request here with only your own signer directories.
