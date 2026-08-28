# Wireshark Analysis: Extracting Credentials from HTTP Traffic

## Objective
Analyze a provided packet capture (.pcap) file to identify HTTP traffic and demonstrate how credentials transmitted over unencrypted HTTP can be intercepted.

## Tools Used
- Wireshark (network protocol analyzer)

## Methodology
1. Imported the provided .pcap file into Wireshark.
2. Applied the display filter `http` to isolate HTTP traffic from the full capture.
3. Identified a POST request to `/doLogin` on the target site.
4. Expanded the "HTML Form URL Encoded" section under Hypertext Transfer Protocol to view submitted form data.
5. Located the submitted form fields and their values.

## Target Site
http://demo.testfire.net/login.jsp
(A publicly available, intentionally vulnerable demo banking application used for security testing practice — not a real production system.)

## Findings
The login form was submitted over **unencrypted HTTP**, meaning the form data was visible in plaintext within the packet capture.

Extracted form fields:
- **uid (username):** Cybersecurity
- **passw (password):** This is my password

## Why This Matters
Because the site used HTTP instead of HTTPS, the login form data was transmitted without encryption. Anyone capturing traffic on the same network path (e.g., via a man-in-the-middle position) could read submitted credentials directly, as demonstrated here. This is why HTTPS/TLS is essential for any page handling authentication or sensitive data.

## Evidence
See screenshots in this folder:
- HTTP filter applied showing the GET/POST request sequence.
- Expanded POST packet showing the extracted form field values.

## Skills Demonstrated
- Wireshark display filtering (`http`)
- Packet capture (.pcap) analysis
- HTTP protocol inspection
- Recognizing cleartext credential exposure
