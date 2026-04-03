Lawn Service QR Contact System — README
Summary
This is a single-file, zero-dependency HTML application that generates a branded QR code.
When scanned by any smartphone camera, the QR code opens the device’s native SMS
application with a destination phone number and pre-written message already filled in. The
customer simply taps Send.
No servers, databases, logins, app installs, or third-party services are required for the
customer-facing flow. The only external resource is the QRCode.js library, loaded from a
public CDN.
Files
File Purpose
lawn-contact-
qr.html
The complete application — QR generator, live preview, and
download tool
README.md This document
How It Works — Step by Step
1. Page Load → QR Code Generation
When lawn-contact-qr.html is opened in any browser, JavaScript runs immediately:
It constructs an SMS URI using RFC 5724 syntax:
sms:+15739877141?body=I%20am%20interested%20in%20your%20service.
It passes this URI to QRCode.js, which renders a <canvas> element containing the
encoded QR code.
The QR code is displayed in the left-hand card with your brand’s deep green colour
scheme.
2. Customer Scans the QR Code
Any smartphone camera app (iOS 11+, Android 8+) recognises QR codes natively — no
scanner app needed. Scanning reads the embedded SMS URI.
3. Browser Opens → SMS Triggered
The sms: URI scheme is handled natively by mobile operating systems:
Platform Behaviour
iOS (Safari, Chrome, Edge) Opens the native Messages app, number and
body pre-filled
Android (Chrome, Samsung Internet,
Firefox)
Opens the default SMS app with number and
body pre-filled
The customer sees the pre-written message “I am interested in your service.” already in
the compose field. They tap Send. The message is delivered to +1 (573) 987-7141.
4. Optional: The Branded Button
The right-hand card shows a live preview of what customers see when the QR is configured
to point to a hosted URL instead of directly to the SMS URI. The gold “Contact Lawn
Service” button also triggers the same sms: link, giving a branded experience for any
customer who lands on the page via a link or direct URL share rather than QR scan.
5. Download the QR Code as PNG
The Download PNG button converts the rendered <canvas> to a data URL and triggers a
browser download. The PNG can be dropped into flyers, yard signs, invoices, or social
media posts.
Key Technical Elements
QRCode.js
Library: qrcodejs v1.0.0 via cdnjs
Renders to a <canvas> element in the browser — no server-side generation
Error correction level H (30% data restoration tolerance) — makes the code resilient to
minor print damage or dirt
Custom colours: dark #1a4a2e (brand green) on white
SMS URI Scheme (RFC 5724)
The sms: URI is the universally supported method for opening a native SMS compose
window on mobile devices. The ?body= parameter passes URL-encoded text as the pre-
filled message body. This is distinct from a tel: link (which dials a call) or a mailto: link
(email).
sms:<phone>?body=<url-encoded-message>
This works on every iOS version that has a camera QR reader (iOS 11+) and every Android
version with Chrome 40+ or Samsung Internet 4+, covering effectively all phones in active
use as of 2025.
Canvas-to-PNG Download
After QRCode.js finishes rendering (polled with a setTimeout of 400ms to ensure paint
completion), the <canvas> element is converted to a PNG data URL via
canvas.toDataURL('image/png') . This data URL is bound to the download anchor’s href
attribute, enabling a one-click local PNG save with no server round-trip.
CSS Architecture
CSS custom properties (variables) drive all colours and radii for easy re-theming
@import (body)
loads two Google Fonts: Playfair Display (display/heading) and DM Sans
Fully responsive — collapses to a single column below 640px viewport width
All animations are CSS @keyframes (no JavaScript animation library)
Configuration — Changing Phone Number or Message
Open lawn-contact-qr.html bottom of the file inside the <script> tag:
in any text editor. Find the Configuration block near the
// ─── Configuration ────────────────────────────────────────────────────────
const PHONE_NUMBER = '+15739877141';
const SMS_MESSAGE = 'I am interested in your service.';
Change PHONE_NUMBER to any E.164 format number (e.g. +14155551234 )
Change SMS_MESSAGE to any pre-filled text
Save the file and reload — the QR code regenerates automatically
Deployment Options
For yard signs, flyers, or business cards the Direct SMS option is recommended — it is the
fastest path from scan to message sent.
Browser & OS Compatibility
Option Description Hosting
Required
Direct SMS
QR (current)
QR encodes the sms: URI directly. Phone opens SMS
immediately on scan.
None
Hosted
Landing
Page
Host this HTML file at a short URL. QR encodes that
URL. Customer sees branded button first.
Any web
server or CDN
Environment QR Generator Page SMS Button
Desktop Chrome / Firefox
/ Safari
QR renders, PNG
download works
SMS link may not work on
desktop
iOS Safari Opens Messages
iOS Chrome / Edge Opens Messages
Android Chrome Opens default SMS app
Android Samsung
Internet Opens Messages
Android Firefox Opens default SMS app
The QR generator page is best used on a desktop to download the PNG. The SMS button
is designed for mobile use only (desktop operating systems do not have a native SMS app).
Limitations
The sms: compose window only.
URI scheme does not guarantee delivery confirmation — it opens the
Some corporate MDM-managed devices may restrict the sms: scheme.
If the customer’s default SMS app is changed or restricted, behaviour may vary.
QRCode.js requires the browser to support the HTML5 Canvas API (all modern browsers
do).
Generated by imagiSOFT Ltd — QR + SM
