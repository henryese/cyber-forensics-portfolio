# Cyber Forensics; Practical Investigations

A collection of three digital forensics projects I worked on. Each one is a different kind of case, one started with a hard drive, one with captured network traffic, and one with a snapshot of a computer's memory. I've put my report and the screenshots I took along the way in here as a portfolio of the work.

Just so it's clear, the actual evidence isn't in this repo no disk image, pcap, memory dump or the malware itself. Some of it is live malware, all of it is big, and none of it is really mine to share. What you'll find here is my own analysis and write-up of what I found.

## What's in here
- `report/` — my full report (PDF)
- `screenshots/` — the screenshots from each case, numbered to match the report
- `data/` — a few CSV exports I pulled out of Autopsy while working the hard-drive case

## The three cases

**Case 1. the hard drive.** This one started with a Windows 10 disk image, and the job was basically to work out who'd been using the machine and what they'd been up to. The first thing I did was check the image hash against the acquisition log, and straight away it didn't match. Turned out one of the image segments had been corrupted when it was extracted, so I had to track down exactly which one, replace it from the original download, and verify everything again before I could trust the evidence. Once that was sorted I went through the browser and registry artefacts  email accounts, browsing history, downloads, saved Wi-Fi, even some geotagged photos and pulled the SAM and SYSTEM registry hives to crack the Windows login password.

**Case 2. the network capture.** Here I had a packet capture that an IDS had flagged. I opened it up in Wireshark (inside Kali, since there was malware in the traffic) and pieced together what happened. It turned out to be a TrickBot infection. I identified the infected machine and the user account, found a DLL that had been served up disguised as a .png, hashed it, and mapped out the command-and-control servers it was talking to and the data it was quietly stealing.

**Case 3. the "bad PDF".** The last one was a memory dump from a Windows XP machine where someone had opened a dodgy PDF. Using Volatility I followed the trail: the PDF exploited an old version of Adobe Reader, which then injected code into a legitimate Windows process to stay hidden. Digging through the process memory I found the C2 URLs and a set of online-banking targets it was Zeus-style banking malware and recovered the administrator password hash.

## Tools I used
Autopsy, FTK Imager, Wireshark, Volatility, John the Ripper and impacket, plus a fair amount of PowerShell and the Linux command line.

## How I approached it
I stuck to the standard forensic process throughout (NIST SP 800-86, the ACPO guidelines and ISO 27037) working only on copies, verifying hashes before and after, and keeping enough notes that someone else could follow the same steps and get the same results.
