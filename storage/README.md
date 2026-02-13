# Storage Templates

This directory contains templates for file storage and transfer solutions.

## Supported Storage Solutions

### File Transfer
- **FTP Server** - File Transfer Protocol server (vsftpd, ProFTPD)
- **SFTP** - SSH File Transfer Protocol
- **FTPS** - FTP over SSL/TLS

### Object Storage
- **MinIO** - High-performance object storage
- **SeaweedFS** - Distributed file system

### Network Storage
- **Samba** - SMB/CIFS file sharing
- **NFS** - Network File System

### Cloud Storage Sync
- **Nextcloud** - Self-hosted cloud storage
- **ownCloud** - Personal cloud storage
- **Syncthing** - Continuous file synchronization

## Template Requirements

Each storage template should include:
- Service version
- Port configuration
- Storage capacity settings
- User authentication
- Access permissions
- Backup configuration
- Encryption options

## Usage Notes

- Always use encryption for sensitive data
- Set appropriate user quotas
- Regular backups of stored data
- Monitor disk space usage
- Configure access controls properly
