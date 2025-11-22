# InfiniBand Cleanup Role

This Ansible role provides comprehensive cleanup of InfiniBand/MLNX OFED packages and their dependencies from Ubuntu systems.

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

**Note**: The role uses a two-stage approach:

1. First, it identifies and removes specific installed packages
2. Then, it applies wildcard patterns to catch any remaining packages

## How it works

1. **Discovery phase**: Scans installed packages using `dpkg -l` to find InfiniBand-related packages
2. **Targeted removal**: Uses Ansible's apt module to safely remove identified packages
3. **Wildcard cleanup**: Applies wildcard patterns to catch any remaining packages
4. **Cleanup**: Runs autoremove to clean up orphaned dependencies

## Role Variables

- `infiniband_cleanup_enabled`: Whether to perform InfiniBand/MLNX OFED cleanup (default: `false`)
- `infiniband_cleanup_packages`: List of packages and patterns to remove (see defaults/main.yml for full list)
- `infiniband_cleanup_search_patterns`: List of search patterns used to find InfiniBand packages (see defaults/main.yml for full list)
- `infiniband_cleanup_wildcard_patterns`: List of wildcard patterns for additional cleanup (see defaults/main.yml for full list)
- `infiniband_cleanup_autoremove`: Whether to run autoremove after package cleanup (default: `true`)
- `infiniband_cleanup_debug`: Whether to display debug information (default: `true`)

## Example Playbook

```yaml
- hosts: gpu_nodes
  become: yes
  roles:
    - role: infiniband_cleanup
      vars:
        infiniband_cleanup_enabled: true
```
