# BigCommerce Coupon Generator - Application Overview

## What This Application Does

This is a **Node.js utility application** designed to bulk generate coupon codes for BigCommerce e-commerce stores. The application provides two main functionalities:

1. **Generate Coupon Codes in BigCommerce**: Creates coupon codes directly in a BigCommerce store via API
2. **Generate CSV of Codes**: Creates a CSV file with coupon codes without interacting with BigCommerce

## Core Functionality

### 1. Coupon Code Generation
- **Generates random alphanumeric codes** using uppercase letters (A-Z) and digits (0-9)
- **Configurable code length** between 6 and 50 characters
- **Duplicate prevention** - ensures no duplicate codes are generated
- **Configurable quantity** - generate any number of codes you need

### 2. BigCommerce Integration
- **API Integration** with BigCommerce using the `node-bigcommerce` package
- **Promotion Validation** - ensures the target promotion is a coupon promotion
- **Automatic Coupon Creation** - each generated code becomes a usable coupon in BigCommerce
- **Progress Tracking** - shows real-time progress during bulk creation
- **Error Handling** - validates API responses and handles errors gracefully

### 3. CSV Export
- **Timestamped files** - results saved with timestamp for organization
- **Resume capability** - if interrupted, the CSV contains codes successfully created
- **Standalone mode** - can generate codes without BigCommerce API for testing

## Application Architecture

```
coupon-generator/
├── generate-coupons.js     # Main script: Creates coupons in BigCommerce
├── generate-csv.js         # Utility script: Generates CSV only
├── lib/
│   ├── generate-codes.js   # Code generation logic
│   └── write-csv.js        # CSV file writing utility
├── results/                # Output directory for CSV files
├── .env-template          # Environment variables template
└── package.json           # Project configuration
```

## How It Works

### Code Generation Process:
1. **Random Generation**: Uses Math.random() to select characters from 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'
2. **Duplicate Check**: Recursively generates new codes if duplicates are detected
3. **Batch Processing**: Generates the specified quantity of unique codes

### BigCommerce Integration Process:
1. **Environment Setup**: Reads API credentials and configuration from environment variables
2. **API Authentication**: Creates BigCommerce client with store hash and API token
3. **Promotion Validation**: Verifies the promotion exists and is a coupon type
4. **Batch Creation**: Iterates through generated codes and creates each as a coupon
5. **Progress Tracking**: Updates terminal with creation progress
6. **Result Storage**: Saves successfully created codes to CSV for backup

## Configuration Requirements

The application requires these environment variables:

| Variable     | Description                                    | Example        |
|--------------|------------------------------------------------|----------------|
| STORE_HASH   | BigCommerce store identifier                   | abc123def      |
| API_TOKEN    | BigCommerce API token (Marketing: modify)     | v2_xxxx...     |
| QUANTITY     | Number of coupon codes to generate             | 100            |
| PROMOTION_ID | ID of the coupon promotion in BigCommerce      | 12345          |
| CODE_LENGTH  | Length of each coupon code (6-50 chars)       | 12             |

## Usage Scenarios

### Scenario 1: E-commerce Marketing Campaign
- **Use Case**: Black Friday sale with 1000 unique discount codes
- **Process**: Configure promotion in BigCommerce → Set environment variables → Run `npm run start`
- **Result**: 1000 unique coupon codes created in BigCommerce + CSV backup

### Scenario 2: Code Generation for External Use
- **Use Case**: Generate codes for print marketing materials
- **Process**: Set QUANTITY and CODE_LENGTH → Run `npm run csv`
- **Result**: CSV file with codes (no BigCommerce interaction)

### Scenario 3: Testing and Development
- **Use Case**: Test coupon functionality before production
- **Process**: Use small QUANTITY for testing → Validate codes work → Scale up
- **Result**: Verified coupon system with manageable test data

## Technical Features

### Error Handling
- **API Validation**: Checks if BigCommerce API is accessible
- **Promotion Validation**: Ensures promotion exists and is coupon type
- **Environment Validation**: Validates all required environment variables
- **Code Creation Validation**: Checks each API response for successful coupon creation

### Performance Considerations
- **Sequential Processing**: Creates coupons one at a time to avoid API rate limits
- **Progress Feedback**: Real-time terminal updates during bulk operations
- **Incremental Saving**: CSV file updated after each successful coupon creation
- **Failure Recovery**: Partial results saved if process is interrupted

### Data Security
- **Environment Variables**: Sensitive API credentials stored in .env file
- **Git Ignore**: .env file excluded from version control
- **API Scope**: Requires only Marketing: modify scope (minimal permissions)

## Output Files

### CSV File Structure
```csv
code
ABC123DEF456
XYZ789GHI012
...
```

### File Naming Convention
- **Coupons**: `coupons-{timestamp}.csv` (created in BigCommerce)
- **Codes Only**: `codes-{timestamp}.csv` (CSV generation only)
- **Location**: `./results/` directory

## Dependencies

- **dotenv**: Environment variable management
- **node-bigcommerce**: Official BigCommerce API client
- **Node.js Built-ins**: fs, path, readline for file operations and user interface

This application is ideal for e-commerce businesses using BigCommerce who need to create large quantities of unique coupon codes efficiently and reliably.