# Redfin - a Bluefin Fork

Redfin extends [ghcr.io/ublue-os/bluefin-dx:stable](https://github.com/orgs/ublue-os/packages) with the package set and custom Just recipes I use on my daily workstation. It's built from [ublue-os/image-template](https://github.com/ublue-os/image-template), the standard scaffold for Universal Blue custom images.

# Bluefin
Bluefin is a cloud-native desktop operating system that reimagines the Linux desktop experience for modern computing environments.

# Changes
Packages to be added to the base image can be configured via `packages.json`. There are 2 sections. One for `packages` and one for `repos`. Repos should containe `copr` repositories and will be enabled with `dnf enable`.

Installation will use `build_files/build.sh` to parse `packages.json` and enable the packages before installing the packages.

Currently `packages.json` adds:

- `amfora` - terminal Gemini browser
- `cmatrix` - terminal eye candy
- `duf` - disk usage/free utility
- `eog` - Eye of GNOME image viewer
- `ghostty` - GPU-accelerated terminal emulator
- `globalprotect-openconnect` - GlobalProtect VPN client
- `lshw` - hardware lister
- `mise` - dev tool/runtime version manager
- `onefetch` - git repo summary tool
- `sassc` - required to build user GNOME Shell themes
- `unzip`
- `webapp-manager` - turns websites into desktop apps

See `SETUP.md` for the Flatpak and Brew packages installed post-install.

# Justfile Documentation

The `Justfile` contains various commands and configurations for building and managing container images and virtual machine images using Podman and other utilities.
To use it, you must have installed [just](https://just.systems/man/en/introduction.html) from your package manager or manually. It is available by default on all Universal Blue images.

## Environment Variables

- `image_name`: The name of the image (default: "redfin").
- `default_tag`: The default tag for the image (default: "latest").
- `bib_image`: The Bootc Image Builder (BIB) image (default: "quay.io/centos-bootc/bootc-image-builder:latest").

## Building The Image

### `just build`

Builds a container image using Podman.

```bash
just build $target_image $tag
```

Arguments:
- `$target_image`: The tag you want to apply to the image (default: `$image_name`).
- `$tag`: The tag for the image (default: `$default_tag`).

## Building and Running Virtual Machines and ISOs

The below commands all build QCOW2 images. To produce or use a different type of image, substitute in the command with that type in the place of `qcow2`. The available types are `qcow2`, `iso`, and `raw`.

### `just build-qcow2`

Builds a QCOW2 virtual machine image.

```bash
just build-qcow2 $target_image $tag
```

### `just rebuild-qcow2`

Rebuilds a QCOW2 virtual machine image.

```bash
just rebuild-vm $target_image $tag
```

### `just run-vm-qcow2`

Runs a virtual machine from a QCOW2 image.

```bash
just run-vm-qcow2 $target_image $tag
```

### `just spawn-vm`

Runs a virtual machine using systemd-vmspawn.

```bash
just spawn-vm rebuild="0" type="qcow2" ram="6G"
```

## File Management

### `just check`

Checks the syntax of all `.just` files and the `Justfile`.

### `just fix`

Fixes the syntax of all `.just` files and the `Justfile`.

### `just clean`

Cleans the repository by removing build artifacts.

### `just lint`

Runs shell check on all Bash scripts.

### `just format`

Runs shfmt on all Bash scripts.
