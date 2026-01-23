:star: If you appreciate this integration, please consider giving it a star! Your support encourages me to continue improving and expanding this project. Thank you! :star:

# Actual Budget integration for Home Assistant

This is a custom integration for Home Assistant that allows you to track your actual budget.

Note: It's a work in progress, it should work but it may have some bugs and breaking changes.

# Features

- Gets all accounts balance and set it as sensors
- Gets all budgets and set the current month as sensors (last month and total are set as extra attributes)
- Tracks uncategorised transactions with a dedicated sensor showing count and transaction details

# Installation

## HACS

1. Go to HACS page
2. Search for `Actual Budget`
3. Install it

Note: If this is not in HACS yet, you can add this repository manually.

## Add The Repository Manually

1.
<img width="933" alt="SCR-20240830-lpfj" src="https://github.com/user-attachments/assets/b8ebd9ca-ccc2-4f60-a9a9-6cf69b71cafe">

2.
<img width="492" alt="SCR-20240830-lovt" src="https://github.com/user-attachments/assets/1ed50bff-77a2-46d4-9dc1-3aff97ee585a">

3. restart home assistant, and add it in the Settings> Devices and services> Add integration> actualbudget

# Configuration

1. Go to Configuration -> Integrations
2. Click on the "+" button
3. Search for `Actual Budget`
4. Enter the needed information

| Setting       | Required | Description |
| ------------- | --------- | ----------- |
| Endpoint      | Yes       | The endpoint of the Actual Budget API |
| Password      | Yes       | The password of the Actual Budget API |
| Encrypt Password | No    | The password to decrypt the Actual Budget file (if set) |
| File          | Yes       | The file id of the Actual Budget file |
| Cert          | No        | The certificate to use for the connection, you can set it as 'SKIP' to ignore certificate validation|

Example:

```
Endpoint: https://localhost:5001
Password: password
Encrypt Password: ''
File: ab7c8d8e-048b-41b1-a9cf-13f0679edc0b
Cert: 'SKIP'
```

# Sensors

The integration creates the following sensors:

## Account Sensors
- **Entity Name**: `{prefix}_{account_name}` (e.g., `actualbudget_Checking`)
- **State**: Current account balance
- **Device Class**: Monetary
- **Update Frequency**: Every 60 minutes

## Budget Sensors
- **Entity Name**: `{prefix}_budget_{budget_name}` (e.g., `actualbudget_budget_Groceries`)
- **State**: Running total of current + past months' budget amounts plus category balance
- **Attributes**:
  - `current_month`: Current month (YYYYMM format)
  - `current_amount`: Current month amount
  - `previous_month`: Previous month (YYYYMM format)
  - `previous_amount`: Previous month amount
  - `total_amount`: Sum of all monthly amounts
- **Device Class**: Monetary
- **Update Frequency**: Every 60 minutes

## Uncategorised Transactions Sensor
- **Entity Name**: `{prefix}_uncategorised_transactions` (e.g., `actualbudget_uncategorised_transactions`)
- **State**: Count of transactions without a category assigned
- **Attributes**:
  - `transactions`: List of uncategorised transaction details
    - `id`: Transaction ID
    - `date`: Transaction date (ISO format)
    - `amount`: Transaction amount
    - `account`: Account name
    - `payee`: Payee name
    - `notes`: Transaction notes
  - `count`: Number of uncategorised transactions (same as state)
- **Update Frequency**: Every 60 minutes

**Note**: All sensors have a minimum 1-minute debounce between updates to prevent API hammering.
