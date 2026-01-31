# Building KeePassXC for ARM64 Linux

This guide explains how to build KeePassXC for ARM64 Linux systems.

## Prerequisites

- Docker installed on your system
- At least 4GB of free disk space
- At least 2GB of RAM for compilation

## Building with Docker

The recommended approach is to use Docker to build KeePassXC in a controlled environment, avoiding dependency conflicts on your host system.

### Step 1: Build the Docker image

```bash
# Build the Docker image for ARM64
docker build -f Dockerfile.arm64.build -t keepassxc-builder-arm64 .
```

### Step 2: Run the build process

```bash
# Create a directory to store the output
mkdir -p build-output

# Run the build container
docker run --rm \
  -v $(pwd)/build-output:/workspace \
  keepassxc-builder-arm64
```

### Step 3: Install the built package

Once the build completes, you'll find the compiled KeePassXC in the `build-output` directory:

```bash
# Extract the package
cd build-output
tar -xzf keepassxc-arm64-linux.tar.gz

# Install to system (as root)
sudo cp -r usr/local/bin/keepassxc /usr/local/bin/
sudo cp -r usr/local/share/ /usr/local/

# Or install to user directory
cp -r usr/local/bin/keepassxc ~/bin/
```

## Alternative: Using the Build Script

If your system has all the required dependencies installed, you can try the build script:

```bash
# Make the script executable
chmod +x build_keepassxc.sh

# Run the build process
./build_keepassxc.sh
```

**Note**: This approach may fail if your system has dependency conflicts, as discovered on your system.

## GitHub Actions for Automated Builds

For regular builds or building on different platforms, you might want to set up GitHub Actions. Create `.github/workflows/build.yml` in a repository:

```yaml
name: Build KeePassXC

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        arch: [arm64]

    steps:
    - uses: actions/checkout@v3
    
    - name: Set up QEMU
      uses: docker/setup-qemu-action@v2
      with:
        platforms: arm64
    
    - name: Build for ARM64
      run: |
        docker build -f Dockerfile.arm64.build -t keepassxc-builder-${{ matrix.arch }} .
        mkdir -p build-output
        docker run --rm -v $(pwd)/build-output:/workspace keepassxc-builder-${{ matrix.arch }}
    
    - name: Upload artifacts
      uses: actions/upload-artifact@v3
      with:
        name: keepassxc-${{ matrix.arch }}
        path: build-output/keepassxc-arm64-linux.tar.gz
```

## Result

After successful compilation, you will have:

- A portable binary package of KeePassXC for ARM64
- All necessary dependencies included in the build
- An installation package ready to deploy on ARM64 Linux systems

## Verification

To verify the build worked correctly:

```bash
# Check if the binary exists and is executable
file build-output/usr/local/bin/keepassxc
./build-output/usr/local/bin/keepassxc --version
```

The resulting binary will be statically linked where possible and suitable for distribution on ARM64 Linux systems.