# action-autobuild-release

Collects autobuild artifacts from previous jobs and uploads them to GitHub Releases.

## Use

The following is an example of using **action-autobuild-release** with
**action-autobuild**:

```yaml
name: Build
on: [push]
jobs:
  build:
    strategy:
      matrix:
        os: [windows-2022, macos-11, ubuntu-22.04]
        addrsize: ["64"]
        include:
          - os: windows-2022
            addrsize: "32"
    runs-on: ${{ matrix.os }}
    steps:
      - uses: secondlife/action-autobuild@v3
        with:
          addrsize: ${{ matrix.addrsize }}
  release:
    needs: build
    runs-on: [ubuntu-latest]
    if: startsWith(github.ref, 'refs/tags/v')
    steps:
      - uses: secondlife/action-autobuild-release@main
        with:
          public: true
```
