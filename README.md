# Automating Email Generation with Ansible

## Overview

This repository contains an Ansible playbook designed to automate the process of generating and sending emails using Ansible's `mail` module. This can be useful for automating notifications, reports, or any other kind of email communication in your infrastructure.

## Prerequisites

Before running the playbook, ensure you have the following prerequisites in place:

1. **Ansible Installed**: Ensure that Ansible is installed on the control node (the machine where you will run the playbook).
   - You can install Ansible using pip: 
     ```bash
     pip install ansible
     ```

2. **SMTP Server Access**: The playbook requires an SMTP server to send emails. You should have access to an SMTP server and know the server address, port, and authentication credentials if required.

## Playbook Configuration

The provided playbook, `sendemail.yaml`, is pre-configured with placeholder values. Before running the playbook, you need to customize these placeholders with your specific details.

### Variables to Customize:

- `host`: Replace with your SMTP server address.
- `port`: Set the appropriate SMTP port (common ports are 25, 465, or 587).
- `username`: Your SMTP username (often the sender's email address).
- `password`: Your SMTP password.
- `to`: The recipient's email address.
- `from`: The sender's email address.
- `subject`: The subject line for the email.
- `body`: The content of the email.

Example snippet from the playbook:

```yaml
mail:
  host: smtp.example.com           # Replace with your SMTP server
  port: 587                        # Replace with your SMTP port
  username: your_email@example.com # Replace with your SMTP username
  password: your_password          # Replace with your SMTP password
  to: recipient@example.com        # Replace with recipient's email
  from: your_email@example.com     # Replace with sender's email
  subject: "Automated Email from Ansible"
  body: |
    Hello,

    This is an automated email sent by an Ansible playbook.

    Regards,
    Ansible Automation
  secure: starttls                 # Use 'starttls' for secure connection or 'ssl' for SSL/TLS
  charset: utf-8                   # Ensure proper encoding
```

## Running the Playbook
Once you've customized the playbook with your details, you can run it to send the email. Follow these steps:

1. Save the playbook as sendemail.yaml in your working directory.

2. Run the playbook using the following command:
```bash
ansible-playbook sendemail.yaml
```

## Updated Playbook Overview (sendemailv2.yaml)
This updated Ansible playbook will:

1. Read an Excel Spreadsheet: Using the community.general.read_excel Ansible module (which requires the community.general collection), the playbook will read data from an Excel spreadsheet.
2. Identify Non-Consistencies: Compare values across certain columns to identify non-consistencies.
3. Generate a List of Emails: Extract a list of email addresses from the spreadsheet associated with the identified non-consistencies.
4. Send Emails: Use the mail module to send a notification email to the extracted list of email addresses.

### Prerequisites for updated playbook (sendemailv2.yaml)
1. Ansible Collection: The community.general collection must be installed to use the read_excel module.
install it with:
```bash
ansible-galaxy collection install community.general
```
3. Python Dependencies: The control node will require Python libraries like openpyxl to handle Excel files.
install them with:
```bash
pip install openpyxl
```
### Explanation of the Updates
1. Reading the Excel File:
The community.general.read_excel module reads the specified Excel file and registers the data into excel_data.
The sheet_name variable specifies which sheet to read from.

2. Identifying Non-Consistencies:
We use a set_fact task to iterate through the rows of the Excel sheet. For each row, the playbook checks if the values in check_column_1 and check_column_2 are different.
If they differ, the email address from email_column is added to a list.

3. Ensuring Unique Emails:
The playbook uses a filter to remove duplicate email addresses, ensuring each recipient only receives one email.

4. Sending Emails:
The mail module is configured to send emails to the list of unique email addresses extracted from the Excel file. The to field joins the list of emails into a single string, separating each email with a comma.

### Customization
1. Columns to Check: Adjust the check_column_1 and check_column_2 variables to specify which columns you want to compare for inconsistencies.
2. Email Content: Customize the subject and body of the email to fit your needs.
3. SMTP Configuration: Ensure your SMTP server settings are correctly configured.
