# Maintaining protobuf specs

The Penumbra project dynamically generates code for interfacing
with [gRPC]. The following locations within the repository
are relevant:

  * `proto/proto/penumbra/**/*.proto`, the developer-authored spec files
  * `proto/src/gen/*.rs`, the generated Rust code files
  * `tools/proto-compiler/`, the build logic for generated the Rust code files

We use [buf] to auto-publish the protobuf schemas at
[buf.build/penumbra-zone/penumbra][protobuf], and to generate Go and Typescript packages.
The Rust code files are generated with our own tooling, located at `tools/proto-compiler`.

Our custom tooling for generating the Rust files will also shape the Serde implementations
of the derived Rust types to have more favorable JSON output (such as rendering
addresses as [Bech32]-encoded strings).

## Installing protobuf

Obtain the most recent pre-compiled binary from the [`protoc` website].
After installing, run `protoc --version` and confirm you're running
at least `3.21.8` (or newer). Don't install `protoc` from package managers
such as `apt`, as those versions are often outdated, and will not work
with Penumbra.

## Building protobuf

Switch to the [proto-compiler] directory and run the tool:

```shell
cd tools/proto-compiler
cargo run
```

Then run `git status` to determine whether any changes were made.
The build process is deterministic, so regenerating multiple times
from the same source files should not change the output.
A possible exception to this rule is if `prost` makes a superficial
change to the output that isn't substantive.

If the generated output would change in any way, CI will
fail, prompting the developer to commit the changes.

[`protoc` website]: https://grpc.io/docs/protoc-installation/#install-pre-compiled-binaries-any-os
[proto-compiler]: https://github.com/penumbra-zone/penumbra/tree/main/tools/proto-compiler
[gRPC]: https://grpc.io/
[protobuf]: https://buf.build/penumbra-zone/penumbra
[buf]: https://buf.build/
[Bech32]: https://en.bitcoin.it/wiki/Bech32