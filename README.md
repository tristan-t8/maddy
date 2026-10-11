# Maddy Mail Server

## Overview

Maddy is a composable all-in-one email server written in Go. It provides the core functionality needed to send, receive, and store emails on a self-hosted mail server.

This project documents a self-hosted email setup using Maddy for mail services and compatible email clients for accessing mailboxes.

## Features

- **SMTP** — Sends outgoing emails and receives incoming emails from other mail servers.
- **IMAP** — Allows email clients to access and synchronize stored emails.
- **TLS Encryption** — Protects connections between clients and the server when properly configured.
- **Self-Hosted Email** — Provides control over email infrastructure and server configuration.
- **Email Client Compatibility** — Supports clients that implement the appropriate mail protocols.

## Protocols Explained

### SMTP (Simple Mail Transfer Protocol)

SMTP is used to send email messages and transfer incoming messages between mail servers.

Common ports:
- `25` — Server-to-server email delivery.
- `587` — Message submission, typically with STARTTLS.
- `465` — Message submission over implicit TLS.

### IMAP (Internet Message Access Protocol)

IMAP allows email applications to access and manage messages stored on a mail server without downloading and maintaining a separate, independent mailbox on every device.

For example, when an email is read on a computer, an IMAP-compatible phone app can synchronize that message's read status.

Common ports:
- `143` — IMAP, commonly upgraded to TLS using STARTTLS.
- `993` — IMAP over implicit TLS, commonly called IMAPS.

**IMAP provides mailbox access and synchronization; it does not send outgoing emails.** Sending messages is generally handled through SMTP submission.

## Basic Architecture

```text
             Incoming Email
                    |
                    v
             Other Mail Servers
                    |
                    v
              Maddy Server
                    |
             +------+------+
             |             |
             v             v
         Mail Storage   SMTP Service
             |
             v
        IMAP Service
             |
       +-----+------+
       |            |
       v            v
   Computer       Mobile
   Email App      Email App
```

*Note: The exact services, ports, and storage configuration depend on the deployment.*

## Email Client Configuration

Use the actual hostname and account credentials configured on your server.

| Setting | Protocol | Typical Port | Security |
|---|---|---:|---|
| Incoming mail | IMAP | 993 | Implicit TLS |
| Outgoing mail | SMTP submission | 587 | STARTTLS |
| Outgoing mail | SMTP submission | 465 | Implicit TLS |

- **Username:** The full email address, if required by the server configuration.
- **Password:** The mailbox password configured on the server.
- **Server hostname:** Your configured mail domain, such as `mail.example.com`.
- **Certificate:** The server should present a valid TLS certificate matching the hostname.

These are typical settings, not confirmation that every port is enabled on your server.

## Security Considerations

- Use valid TLS certificates for mail services.
- Use strong mailbox passwords.
- Avoid exposing unnecessary ports to the public internet.
- Configure SPF, DKIM, and DMARC DNS records to help authenticate outgoing mail.
- Keep the server software and operating system updated.
- Back up mail storage and configuration securely.
- Do not expose administrative interfaces or credentials publicly.

## Troubleshooting

If email clients cannot connect, check:

1. DNS records point to the correct server.
2. The required ports are listening and allowed through the firewall.
3. TLS certificates are valid and match the server hostname.
4. The mailbox exists and the login credentials are correct.
5. Maddy logs show no authentication, storage, or connection errors.
6. IMAP and SMTP services are enabled and configured correctly.
