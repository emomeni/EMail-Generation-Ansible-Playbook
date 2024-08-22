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
