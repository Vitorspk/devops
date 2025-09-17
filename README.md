# DevOps Virtual Store Infrastructure

A complete DevOps infrastructure setup for a virtual store application using Infrastructure as Code principles with Vagrant and Puppet.

## Overview

This project demonstrates a production-ready infrastructure setup for an e-commerce application called "Loja Virtual" (Virtual Store). It automates the provisioning and configuration of a multi-tier architecture using modern DevOps tools and practices.

## Architecture

The infrastructure consists of three virtual machines:

- **Database Server (`db`)**: MySQL database server at `192.168.33.10`
- **Web Server (`web`)**: Tomcat application server at `192.168.33.12`
- **Monitoring Server (`monitor`)**: Nagios monitoring server at `192.168.33.14`

## Prerequisites

- [Vagrant](https://www.vagrantup.com/) (2.0+)
- [VirtualBox](https://www.virtualbox.org/) or another Vagrant-supported provider
- [Git](https://git-scm.com/)
- Ruby (for Librarian-Puppet)

## Quick Start

1. Clone the repository:
```bash
git clone https://github.com/Vitorspk/devops.git
cd devops
```

2. Install Vagrant plugins:
```bash
vagrant plugin install vagrant-librarian-puppet
```

3. Start the virtual machines:
```bash
# Start all VMs
vagrant up

# Or start individual VMs
vagrant up db
vagrant up web
vagrant up monitor
```

4. Access the services:
- Web Application: http://192.168.33.12:8080/devopsnapratica/
- Admin Panel: http://192.168.33.12:8080/devopsnapratica/admin/
- HTTPS: https://192.168.33.12:8443
- Nagios Monitoring: http://192.168.33.14/nagios3 (user: nagiosadmin)

## Project Structure

```
.
├── Vagrantfile              # Vagrant configuration for VMs
├── manifests/               # Puppet manifests entry points
│   ├── db.pp               # Database server manifest
│   └── web.pp              # Web server manifest
├── modules/                 # Custom Puppet modules
│   ├── loja_virtual/       # Main application module
│   ├── mysql/              # MySQL configuration module
│   └── tomcat/             # Tomcat configuration module
├── librarian/              # Librarian-Puppet configuration
│   ├── Puppetfile          # External module dependencies
│   └── modules/            # Downloaded external modules
├── web-config              # Web server configuration files
└── .github/workflows/      # GitHub Actions CI/CD pipelines
```

## Configuration Management

### Puppet Modules

The project uses custom Puppet modules for configuration management:

- **`loja_virtual`**: Main module that orchestrates the application setup
- **`mysql`**: Handles MySQL server installation and database configuration
- **`tomcat`**: Manages Tomcat 7 installation and SSL configuration

### Key Features

- **Automated Database Setup**: MySQL server with configured schemas and users
- **Web Server Configuration**: Tomcat with SSL support and proper Java settings
- **Application Deployment**: Automated WAR file deployment
- **Infrastructure Monitoring**: Nagios setup with service checks
- **Idempotent Configuration**: Puppet ensures consistent state across runs

## Development

### Managing VMs

```bash
# Check VM status
vagrant status

# SSH into a VM
vagrant ssh db
vagrant ssh web
vagrant ssh monitor

# Provision/re-provision a VM
vagrant provision web

# Destroy and recreate VMs
vagrant destroy -f
vagrant up
```

### Applying Puppet Changes

After modifying Puppet manifests or modules:

```bash
# Re-run provisioning
vagrant provision [vm-name]

# Or SSH and apply manually
vagrant ssh web
sudo puppet apply /vagrant/manifests/web.pp --modulepath=/vagrant/modules:/vagrant/librarian/modules
```

## Application Details

The deployed application is a Java-based e-commerce platform with:
- Frontend shopping interface
- Administrative backend
- MySQL database for product and order management
- SSL-enabled connections
- Session management

## Monitoring

The Nagios monitoring server checks:
- SSH service availability
- HTTP/HTTPS endpoints
- Database connectivity
- Disk usage
- Service uptime

## CI/CD Integration

The project includes GitHub Actions workflows for:
- Automated code reviews using Claude AI
- Infrastructure validation
- Continuous integration checks

## Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 8080, 8443, and 3306 are not in use
2. **Memory issues**: Each VM requires ~512MB RAM minimum
3. **Provisioning failures**: Check Puppet logs with `sudo puppet apply --debug`
4. **Network issues**: Verify VirtualBox network adapters are configured correctly

### Logs

- Tomcat logs: `/var/lib/tomcat7/logs/catalina.out`
- MySQL logs: `/var/log/mysql/error.log`
- Puppet logs: Run with `--debug` flag for detailed output

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is for educational and demonstration purposes. Please review the license file for more details.

## Support

For issues, questions, or suggestions, please open an issue on GitHub.

---

Built with ❤️ using Vagrant, Puppet, and modern DevOps practices.