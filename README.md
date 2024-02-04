# Nix packaged Flask webapp

This is a simple Flask webapp that is packaged using Nix as a docker container. The build is thus reproducible and the result should be same every time it is built unlike the other methods of building docker containers.

## Usage

Nix package manager must be installed on the system, follow this install [guide](https://nixos.org/download#download-nix) to install Nix. Then experimental Flake support needs to be enabled to use the `flake.nix` and not the legacy way of using Nix, add the following to `~/.config/nix/nix.conf`:
```
experimental-features = nix-command flakes
```

Verify the Nix install with Flake support by running the following command (runs GNU Hello):
```bash
nix run nixpkgs#hello
```
It should print `Hello, world!`. It that works, then you are good to go.

To build the docker container, run the following command (for x86_64-linux):
```bash
# you may use x86_64-darwin, aarch64-linux and aarch64-darwin for other platforms
nix build .#docker-image.x86_64-linux
```
This creates the docker container with the webapp and the dependencies as a tarball `result` (which is a symlink to the actual tarball).

Load the tarball into docker, run the following command:
```bash
docker load -i result
```

Run the docker container with the following command:
```bash
docker run -p 8000:8000 --rm myapp
```

Visit `http://localhost:8000` in your browser to see the webapp running with the message `Hello, World!`.

