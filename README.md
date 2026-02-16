# FlexDmanager

![License](https://img.shields.io/badge/license-MIT-green)
![Language](https://img.shields.io/badge/language-Shell-orange)

A powerful and flexible system management tool for Linux and BSD operating systems. FlexDmanager provides comprehensive system utilities for managing processes, files, and system resources with ease.

## Table of Contents

- [Features](#features)
- [Screenshots](#screenshots)
- [Requirements](#requirements)
- [Installation](#installation)
  - [Linux](#linux)
  - [BSD](#bsd)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [Uninstallation](#uninstallation)
- [Contributing](#contributing)
- [License](#license)
- [Support](#support)

## Features

✨ **Core Features:**
- 📊 Real-time system monitoring and statistics
- 🎛️ Process and resource management
- 🔐 Secure file operations
- ⚡ High performance and lightweight design
- 🖥️ Cross-platform support (Linux & BSD)
- 🎯 User-friendly command-line interface
- 📝 Comprehensive logging and reporting

## Screenshots

### Application Interface

![FlexDmanager Interface](src/Screenshot%20From%202026-02-15%2020-25-50.png)

*FlexDmanager main dashboard showing system status and management options*

## Requirements

### Minimum System Requirements

- **OS:** Linux (kernel 3.10+) or BSD (FreeBSD 10.0+, OpenBSD 5.8+, NetBSD 7.0+)
- **RAM:** 256 MB minimum
- **Disk Space:** 50 MB available
- **Shell:** Bash 4.0+

### Dependencies

- `bash` or compatible shell
- Standard Unix utilities (coreutils, procps-ng/procps)
- `grep`, `awk`, `sed`
- Optional: `curl` for remote operations

## Installation

### Linux

#### Ubuntu/Debian-based Systems

```bash
# Update package manager
sudo apt-get update

# Install dependencies
sudo apt-get install -y curl wget git

# Clone the repository
git clone https://github.com/Tony-crx/FlexDmanager.git
cd FlexDmanager

# Make the script executable
chmod +x src/FlexDmanager.sh

# Install to system
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager

# Verify installation
flexdmanager --version
```

#### CentOS/RHEL-based Systems

```bash
# Update package manager
sudo yum update -y

# Install dependencies
sudo yum install -y curl wget git

# Clone the repository
git clone https://github.com/Tony-crx/FlexDmanager.git
cd FlexDmanager

# Make the script executable
chmod +x src/FlexDmanager.sh

# Install to system
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager

# Verify installation
flexdmanager --version
```

#### Fedora

```bash
# Update package manager
sudo dnf update -y

# Install dependencies
sudo dnf install -y curl wget git

# Clone the repository
git clone https://github.com/Tony-crx/FlexDmanager.git
cd FlexDmanager

# Make the script executable
chmod +x src/FlexDmanager.sh

# Install to system
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager

# Verify installation
flexdmanager --version
```

#### Arch Linux

```bash
# Update package manager
sudo pacman -Syu

# Install dependencies
sudo pacman -S curl wget git

# Clone the repository
git clone https://github.com/Tony-crx/FlexDmanager.git
cd FlexDmanager

# Make the script executable
chmod +x src/FlexDmanager.sh

# Install to system
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager

# Verify installation
flexdmanager --version
```

### BSD

#### FreeBSD

```bash
# Update package manager
sudo pkg update

# Install dependencies
sudo pkg install -y curl wget git

# Clone the repository
git clone https://github.com/Tony-crx/FlexDmanager.git
cd FlexDmanager

# Make the script executable
chmod +x src/FlexDmanager.sh

# Install to system
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager

# Verify installation
flexdmanager --version
```

#### OpenBSD

```bash
# Update package manager
sudo pkg_add -u

# Install dependencies
sudo pkg_add curl wget git

# Clone the repository
git clone https://github.com/Tony-crx/FlexDmanager.git
cd FlexDmanager

# Make the script executable
chmod +x src/FlexDmanager.sh

# Install to system
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager

# Verify installation
flexdmanager --version
```

#### NetBSD

```bash
# Update package manager
sudo pkgin update

# Install dependencies
sudo pkgin install curl wget git

# Clone the repository
git clone https://github.com/Tony-crx/FlexDmanager.git
cd FlexDmanager

# Make the script executable
chmod +x src/FlexDmanager.sh

# Install to system
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager

# Verify installation
flexdmanager --version
```

## Usage

### Basic Commands

```bash
# Show help and available options
flexdmanager --help

# Display version information
flexdmanager --version

# Show system status
flexdmanager status

# List running processes
flexdmanager process list

# Monitor system resources
flexdmanager monitor

# Manage services
flexdmanager service start|stop|restart
```

### Advanced Usage

```bash
# Get detailed system information
flexdmanager system info

# Generate a system report
flexdmanager report generate

# Export logs
flexdmanager log export

# Configure settings
flexdmanager config edit
```

### Examples

```bash
# Monitor system performance in real-time
flexdmanager monitor --realtime

# Check specific process
flexdmanager process find nginx

# Generate weekly report
flexdmanager report generate --period weekly --output /tmp/report.txt
```

## Troubleshooting

### Common Issues

**Issue: Permission Denied**
```bash
# Solution: Make the script executable
chmod +x /usr/local/bin/flexdmanager
```

**Issue: Command Not Found**
```bash
# Solution: Verify installation path
which flexdmanager
# If not found, reinstall:
sudo cp src/FlexDmanager.sh /usr/local/bin/flexdmanager
```

**Issue: Bash Version Incompatible**
```bash
# Check your bash version
bash --version

# Upgrade bash (Ubuntu/Debian)
sudo apt-get install -y --only-upgrade bash

# Upgrade bash (CentOS/RHEL)
sudo yum install -y --setopt=obsoletes=0 bash
```

**Issue: Missing Dependencies**
```bash
# Reinstall dependencies based on your system
# Linux: sudo apt-get install coreutils procps-ng grep gawk sed
# BSD: sudo pkg install coreutils proctools grep gawk sed
```

### Getting Help

- Check the help menu: `flexdmanager --help`
- View logs: `flexdmanager log show`
- Open an issue: [GitHub Issues](https://github.com/Tony-crx/FlexDmanager/issues)

## Uninstallation

### Remove FlexDmanager

```bash
# Remove from system
sudo rm /usr/local/bin/flexdmanager

# Optional: Remove configuration files
rm -rf ~/.flexdmanager

# Optional: Remove source code
rm -rf ~/FlexDmanager
```

## Contributing

We welcome contributions! Here's how you can help:

1. **Fork the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/FlexDmanager.git
   ```

2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes and commit**
   ```bash
   git add .
   git commit -m "Add your descriptive commit message"
   ```

4. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Submit a Pull Request**
   - Provide a clear description of your changes
   - Reference any related issues
   - Include test cases if applicable

### Code Style Guidelines

- Use clear and descriptive variable names
- Add comments for complex logic
- Follow standard shell script conventions
- Test on both Linux and BSD before submitting

## License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2026 Tony-crx

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

## Support

### Need Help?

- 📧 **Email:** [Create an issue](https://github.com/Tony-crx/FlexDmanager/issues)
- 🐛 **Bug Reports:** [GitHub Issues](https://github.com/Tony-crx/FlexDmanager/issues)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/Tony-crx/FlexDmanager/discussions)
- 📖 **Documentation:** Check the [wiki](https://github.com/Tony-crx/FlexDmanager/wiki)

### Follow Us

- ⭐ Star this repository if you find it helpful
- 🔔 Watch for updates and new releases
- 🤝 Share with your network

---

**Made with ❤️ by [Tony-crx](https://github.com/Tony-crx)**

Last Updated: February 15, 2026
