Personalized Email Sender with Python
This project demonstrates how to automate the process of generating personalized emails/messages and sending them to a list of leads using Python. It leverages pandas for data handling and smtplib for email sending.

Table of Contents
Project Overview
Features
Prerequisites
Setup
Usage
Email Security (App Passwords)
Project Overview
This notebook provides a simple yet effective solution for reaching out to potential clients or contacts with tailored messages. It reads lead information from an Excel file, dynamically creates personalized email content, and then sends these emails via Gmail's SMTP server.

Features
Load lead data from an Excel file (.xlsx).
Generate personalized messages/emails based on lead-specific information (Name, Company, Industry, Application).
Send emails using Python's smtplib library with Gmail.
Prerequisites
Before running this notebook, ensure you have the following installed:

Python 3.x
pandas
openpyxl (for reading .xlsx files)
You can install the required Python libraries using pip:

pip install pandas openpyxl
Setup
1. Prepare your Leads Data
Create an Excel file named leads.xlsx (or leads.csv if you modify the code to use pd.read_csv) in the same directory as your notebook. This file should contain at least the following columns:

Name: The name of the lead.
Company: The company of the lead.
Email: The email address of the lead.
Industry: The industry the lead is in.
Application: The specific application or work the lead is involved with.
Example leads.xlsx structure:

Name	Company	Email	Industry	Application
A	A1 Sales	A@hotmail.com	Medical	Image Processing
B	B2 Sales	B@hotmail.com	Automotive	Autonomous Car
C	C3	C@gmail.com	Audio	Signal Processing
2. Configure Gmail Credentials
To send emails, you'll need to provide your Gmail address and an App Password (not your regular Gmail login password). Replace the placeholders in the email sending cell:

EMAIL_ADDRESS = 'XXXXX@gmail.com' # Your Gmail address
EMAIL_PASSWORD = 'XXXXX' # Your App Password
Important: For security reasons, never hardcode your main Gmail password directly into your scripts. Use Gmail App Passwords for external applications.

Usage
Load Leads Data: The first code cell loads your leads.xlsx file into a pandas DataFrame.

import pandas as pd
leads = pd.read_excel('leads.xlsx')
print(leads.head(5))
Review Personalized Messages: The second code cell demonstrates how personalized messages are generated. It iterates through a sample of your leads and prints the generated message to the console. You can adjust the leads.head(2) to leads.head() to see all messages, or leads.head(n) to see n messages.

for index, row in leads.head(2).iterrows():
    message = f"""Hi {row['Name']},
    I noticed your work of doing {row['Application']} in the {row['Industry']} Industry is really impressive.
    I'd would love to connect with you to discuss how we can bring more values to your work.
    Is this Thursday a good time for coffee chat? :)

    Best regards,
    [Name]
    [Contact]
    """
    print(message)
    print("-"*40)
Send Emails: The final code cell handles sending the personalized emails. Make sure your EMAIL_ADDRESS and EMAIL_PASSWORD are correctly configured before running this cell. By default, it sends to the first two leads as a test. Remove .head(2) to send to all leads.

import smtplib
from email.message import EmailMessage

EMAIL_ADDRESS = 'XXXXX@gmail.com'
EMAIL_PASSWORD = 'XXXXX'

for index, row in leads.head(2).iterrows():
    msg = EmailMessage()
    msg['Subject'] = f'Collaboration opportunity for {row["Company"]}'
    msg['From'] = EMAIL_ADDRESS
    msg['To'] = row['Email']
    msg.set_content(f"""
Hi {row['Name']},

 I noticed your work of doing {row['Application']} in the {row['Industry']} Industry is really impressive.
 I'd would love to connect with you to discuss how we can bring more values to your work.
 Is this Thursday a good time for coffee chat? :)

 Best regards,
 [Name]
 [Contact]
  """)
    with smtplib.SMTP_SSL('smtp.gmail.com', 465) as smtp:
        smtp.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
        smtp.send_message(msg)

    print(f"Email sent to {row['Name']} at {row['Company']}")
Email Security (App Passwords)
If you have 2-Factor Authentication (2FA) enabled on your Gmail account (which is highly recommended), you cannot use your regular password directly. Instead, you need to generate an App Password.

To generate an App Password:

Go to your Google Account.
Navigate to Security.
Under "How you sign in to Google," select App passwords.
You may need to sign in.
Select the app and device you want the App password for (e.g., "Mail" and "Other (Custom name)" - you can name it 'Python Email Sender').
Click Generate.
A 16-character code will be generated. This is your App Password. Copy it and use it as EMAIL_PASSWORD in your script. Keep this password secure, as it grants access to your Gmail account for sending emails.
