<div align="center">

# [cargo-featalign action](https://github.com/hack-ink/cargo-featalign-action)
### Use [`cargo-featalign`](https://github.com/hack-ink/cargo-featalign) to check your crate's features.

</div>

### Introduction
**This will print the dry run output. If there is no output, it means your crate is good to go.**

### Usage
**Take [polkadot-sdk](https://github.com/paritytech/polkadot-sdk) as an example.**
```sh
gh repo clone paritytech/polkadot-sdk
```

**Check single crate.**
```yml
name: Checks
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
jobs:
  features-check:
    name: Task check features
    runs-on: ubuntu-latest
    steps:
      - name: Fetch latest code
        uses: actions/checkout@v4
      - name: Check
        uses: hack-ink/cargo-featalign-action@v0.1.0
        with:
          crate: polkadot/runtime/polkadot
          features: std,runtime-benchmarks,try-runtime
          default-std: true
```

**Check multiple crates at once.**
```yml
name: Checks
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
jobs:
  runtime-checks:
    name: Task check runtimes
    strategy:
      matrix:
        target: [polkadot/runtime/polkadot, polkadot/runtime/kusama]
    runs-on: ubuntu-latest
    steps:
      - name: Fetch latest code
        uses: actions/checkout@v4
      - name: Check ${{ matrix.target.chain }}
        uses: hack-ink/cargo-featalign-action@v0.1.0
        with:
          crate: polkadot
          features: std,runtime-benchmarks,try-runtime
          default-std: true
```
