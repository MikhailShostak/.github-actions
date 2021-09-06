# Predefined GitHub Actions

## Supported Platforms
Host    | Target         | Compiler
--------|----------------|---------------
macOS   | macOS x86_64   | Apple Clang 12
Ubuntu  | Ubuntu x86_64  | GCC 9
Windows | Windows x86_64 | MSVC 16

## Usage Example
``` yaml
jobs:
  build:
    steps:
      - name: Setup
        uses: ./.github-actions/platforms/ubuntu-gcc
      - name: Build
        uses: ./.github-actions/steps/build
```

## Workflow Template (recursive submodules)
``` yaml
name: Target
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:
jobs:
  macos:
    name: macOS (Apple Clang 12)
    runs-on: macos-latest
    strategy:
      matrix:
        platform:
          - arch: x86_64
        config:
          - Release
          - Debug
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          submodules: recursive
      - name: Setup
        uses: ./.github-actions/platforms/macos-apple-clang
      - name: Build
        uses: ./.github-actions/steps/build
  ubuntu:
    name: Ubuntu (GCC 9)
    runs-on: ubuntu-latest
    strategy:
      matrix:
        platform:
          - arch: x86_64
        config:
          - Release
          - Debug
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          submodules: recursive
      - name: Setup
        uses: ./.github-actions/platforms/ubuntu-gcc
      - name: Build
        uses: ./.github-actions/steps/build
  windows:
    name: Windows (MSVC 16)
    runs-on: windows-latest
    strategy:
      matrix:
        platform:
          - arch: x86_64
        config:
          - Release
          - Debug
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          submodules: recursive
      - name: Setup
        uses: ./.github-actions/platforms/windows-msvc
      - name: Build
        uses: ./.github-actions/steps/build
```
