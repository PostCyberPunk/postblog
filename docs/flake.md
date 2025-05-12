# Flake Outputs Documentation

In Nix, the `outputs` of a flake define the components or functionalities that the flake provides. These components can be referenced by other flakes or users. Below is a comprehensive list of what can be included in the `outputs` of a `flake.nix` file.

## Common Fields in `outputs`

### 1. **`defaultPackage`**
- Specifies the default package to build or run.
- Typically used with `nix run` or `nix build`.

```nix
outputs = { self, nixpkgs }: {
  defaultPackage.x86_64-linux = pkgs.hello;
};
```

---

### 2. **`defaultApp`**
- Defines the default application to run via `nix run`.
- Provides an entry point for the application.

```nix
outputs = { self, nixpkgs }: {
  defaultApp = {
    type = "app";
    program = "${pkgs.hello}/bin/hello";
  };
};
```

---

### 3. **`packages`**
- Lists multiple packages, often categorized by system architecture.
- Allows users to build specific packages using `nix build .#packageName`.

```nix
outputs = { self, nixpkgs }: {
  packages.x86_64-linux = {
    myPackage = pkgs.hello;
  };
};
```

---

### 4. **`apps`**
- Similar to `defaultApp`, but lets you define multiple applications.
- Each app can have its own entry point.

```nix
outputs = { self, nixpkgs }: {
  apps.x86_64-linux = {
    hello = {
      type = "app";
      program = "${pkgs.hello}/bin/hello";
    };
  };
};
```

---

### 5. **`nixosConfigurations`**
- Provides NixOS system configurations.
- Can be used with `nixos-rebuild` to configure NixOS systems.

```nix
outputs = { self, nixpkgs }: {
  nixosConfigurations.myMachine = nixpkgs.lib.nixosSystem {
    system = "x86_64-linux";
    modules = [ ./configuration.nix ];
  };
};
```

---

### 6. **`nixosModules`**
- Offers reusable NixOS modules for configuration.

```nix
outputs = { self, nixpkgs }: {
  nixosModules.myModule = ./modules/myModule.nix;
};
```

---

### 7. **`checks`**
- Defines tests or checks, typically for CI/CD or validation.

```nix
outputs = { self, nixpkgs }: {
  checks.x86_64-linux = {
    test = pkgs.runCommand "test" {} ''
      echo "Running tests"
    '';
  };
};
```

---

### 8. **`devShells`**
- Provides development environments (shells).
- Can be accessed via `nix develop`.

```nix
outputs = { self, nixpkgs }: {
  devShells.x86_64-linux.default = pkgs.mkShell {
    buildInputs = [ pkgs.hello ];
  };
};
```

---

### 9. **`overlay`**
- Supplies a custom overlay for Nixpkgs, allowing extension or modification of existing packages.

```nix
outputs = { self, nixpkgs }: {
  overlay = final: prev: {
    myPackage = final.hello;
  };
};
```

---

### 10. **`templates`**
- Defines templates for project initialization.
- Can be used with `nix flake init`.

```nix
outputs = { self, nixpkgs }: {
  templates.default = ./templates/default.nix;
};
```

---

### 11. **`lib`**
- Provides utility functions or reusable Nix expressions.

```nix
outputs = { self, nixpkgs }: {
  lib = {
    myFunction = x: x + 1;
  };
};
```

---

### 12. **`test`**
- Defines test logic or environments.

```nix
outputs = { self, nixpkgs }: {
  test = {
    example = pkgs.runCommand "example-test" {} ''
      echo "This is a test"
    '';
  };
};
```

---

### 13. **`formatter`**
- Supplies code formatters, often used for maintaining code style.

---

### 14. **`docs`**
- Provides documentation or a way to build/access them.

```nix
outputs = { self, nixpkgs }: {
  docs = {
    manual = ./docs/manual.md;
  };
};
```

---

### 15. **`darwinConfigurations`**
- Similar to `nixosConfigurations`, but for macOS systems.

```nix
outputs = { self, nixpkgs, darwin }: {
  darwinConfigurations.myMac = darwin.lib.darwinSystem {
    system = "x86_64-darwin";
    modules = [ ./mac-configuration.nix ];
  };
};
```

---

### 16. **`legacyPackages`**
- Provides compatibility for legacy Nixpkgs users.

---

## Comprehensive Example

```nix
{
  description = "Comprehensive flake example";

  inputs.nixpkgs.url = "github:NixOS/nixpkgs";

  outputs = { self, nixpkgs }: {
    packages.x86_64-linux.default = pkgs.hello;

    devShells.x86_64-linux.default = pkgs.mkShell {
      buildInputs = [ pkgs.hello ];
    };

    nixosConfigurations.myMachine = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [ ./configuration.nix ];
    };

    templates.default = ./templates/default.nix;

    checks.x86_64-linux.test = pkgs.runCommand "test" {} ''
      echo "Running tests"
    '';
  };
}
```

## Summary
The `outputs` section in a flake is highly flexible and allows you to define various components such as packages, apps, development environments, NixOS configurations, templates, and more. You can customize it to fit the needs of your project and make it reusable for others.
