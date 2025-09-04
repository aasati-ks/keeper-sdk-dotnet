# Keeper Commander - Usage Guide
**Keeper Commander** is a cross-platform command-line and scripting tool for managing your Keeper Password Manager vault and enterprise environment. With Commander, you can securely access, organize, and automate your password records, folders, and shared resources. It supports a wide range of features including authentication, vault operations, record and folder management, enterprise administration, device approvals, and advanced security controls. Commander is ideal for both individual users and IT administrators who want to integrate Keeper into their workflows, automate routine tasks, or manage large-scale deployments with ease and security.


## Table of Contents
- [Overview](#overview)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Authentication](#authentication)
- [Basic Commands](#basic-commands)
- [Vault Operations](#vault-operations)
- [Record Management](#record-management)
- [Folder Operations](#folder-operations)
- [Shared Folder Management](#shared-folder-management)
- [Device Management](#device-management)
- [Enterprise Commands](#enterprise-commands)
- [Advanced Features](#advanced-features)
- [Examples](#examples)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [API Integration](#api-integration)

## Overview

**Keeper Commander** is a powerful command-line interface (CLI) application built with the .NET SDK that provides comprehensive access to your Keeper Password Manager vault. It enables automation, scripting, and advanced management of your passwords, records, folders, and enterprise features.

### What Commander Can Do

- **Authentication**: Secure login with 2FA, biometric authentication (Windows Hello, TouchID)
- **Vault Management**: Access, search, and organize your password vault
- **Record Operations**: Create, read, update, and delete password records
- **Folder Management**: Organize records into folders and shared folders
- **Enterprise Administration**: Manage users, teams, roles, and enterprise settings
- **Device Management**: Handle device approval workflows
- **Security Features**: BreachWatch integration, record type management
- **Automation**: Script complex operations and integrate with other systems

### Key Benefits

- **Automation**: Automate password management tasks
- **Enterprise Control**: Advanced administrative capabilities
- **Integration**: Integrate Keeper into your existing workflows
- **Security**: Multi-factor authentication and advanced security features
- **Flexibility**: Command-line interface for power users and automation

## Installation

### Prerequisites

- **.NET Runtime**: 
  - **Linux/macOS**: .NET 8.0 or later
  - **Windows**: .NET Framework 4.7.2+ or .NET 8.0
- **Operating System**: Windows 10+, macOS 10.15+, or Linux (Ubuntu 18.04+/equivalent)
- **Keeper Account**: Active Keeper Security account

### Build from Source

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Keeper-Security/keeper-sdk-dotnet.git
   cd keeper-sdk-dotnet/Commander
   ```

2. **Build the Application**
   ```bash
   dotnet build -c Release
   ```

3. **Run Commander**
   ```bash
   # Linux and macOS (.NET 8.0)
   dotnet run --framework=net8.0
   
   # Windows (.NET Framework 4.7.2)
   dotnet run --framework=net472
   
   # Or use the built executable
   # Linux/macOS
   ./bin/Release/net8.0/Commander
   
   # Windows
   ./bin/Release/net472/Commander.exe
   ```

## Getting Started

### First Time Setup

1. **Launch Commander**
   ```bash
   ./Commander
   ```

2. **Login to Keeper**
   ```
   Keeper> login your.email@company.com
   ```

3. **Sync Your Vault**
   ```
   Keeper> sync-down
   ```

4. **List Your Records**
   ```
   Keeper> ls
   ```

### Basic Navigation

Commander uses a folder-based navigation system similar to a file system:

```bash
# Show current location
Keeper> pwd

# List contents
Keeper> ls

# Navigate to folder
Keeper> cd FolderName

# Go back to parent
Keeper> cd ..

# Go to root
Keeper> cd /
```

## Authentication

### Standard Login

```bash
# Login with username
login user@example.com

# Login with last used username
login
```

### Two-Factor Authentication

Commander supports multiple 2FA methods:
- **TOTP Apps** (Google Authenticator, Authy, etc.)
- **SMS** codes
- **Security Keys** (YubiKey, FIDO2 devices)


### Device Management

```bash
# View device approval requests
devices

# Approve specific device
devices --approve DEVICE_ID

# Deny device request
devices --deny DEVICE_ID
```

## Basic Commands

### Essential Commands

| Command | Alias | Description |
|---------|--------|-------------|
| `help` | `?` | Show available commands |
| `login` | | Login to Keeper |
| `logout` | | Logout from Keeper |
| `sync-down` | `d` | Download and sync vault |
| `quit` | `q` | Exit Commander |

### Getting Help

```bash
# Show all commands
help

# Get help for specific command
help login
help enterprise-user --help
```

## Vault Operations

### Synchronization

```bash
# Download latest vault data
sync-down
d  # alias

# Force full sync - This forces syncing record types and other extras
sync-down --force
```

### Search and Discovery

```bash
# Search records by title
search gmail

# Search with regular expressions
search ".*bank.*"

# Search in specific folder
cd Banking
search account

# Advanced search options
search --limit 10 gmail
search --folder "Work Accounts" password
```

### Record Display

```bash
# Display record details
get RecordTitle

# Show record history
record-history RecordTitle
```

## Record Management

### Creating Records

```bash
# Add a basic login record
add-record --title "Gmail Account" login=user@gmail.com password=securepassword123 --type=login

# Add record with URL
add-record --title "Banking" login=username password="pass123" url="https://bank.com" --type=login

# Add record with notes
add-record --title "Server Access" login=admin password="serverpass" notes="Production server credentials" --type=login

# Generate secure password
add-record --title "New Account" login=user@email.com --generate --type=login
```

### Updating Records

```bash
# Update password only
update-record <recordUID> password=newpassword123 --title "changed Gmail Account"
# Update both password and login fields
update-record <RECORDUID> password=somenewpassword login=newlogin@email.com

# Update multiple fields
update-record <RECORDUID> password="newpass" login="newemail@gmail.com"

# Update with generated password
update-record <RECORDUID> --generate

# Add notes to existing record
update-record <RECORDUID> --notes "Updated server access info"
```

### Record Types

```bash
# List available record types
record-type-info

# Add custom record type
record-type-add --name "Database" --fields "host,port,database,username,password"

# Update record type
record-type-update --id 123 --name "Updated Database Type"

# Load record types from file
load-record-types --file recordtypes.json
```

### Deleting Records

```bash
# Delete specific record
rm "Record Title"

# Delete multiple records (with confirmation)
rm "Record1" "Record2" "Record3"

# Force delete without confirmation
rm --force "Record Title"
```

## Folder Operations

### Basic Folder Commands

```bash
# Create folder
mkdir "Work Accounts"

# Create nested folders
mkdir "Personal/Banking/Savings"

# Delete folder (empty)
rmdir "Old Folder"

# Delete folder and contents
rmdir --force "Folder With Records"
```

### Navigation

```bash
# Change directory
cd "Work Accounts"

# Show folder tree
tree

# Show detailed tree
tree --verbose

# List folder contents
ls
ls -l  # detailed list
ls --format json  # JSON format
```

### Moving Records and Folders

```bash
# Move record to folder
mv "Gmail Account" "Personal/Email"

# Move folder
mv "Old Banking" "Personal/Financial"

```

## Shared Folder Management

### Shared Folder Operations

```bash
# List all shared folders
sf-list

# Create shared folder
mkdir --shared "Team Passwords"

# Manage user access
sf-user --folder "Team Passwords" --add user@company.com
sf-user --folder "Team Passwords" --remove user@company.com

# Set user permissions
sf-user --folder "Shared" --user user@company.com --permission read-write

# Manage record permissions in shared folder
sf-record --folder "Shared" --record "Server Access" --permission read-only
```

### Permission Levels

- **Owner**: Full control, can modify folder settings
- **Admin**: Can add/remove users and records
- **Read-Write**: Can view and modify records
- **Read-Only**: Can only view records

## Device Management

### Device Commands

```bash
# Show current device info
this-device

# List other devices
devices

# Show device approval queue
devices --pending

# Approve device
devices --approve DEVICE_ID --user user@company.com

# Deny device request
devices --deny DEVICE_ID

# Remove device
devices --remove DEVICE_ID
```

### Device Security

```bash
# Share data key with enterprise
share-datakey

# Update device settings
this-device --name "My Work Laptop"
this-device --timeout 30  # Set session timeout
```

## Enterprise Commands

### Enterprise Data Management

```bash
# Load enterprise data
enterprise-get-data

# Display enterprise structure
enterprise-node

# Show node tree
enterprise-node --node "Root Node" --verbose
```

### User Management

```bash
# List enterprise users
enterprise-user

# Add new user
enterprise-user --add --email "newuser@company.com" --name "New User"

# Transfer user data
enterprise-transfer-user --from "olduser@company.com" --to "newuser@company.com"

# Manage user status
enterprise-user --suspend user@company.com
enterprise-user --unsuspend user@company.com
enterprise-user --delete user@company.com

# Extend account sharing
enterprise-users --extend-sharing --users "user1@company.com,user2@company.com"
```

### Team Management

```bash
# List teams
enterprise-team

# Create team
enterprise-team --add --name "Development Team"

# Add users to team
enterprise-team --team "Development Team" --add-user "dev1@company.com"
enterprise-team --team "Development Team" --add-user "dev2@company.com"

# Remove users from team
enterprise-team --team "Development Team" --remove-user "olddev@company.com"

# Delete team
enterprise-team --delete "Old Team"
```

### Role Management

```bash
# List enterprise roles
enterprise-role

# Create role
enterprise-role --add --name "Security Admin"

# Assign users to role
enterprise-role --role "Security Admin" --add-user "admin@company.com"

# Assign teams to role
enterprise-role --role "Security Admin" --add-team "Admin Team"

# Set role permissions
enterprise-role --role "Security Admin" --permissions "admin,audit"
```

### Device Approval (Enterprise)

```bash
# Manage enterprise device approvals
enterprise-device

# Auto-approve devices for specific users
enterprise-device --auto-approve --users "trusted@company.com"

# Bulk approve devices
enterprise-device --approve-all --node "Sales Department"
```

## Advanced Features

### BreachWatch Integration

```bash
# Run BreachWatch scan
breachwatch

# Check specific records
breachwatch --records "Gmail,Banking"

# Generate security report
breachwatch --report --format json

# Set breach monitoring
breachwatch --monitor --email "admin@company.com"
```

### Attachments

```bash
# Upload attachment to record
upload-attachment --record "Document Storage" --file "/path/to/document.pdf"

# Download attachment
download-attachment --record "Document Storage" --attachment "document.pdf"

# Download all attachments from record
download-attachment --record "All Documents" --all
```

### Import/Export

```bash
# Import records from JSON
import-records --file records.json

# Export records to JSON
export-records --file backup.json

# Export specific folder
export-records --folder "Work Accounts" --file work_backup.json
```

## Examples

### Daily Operations

```bash
# Morning routine: sync and check for breached passwords
sync-down
breachwatch

# Find and update a specific account
search "github"
update-record "GitHub" --generate-password

# Check team shared folders
sf-list
cd "Team Shared"
ls -l
```

### Enterprise Administration

```bash
# Weekly user management
enterprise-get-data
enterprise-user --inactive --days 30
enterprise-device --pending

# Security audit
breachwatch --report
enterprise-user --list --format json > user_audit.json
```

### Automation Examples

```bash
# Batch password updates
for record in $(search "old-server" --format names); do
    update-record "$record" --generate-password
done

# Cleanup old records
search "temp-" --format names | xargs rm --force
```

## Configuration

### Configuration File

Commander stores configuration in your local system in a hidden folder.

### Custom Configuration
Its possible to provide custom config file when accessing commander.

```bash
# Use custom config file
Commander --config /path/to/custom/config.json

# Set default server
server "eu.keepersecurity.com"

```

### Environment Variables

```bash
# Set default username
export KEEPER_USERNAME="user@company.com"

# Set configuration directory  
export KEEPER_CONFIG_DIR="/custom/path"

# Set log level
export KEEPER_LOG_LEVEL="DEBUG"
```

## Troubleshooting

### Common Issues

#### Authentication Problems

```bash
# Clear stored credentials
logout
login --clear-cache user@example.com

# Check device approval status
devices --status
```

#### Sync Issues

```bash
# Force full vault sync
sync-down --force --verbose

# Clear local cache
sync-down --clear-cache

# Check connectivity
ping keepersecurity.com
```

#### Enterprise Features Not Available

```bash
# Verify enterprise admin status
enterprise-get-data
#returns invalid command if not admin, else returns nothing but loads enterprise data into storage

# Check permissions
enterprise-role --node=user_email

# Reload enterprise data
enterprise-get-data --force
```

### Getting Help

```bash
# Built-in help
command --help

# Online documentation
# Visit: https://docs.keeper.io/commander-cli/
```

## API Integration

### Using Commander as a Library

```csharp
using KeeperSecurity.Authentication;
using KeeperSecurity.Vault;

// Initialize authentication
var auth = new Auth(new AuthUi(), new JsonConfigurationStorage());
await auth.Login("user@example.com");

// Access vault
var vault = new VaultOnline(auth);
await vault.SyncDown();

// Work with records
var records = vault.KeeperRecords;
foreach (var record in records.Values)
{
    Console.WriteLine($"{record.Title}: {record.Login}");
}
```

### SDK Integration

For programmatic access, use the KeeperSdk library directly:

```csharp
// Add NuGet package: KeeperSecurity.Sdk
using KeeperSecurity.Authentication;
using KeeperSecurity.Vault;

var configuration = new JsonConfigurationStorage();
var auth = new Auth(new ConsoleAuthUI(), configuration);

await auth.Login("user@example.com");

var vault = new VaultOnline(auth);
await vault.SyncDown();

// Your automation code here
```

## Support and Resources

### Documentation
- **SDK Documentation**: [yet to be updated](yet to be updated)
- **API Reference**: [https://docs.keeper.io](https://docs.keeper.io)

### Community
- **GitHub**: [https://github.com/Keeper-Security/keeper-sdk-dotnet](https://github.com/Keeper-Security/keeper-sdk-dotnet)
- **Issues**: Report bugs and feature requests on GitHub

### Professional Support
- **Email**: commander@keepersecurity.com
- **Enterprise Support**: Contact your Keeper representative

---

**About Keeper Security**

Keeper is the leading cybersecurity platform for preventing password-related data breaches and cyberthreats. Learn more at [https://keepersecurity.com](https://keepersecurity.com).

**License**

This software is licensed under the MIT License. See LICENSE file for details.

**Version**: 1.0.0  
**Last Updated**: 2025
