# custom-installer-video

## build with flakes

`.` indicates current directory

```bash
nix build .#nixosConfigurations.exampleIso.config.system.build.isoImage
```

## build with flakes | nixos-generators

```bash
nix run nixpkgs#nixos-generators -- --format iso --flake /path/to/flake#exampleIso -o result
```

## build no flakes

```bash
nix-build '<nixpkgs/nixos>' -A config.system.build.isoImage \\\n -I nixos-config=configuration.nix
```

## build no flakes | nixos-generators

```bash
nixos-generate --format iso --configuration ./configuration.nix -o result
```

## example flake

```nix
{
  # ...

  outputs = { nixpkgs, ... }@inputs:
    {
      nixosConfigurations = {

        default = nixpkgs.lib.nixosSystem {
          specialArgs = { inherit inputs; };
          modules = [ 
            ./hosts/primary/configuration.nix
          ];
        };

        exampleIso = nixpkgs.lib.nixosSystem {
          specialArgs = { inherit inputs; };
          modules = [ 
            ./hosts/isoimage/configuration.nix
          ];
        };

      };
    };
}
```

## module

```nix
{ pkgs, modulesPath, ... }: {

  imports = [
    "\${modulesPath}/installer/cd-dvd/installation-cd-minimal.nix"
  ];

  nixpkgs.hostPlatform = "x86_64-linux";

}                                                                                  
```
