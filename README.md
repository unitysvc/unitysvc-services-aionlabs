# UnitySVC Services - AionLabs

This repository hosts service data for AionLabs digital services on the UnitySVC platform.

## Overview

This repository contains:

- **Provider metadata**: Information about AionLabs as a service provider
- **Service offerings**: Technical specifications for AionLabs AI services
- **Service listings**: User-facing service information, pricing, and documentation

All data is automatically validated and uploaded to the UnitySVC platform when changes are merged to the `main` branch.

## Repository Structure

```
data/
├── provider.toml                    # AionLabs provider metadata
├── README.md                        # Provider documentation
├── docs/                            # Code examples and descriptions
│   ├── code-example.py
│   ├── code-example.js
│   ├── code-example.sh
│   └── description.md
└── services/                        # Service definitions
    ├── aion-1.0/
    │   ├── service.json             # Service offering
    │   └── svcreseller.json         # Service listing
    ├── aion-1.0-mini/
    │   ├── service.json
    │   └── svcreseller.json
    └── aion-rp-llama-3.1-8b/
        ├── service.json
        └── svcreseller.json
```

## Setup

### Install unitysvc-services

The `unitysvc-services` package provides CLI tools for validating and uploading service data:

```bash
pip install unitysvc-services
```

### Install Pre-commit Hooks (Optional but Recommended)

Pre-commit hooks automatically validate and format your data files before each commit:

```bash
# Install pre-commit (choose one method)
pip install pre-commit           # Using pip
brew install pre-commit           # On macOS using Homebrew
conda install -c conda-forge pre-commit  # Using conda

# Install the git hooks in your repository
pre-commit install

# Test the hooks by running them manually on all files
pre-commit run --all-files
```

The pre-commit hooks will automatically:

- Format JSON files with 2-space indentation
- Validate JSON and TOML syntax
- Remove trailing whitespace
- Fix end-of-file formatting
- Validate data files against schemas

**Note**: You can also use `usvc data format` to format files manually without installing pre-commit hooks.

**Troubleshooting**: If the validate hook fails, ensure `unitysvc-services` is installed in your current Python environment:

```bash
pip install unitysvc-services
# Or if using a virtual environment, activate it first
source venv/bin/activate  # On Linux/macOS
# or
venv\Scripts\activate     # On Windows
```

## Development Workflow

### Format Data Files

Before committing, format your data files to match pre-commit requirements:

```bash
# Format all files in the data directory
usvc data format data

# Check if files are properly formatted without modifying them
usvc data format data --check
```

### Validate Data Locally

Before committing changes, validate your data files:

```bash
# Validate all files in the data directory
usvc data validate data

# The validator checks:
# - Schema compliance
# - File references exist
# - Jinja2 template syntax
# - Directory name consistency
# - URL formats
```

### Upload Data Manually

You can manually upload data to a UnitySVC backend:

```bash
# Set environment variables
export UNITYSVC_API_URL="https://api.unitysvc.com/v1"
export UNITYSVC_API_KEY="your-api-key"

# Upload all services (provider + offering + listing uploaded together)
usvc services upload data
```

### Populate Services (if configured)

If your provider has a `services_populator` section in provider.toml, you can update services automatically:

```bash
# Populate services for all providers
usvc data populate data

# Populate for specific provider
usvc data populate data --provider aionlabs

# Dry-run to preview what would be executed
usvc data populate data --dry-run
```

### Service Lifecycle Management

After uploading, manage service status:

```bash
# List uploaded services
usvc services list

# Submit a draft service for review
usvc services submit <service-id>

# Deprecate an active service
usvc services deprecate <service-id>

# Withdraw a submitted service back to draft
usvc services withdraw <service-id>

# Delete a service
usvc services delete <service-id>
```

## GitHub Actions

This repository includes automated workflows:

### Validate Data

Runs on every pull request and push to main:

- Validates all data files against schemas
- Ensures data integrity before merging

### Format Check

Runs on every pull request and push to main:

- Checks JSON/TOML formatting
- Ensures consistent code style

### Upload Data

Runs automatically when changes are merged to `main`:

- Uploads providers, service offerings, and service listings together
- Updates the configured UnitySVC backend

## GitHub Secrets Configuration

To enable automatic uploading when changes are merged to `main`, configure the following GitHub secrets:

### Step 1: Navigate to Repository Settings

1. Go to your repository on GitHub
2. Click **Settings** tab
3. In the left sidebar, click **Secrets and variables** → **Actions**

### Step 2: Add Secrets

Click **New repository secret** and add the following secrets:

#### `SERVICE_BASE_URL`

- **Description**: The UnitySVC backend API URL
- **Example values**:
  - Production: `https://api.unitysvc.com/v1`
  - Staging: `https://staging.unitysvc.com/v1`
  - Development: `https://main.devel.unitysvc.com/v1`

#### `UNITYSVC_API_KEY`

- **Description**: API key for authenticating with the UnitySVC backend
- **How to obtain**:
  1. Log in to the UnitySVC platform, the username should match the "seller" of service listings.
  2. Navigate to **Settings** → **API Keys**
  3. Generate a new API key for yourself
  4. Copy the key (you won't be able to see it again)

### Step 3: Verify Configuration

After adding the secrets:

1. Make a change to a file in the `data/` directory
2. Create a pull request
3. Merge the pull request to `main`
4. Check the **Actions** tab to see the upload workflow run
5. Verify that data appears on your UnitySVC backend

## File Formats

Both JSON and TOML formats are supported for service data. JSON files support JSON5 syntax (comments, trailing commas) for easier editing, but are written as standard JSON for compatibility.

### JSON Format

```json
{
  "schema": "offering_v1",
  "name": "aion-1.0",
  "service_type": "llm",
  "display_name": "Aion 1.0",
  "description": "Advanced AI model for general-purpose tasks"
}
```

### TOML Format

```toml
schema = "offering_v1"
name = "aion-1.0"
service_type = "llm"
display_name = "Aion 1.0"
description = "Advanced AI model for general-purpose tasks"
```

## Contributing

1. Create a new branch for your changes
2. Make your changes to files in the `data/` directory
3. Run `usvc data validate data` to check your changes
4. Commit your changes (pre-commit hooks will run automatically)
5. Push your branch and create a pull request
6. Once approved and merged, data will be automatically uploaded

## Support

For issues or questions:

- **UnitySVC Provider SDK**: https://github.com/unitysvc/unitysvc-services
- **UnitySVC Documentation**: https://docs.unitysvc.com
- **AionLabs**: Contact your AionLabs representative

## License

This repository contains proprietary service data for AionLabs. Unauthorized use or distribution is prohibited.
