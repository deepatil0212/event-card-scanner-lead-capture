# Event Card Scanner & WhatsApp Lead Capture

A lead-capture web app built for the ELASIA 2026 trade event. Staff photograph a visitor's business card, the app reads the contact details from the photo, a person checks them, the lead is saved, and the visitor receives a WhatsApp follow-up with a product-range PDF. Built at Eleczo India Pvt. Ltd.

> **Showcase repository.** This repo contains documentation only. The source code is private because it belongs to my employer. Screenshots use sample data.

## Screenshots

<p>
  <img src="screen-1-scan.png" alt="Scan screen" width="24%">
  <img src="screen-2-verify.png" alt="Verify details screen" width="24%">
  <img src="screen-3-saved.png" alt="Saved confirmation" width="24%">
</p>

*Scan a card, review and correct the extracted details, then save and send the WhatsApp invitation.*

![Admin dashboard](screen-4-admin.png)

*Admin dashboard: totals, WhatsApp sent and failed counts, the card photo, contact details, and delivery status for every scan, with CSV export.*

## At a glance

| | |
|---|---|
| **Type** | Internal web app for event lead capture |
| **Backend** | Python, Flask |
| **OCR** | Google Cloud Vision (text detection) |
| **Database** | MySQL (`registrations` table) |
| **Messaging** | Interakt WhatsApp Business API (template message with product-range PDF) |
| **Frontend** | Server-rendered HTML templates and plain CSS, mobile first |
| **Access** | QR code for quick phone access; Cloudflare Tunnel for public access |
| **Deployment** | Windows server, port 5000 |

## The problem

At an event, collecting visitor details by hand is slow and full of typing mistakes, and follow-ups go out late or not at all. The goal was to turn "take a photo of the card" into a saved lead and a sent WhatsApp message in under a minute, on any phone, without installing an app.

## What I built

- **Scan screen.** On a phone the Scan Card button opens the back camera; on a laptop it opens the file picker. There is also a manual entry option, and an optional field to record who scanned. A QR code gives quick access on phones.
- **OCR and field extraction.** The photo goes to Google Cloud Vision. A rule-based parser then picks out name, phone, email, company and designation from the raw text.
- **Verify step.** The extracted details appear in an editable form next to the card photo, with a Retake option and the raw OCR text available, so a person corrects mistakes before anything is saved.
- **Save and send.** One button stores the lead in MySQL and sends the WhatsApp message through Interakt, with the product-range PDF. Sending can be switched off in configuration for testing.
- **Admin dashboard.** Totals for all scans and today's scans, WhatsApp sent and failed counts, the card photo and details for every lead, the failure reason from WhatsApp when a send fails, and CSV export.
- **Health pages.** `/debug` checks MySQL, Google Vision and Interakt one by one; `/healthz` is a quick database check.

## Architecture

![Architecture](architecture.png)

## How it works

![Scan to invitation workflow](workflow.png)

## Technical highlights

- **Heuristic parsing of unstructured OCR text.** Emails are matched by pattern. Phone numbers are found as 10 to 14 digit sequences, with Indian mobile numbers (starting 6 to 9) preferred. Designations are matched against a keyword list. Company names come from markers such as "Pvt" and "Ltd", or fall back to the email domain.
- **Human in the loop by design.** OCR parsing is never fully right, so the flow always passes through a verify screen instead of saving blindly.
- **Self-diagnosing setup.** The `/debug` page reports each dependency separately, which made installation on a new machine much faster to troubleshoot.
- **Camera access on phones.** Browsers only allow camera capture on HTTPS or localhost, which shaped how phones reach the app.
- **WhatsApp delivery issues.** Messages reported as "sent" but not delivered were traced to WhatsApp throttling marketing templates (error 131049), not to the code. The fix is on the template category side.

## Known limitations and next steps

- Parsing is rule-based, so unusual card layouts need manual correction.
- The admin page and scan form have no login or captcha, and secrets sit in a config file. That is acceptable for a short-lived internal event tool, but it needs hardening before wider use.
- Next steps: an admin login, HTTPS behind a reverse proxy, secrets in environment variables, rate limiting, and running the app as a Windows service.

## Skills demonstrated

Python and Flask, REST API integration (Google Cloud Vision, Interakt), text parsing with regular expressions, MySQL schema design, mobile-friendly HTML and CSS, Windows server deployment, and writing setup and troubleshooting documentation.

## Contact

- Email: deepatil0212@gmail.com
- LinkedIn: [linkedin.com/in/deepa-patil-332925345](https://www.linkedin.com/in/deepa-patil-332925345)
