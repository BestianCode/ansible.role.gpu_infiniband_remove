# InfiniBand Cleanup Role

This Ansible role provides comprehensive cleanup of InfiniBand/MLNX OFED packages and their dependencies from Ubuntu systems.

## Installation

Install from Ansible Galaxy (preferred):

```bash
ansible-galaxy role install bestiancode.gpu_infiniband_remove
```

Or install directly from GitHub:

```bash
ansible-galaxy role install git+https://github.com/BestianCode/ansible.role.gpu_infiniband_remove.git
```

Sample `requirements.yml` entry:

```yaml
roles:
  - name: bestiancode.gpu_infiniband_remove
```

## Features

- **Intelligent package detection**: Automatically identifies installed InfiniBand/MLNX packages
- **Robust removal process**: Handles missing packages gracefully without failing
- **Comprehensive coverage**: Removes both specific packages and catches remaining ones with wildcard patterns
- **Safe operation**: Only attempts to remove packages that are actually installed

## What it removes

This role removes:

- **MLNX OFED kernel modules and utilities**: Core OFED components
- **InfiniBand libraries and tools**: Including MLNX-customized versions with special version patterns (e.g., 2507mlnx58-1.2507097)
- **MLNX-specific tools**: ethtool, iproute2, fw-updater, and management tools
- **OpenSM packages**: Subnet Manager components and documentation
- **KNEM packages**: Kernel module and userspace tools
- **Development packages**: Headers and development libraries for IB/RDMA
- **RDMA core components**: Core RDMA infrastructure and libraries

## Role Variables

- `infiniband_cleanup_enabled`: Whether to perform InfiniBand/MLNX OFED cleanup (default: `false`)
- `infiniband_cleanup_packages`: List of packages and patterns to remove (see defaults/main.yml for full list)
- `infiniband_cleanup_search_patterns`: List of search patterns used to find InfiniBand packages (see defaults/main.yml for full list)
- `infiniband_cleanup_wildcard_patterns`: List of wildcard patterns for additional cleanup (see defaults/main.yml for full list)
- `infiniband_cleanup_autoremove`: Whether to run autoremove after package cleanup (default: `true`)
- `infiniband_cleanup_debug`: Whether to display debug information (default: `true`)

## Usage

Minimal playbook example:

```yaml
- hosts:
    - gpu_nodes
  become: true
  roles:
    - role: bestiancode.gpu_infiniband_remove
      vars:
        infiniband_cleanup_enabled: true
```
