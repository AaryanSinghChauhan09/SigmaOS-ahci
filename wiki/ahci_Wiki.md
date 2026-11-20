# SigmaOS-ahci Component Wiki

## Overview
SigmaOS-ahci provides Advanced Host Controller Interface storage support for the SigmaOS operating system. It demonstrates the storage subsystem and hardware controller integration within the SigmaOS ecosystem, following the principles of clear abstraction and deterministic storage initialization.

## Principles
- Single Responsibility
- Storage Abstraction
- Fail-Fast on Storage Initialization Errors
- Deterministic Storage Setup
- Clear Abstraction Over Storage APIs
- Composition over Inheritance for storage extensions

## Architecture
SigmaOS-ahci follows a storage pattern:
- **Storage Core**: Provides AHCI controller management, disk enumeration, and SATA operation
- **Device Detection**: Supports SATA device discovery, identification, and configuration
- **Operation Management**: Supports read/write operations, power management, and hot-plug handling
- **Error Handling**: Fail-fast on invalid storage configurations or unsupported controller types

## API
The component provides the following public APIs:
- `storage::ahci::controller()` - Get AHCI controller information and capabilities
- `storage::ahci::device::list()` - List detected SATA devices and storage hardware
- `storage::ahci::operation::read(device, offset, length)` - Perform SATA read operation
- `storage::ahci::operation::write(device, offset, data)` - Perform SATA write operation
- Error types for storage failure scenarios or unsupported controller types

## Test Structure (Arrange-Act-ASSERT)
All tests follow the AAA pattern:
1. **Arrange**: Set up test environment with specific storage configuration
2. **Act**: Execute the code under test (controller detection, operation execution)
3. **Assert**: Verify the expected storage results

Example:
```rust
#[test]
fn test_storage_ahci_aaa() {
    // Arrange
    let controller = storage::ahci::controller();
    
    // Act
    let devices = storage::ahci::device::list();
    
    // Assert
    assert!(devices.is_ok());
    let found_devices = devices.unwrap();
    assert!(!found_devices.is_empty());
}
```

## Development
- Follows Rust best practices with `#![forbid(unsafe_code)]`
- Storage initialization is best-effort; gracefully degrades on unsupported configurations
- Comprehensive error types for all storage failure scenarios
- Unit tests provided for controller detection and operation execution
- Integration tests for full storage subsystem with actual AHCI hardware

## TODO
- Add support for additional SATA features (NCQ, hot-plug, power management)
- Expand device detection capabilities (vendor IDs, device models, firmware versions)
- Add operation features (SMART monitoring, error logging, retry logic)
- Create integration tests with actual AHCI hardware and storage operations
- Implement storage persistence and configuration management