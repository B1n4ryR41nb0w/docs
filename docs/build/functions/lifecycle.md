# Lifecycle

Understand the full journey of Serverless Functions in Juno—from setup and development to deployment and ongoing maintenance.

---

## Workflow

Incorporating custom Functions into a Juno Satellite requires developers to prepare their project with the necessary Rust environment. This foundational step ensures the capability to develop tailored logic within the Satellite.

### Initial Setup

To start developing Functions, at the root of your dApp's source code, execute the `juno dev eject` command via the CLI. This command configures your project with the required Rust setup, including a `Cargo.toml` for dependencies and a `lib.rs` file for defining your Functions.

### Developing Functions

Once your project is scaffolded, proceed to define Rust functions and annotate them with Juno-specific macros to indicate the custom behavior of your Functions. Utilize the CLI to build your Functions by executing `juno dev build`.

### Local Development

For local development and testing, a sandbox environment is essential. With Docker installed and using the CLI, you can establish this environment by running `juno dev start`. Alternatively, manual setup instructions are available in the [documentation](../../guides/local-development.md) for a more customized approach.

:::info

The Docker container for the local sandbox environment supports hot reloading. This means that the container will automatically redeploy your local Satellite each time `juno dev build` is executed and a new version is produced. This feature streamlines the development process, allowing for immediate feedback and faster iteration of your Functions.

:::

### Deployment

After finalizing and testing your Functions, the next step is to deploy your modified Satellite to the mainnet. The `juno dev build` command produces a compressed WASM file (`satellite.wasm.gz`) that includes both the optimized code and necessary metadata. This file is located at `./target/deploy/satellite.wasm.gz` relative to your project's root directory. To deploy your Satellite, use the following CLI command from the same root directory:

```bash
juno upgrade -t s -s target/deploy/satellite.wasm.gz
```

This process updates your Satellite with the custom Functions, making them live on the mainnet for real-world application and interaction.

### Summary

| CLI Command                                              | Short description                                                            |
| -------------------------------------------------------- | ---------------------------------------------------------------------------- |
| `juno dev eject`                                         | Initializes your project to extend a Satellite.                              |
| `juno dev start`                                         | Starts the local development environment using Docker.                       |
| `juno dev build`                                         | Compiles your custom Satellite's code. Changes are automatically redeployed. |
| `juno upgrade -t s -s ./target/deploy/satellite.wasm.gz` | Upgrades your Satellite in production.                                       |

---

## **Maintenance**

After deployment, keeping your Satellite functional and optimized requires ongoing monitoring and updates. Staying up to date is also a key factor, as we may introduce new features that need to be integrated into your Satellite to ensure full functionality within the Juno Console.

Since your project includes all Satellite features using `include_satellite!();`, it's essential to stay in sync with Juno’s updates to maintain compatibility.

:::caution

Always upgrade iteratively and avoid skipping version numbers. While we strive to minimize breaking changes, it's crucial to upgrade through each released version sequentially.

For example, if you're on **v0.0.23** and the latest release is **v0.0.26**, first upgrade to **v0.0.24**, then **v0.0.25**, and finally **v0.0.26**. Skipping versions could lead to unexpected issues.

:::

### Updating Your Satellite

To upgrade your Satellite, bump the dependencies in your `Cargo.toml` file located in `/src/satellite/`. The key dependencies to check and update are:

- `junobuild-satellite`
- `junobuild-storage`
- `junobuild-macros`
- `ic_cdk`
- `candid`

If other crates in your project depend on these, they should also be upgraded accordingly.

The recommended versions for each release can be found in the [changelog](/changelog) or [release notes on GitHub](https://github.com/junobuild/juno/releases).

If you need assistance, feel free to reach out through the available support channels.
