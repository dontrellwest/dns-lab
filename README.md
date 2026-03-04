# DNS: Records, Cache, and Resolution

I used the Active Directory environment I already had running to explore how DNS works in practice: creating records, watching how clients cache them, and seeing what happens when records change.

---

## Environment

- Microsoft Azure (DC-1 and Client-1 from the Active Directory lab)
- Windows Server 2022 (DC-1, domain controller and DNS server)
- Windows 10 (Client-1)
- PowerShell and Command Prompt

---

## What I Did

### A-Record

From Client-1, tried to ping "mainframe" and run nslookup on it. Both failed because no DNS record existed. Went to DC-1 and created an A-record pointing "mainframe" to the domain controller's private IP. Went back to Client-1and the ping resolved immediately.

### DNS Caching

Changed the "mainframe" A-record on DC-1 to point to 8.8.8.8 instead. Went back to Client-1 and pinged "mainframe" again. It still returned the old IP because the client had cached the previous result.

Ran `ipconfig /displaydns` to see the cached record, then `ipconfig /flushdns` to clear it. After flushing, the ping returned the updated address.

### CNAME Record

Created a CNAME record on DC-1 pointing "search" to www.google.com. From Client-1, pinged "search" and ran nslookup on it. Both showed the CNAME resolution working correctly.

---

## Key Takeaways

The caching exercise is the most useful part of this lab for real help desk work. When a DNS record changes and a user says something "stopped working," the fix is often just flushing the local cache. Seeing that play out step by step makes it easy to remember.
