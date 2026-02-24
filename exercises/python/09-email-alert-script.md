---
Title: Exercise 09: Email Alert Script with Python
Objective: Learn to send basic email alerts using Python's built-in `smtplib` module. This is a vital skill for automated monitoring systems, allowing scripts to notify administrators of security events, system failures, or other critical incidents.
Estimated Time: 30-60 minutes
Tools Needed:
*   Python 3
*   A text editor
*   An email account with SMTP access (e.g., Gmail, Outlook, or a corporate SMTP server).
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  **Gmail/Outlook Specific:** If using Gmail, you'll need to enable "Less secure app access" or, more securely, generate an "App Password" (recommended) if you have 2-Factor Authentication enabled. For Outlook/Office365, you might need to enable SMTP AUTH and ensure your account can send via SMTP.
3.  Have the SMTP server address, port, your email address, and your email password (or app password) ready.
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `send_alert_email.py`.

2.  **Import Modules**
    *   Add `import smtplib`, `import ssl`, and `from email.mime.text import MIMEText` at the beginning of your script.
        *   `smtplib`: Handles sending email.
        *   `ssl`: For secure connections (TLS/SSL).
        *   `MIMEText`: To create well-formatted email messages.

3.  **Configure Email Settings**
    *   Define variables for `smtp_server`, `port`, `sender_email`, `receiver_email`, and `password`.
    *   **WARNING:** Hardcoding passwords directly in scripts is insecure. For a real-world scenario, use environment variables or a secure configuration management system. For this exercise, it's acceptable for learning purposes in a controlled environment.

4.  **Create the Email Message**
    *   Use `MIMEText()` to create your email subject and body.

5.  **Send the Email**
    *   Create a secure SSL context: `context = ssl.create_default_context()`.
    *   Connect to the SMTP server and start TLS: `server = smtplib.SMTP_SSL(smtp_server, port, context=context)` or `smtplib.SMTP(smtp_server, port)` then `server.starttls(context=context)`.
    *   Log in: `server.login(sender_email, password)`.
    *   Send the email: `server.sendmail(sender_email, receiver_email, msg.as_string())`.
    *   Quit the server: `server.quit()`.

## Example `send_alert_email.py`

```python
import smtplib, ssl
from email.mime.text import MIMEText
import sys
import getpass # For securely asking for password in console

def send_email_alert(subject, body, sender_email, receiver_email, smtp_server, port, password):
    """Sends an email alert using the provided credentials."""

    msg = MIMEText(body)
    msg['Subject'] = subject
    msg['From'] = sender_email
    msg['To'] = receiver_email

    # Create a secure SSL context
    context = ssl.create_default_context()

    try:
        # Connect to the SMTP server (using SMTPS_SSL for port 465, or SMTP + starttls for port 587)
        # For Gmail, use smtp.gmail.com, port 465 (SSL) or 587 (TLS)
        # For Outlook, use smtp.office365.com, port 587 (TLS)
        
        # Using SMTP_SSL for port 465 (typically implicit SSL)
        with smtplib.SMTP_SSL(smtp_server, port, context=context) as server:
            server.login(sender_email, password)
            server.sendmail(sender_email, receiver_email, msg.as_string())
        print("Email sent successfully!")

    except smtplib.SMTPAuthenticationError:
        print("Error: SMTP Authentication failed. Check your username and password, or app password/less secure app access settings.")
    except smtplib.SMTPServerDisconnected:
        print("Error: SMTP server disconnected unexpectedly. Check server address and port.")
    except smtplib.SMTPConnectError:
        print("Error: Could not connect to the SMTP server. Check server address, port, and network connectivity.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        sys.exit(1)

if __name__ == "__main__":
    # --- Configuration ---
    # Replace with your actual email details
    # For security, consider using environment variables for sensitive info in production
    sender_email = input("Enter sender email address: ")
    receiver_email = input("Enter receiver email address: ")
    smtp_server = input("Enter SMTP server (e.g., smtp.gmail.com): ")
    port = int(input("Enter SMTP port (e.g., 465 for SSL, 587 for TLS): "))
    password = getpass.getpass("Enter your email password or app password: ") # getpass hides input

    subject = "Security Alert: Unusual Activity Detected!"
    body = """
    Dear Administrator,

    This is an automated alert from your monitoring system.
    Unusual activity has been detected on the server at 2026-02-24 17:00:00 UTC.
    Please investigate immediately.

    Details:
    - Event: Unauthorized login attempt
    - Source IP: 192.168.1.50
    - User Account: root

    Best regards,
    Monitoring System
    """
    
    send_email_alert(subject, body, sender_email, receiver_email, smtp_server, port, password)
```

## Expected Output

```
Enter sender email address: your_email@example.com
Enter receiver email address: admin_email@example.com
Enter SMTP server (e.g., smtp.gmail.com): smtp.gmail.com
Enter SMTP port (e.g., 465 for SSL, 587 for TLS): 465
Enter your email password or app password: 
Email sent successfully!
```
*(Check the receiver's inbox for the email.)*
![Expected output](images/exercise-09-output.png)

## Reflection Questions

1.  What are the security implications of storing email credentials directly in a script, and what are safer alternatives?
2.  How would you modify this script to send an email with an attachment (e.g., a log file)?
3.  Beyond security alerts, what are other practical applications for automated email notifications in IT operations?

## Next Steps

*   [Exercise 10: Read and Write CSV Files with Python](../python/10-read-write-csv.md)
*   Further reading: [Python `smtplib` documentation](https://docs.python.org/3/library/smtplib.html) and [Python `email` package documentation](https://docs.python.org/3/library/email.html)

## Hints / Troubleshooting

*   **App Passwords:** For Gmail with 2FA, create an "App Password" (Google Account -> Security -> App passwords) and use that instead of your main password.
*   **Less Secure Apps (Gmail):** If you don't use 2FA, you might need to enable "Less secure app access" in your Google Account security settings. This is generally not recommended.
*   **Firewall:** Ensure your network firewall allows outbound connections on the specified SMTP port (e.g., 465 or 587).
*   **`starttls` vs. `SMTP_SSL`:** If your server uses port 587 for TLS, you would typically use `smtplib.SMTP(smtp_server, port)` and then `server.starttls(context=context)`. For port 465, `smtplib.SMTP_SSL` is standard. The example uses `SMTP_SSL` which works for 465.
*   **Error Messages:** Pay close attention to error messages (e.g., `SMTPAuthenticationError`, `SMTPConnectError`) as they often indicate the problem.
