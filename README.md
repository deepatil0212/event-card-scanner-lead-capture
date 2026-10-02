# Event Card Scanner & WhatsApp Lead Capture

A lead-capture app built for the ELASIA 2026 trade event. It scans visitors' business cards, saves them as leads, and follows up on WhatsApp automatically.

> Showcase repository. The source code is private; this README describes the architecture and my contribution.

## Tech stack

Python · Flask · MySQL · Google Cloud Vision (OCR) · Interakt WhatsApp API · Cloudflare Tunnel

## How it works

1. A salesperson scans a visitor's business card.
2. Google Cloud Vision OCR extracts the name, phone, email, and company.
3. The lead is saved to the database.
4. A WhatsApp thank-you message with a product-range PDF is sent automatically through Interakt, tagged to the assigned salesperson.

## Features

- Sales-team selector so each lead is assigned to the right person
- Admin dashboard
- QR code access for quick use on a phone
- Public access through Cloudflare Tunnel

## Screenshots

_Add screenshots here with all personal data blurred or replaced by dummy data._
