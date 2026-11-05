# DNS Information Gathering Using the NSLookup Command

## Overview

NSLookup (Name Server Lookup) is a command-line network administration tool used to query DNS servers and retrieve information about domain names and IP addresses.

It helps security professionals investigate DNS configurations, identify associated IP addresses, retrieve mail exchange servers, discover authoritative nameservers, and examine DNS TXT records.

NSLookup is available on Windows and Linux systems.

## Lab Objective

Learn how to use the NSLookup command to gather DNS information about a target domain.

## Lab Purpose

Understand how to query DNS servers to obtain domain name and IP address mapping information.

## Lab Environment

- **Operating System:** Windows (PowerShell)
- **Alternative:** Kali Linux
- **Tool:** NSLookup
- **Target domain:** `google.com`

## Lab Walkthrough

### Task 1: Perform a Basic DNS Lookup

NSLookup is built into Windows and is also available on Linux.

On Windows, open PowerShell and execute:

```powershell
nslookup www.google.com
```

This performs a DNS lookup to retrieve the IP addresses associated with the hostname.

The response may contain:

- The DNS server used to process the query.
- The server's IP address.
- The hostname being queried.
- The returned IPv4 and/or IPv6 addresses.

NSLookup also supports an interactive mode.

Start interactive mode:

```powershell
nslookup
```

Exit interactive mode:

```text
quit
```

**Understanding the DNS resolver:**

The server displayed in the initial response is the DNS resolver that handled the query. Depending on the network configuration, this could be a router, an ISP-provided resolver, or another configured DNS server.

It acts as the system's point of contact for resolving the requested hostname.

### Task 2: Perform a Reverse DNS Lookup

A reverse DNS lookup attempts to identify a hostname associated with an IP address.

This is commonly performed using a PTR (Pointer) record.

Execute:

```powershell
nslookup 74.125.193.99
```

Unlike a standard forward lookup, which resolves a hostname to an IP address, a reverse lookup queries the DNS infrastructure for a hostname associated with the supplied IP address.

**Important distinction:**

- A records map hostnames to IPv4 addresses.
- AAAA records map hostnames to IPv6 addresses.
- PTR records map IP addresses to hostnames.

A reverse lookup may return a different hostname from the original domain, or no hostname at all. This can occur because of shared hosting, load balancing, and the way IP addresses are configured in DNS.

### Task 3: Identify Mail Exchange Servers

Mail Exchange (MX) records identify the mail servers responsible for receiving email for a domain.

Execute:

```powershell
nslookup -querytype=mx google.com
```

This queries the MX records associated with `google.com`.

The results may provide:

- Mail server hostnames.
- MX preference values.
- The domain's configured mail exchange infrastructure.

Lower MX preference values indicate higher priority.

### Task 4: Identify Authoritative Nameservers

Nameserver (NS) records identify the nameservers associated with a domain's DNS delegation.

First, enter interactive mode:

```powershell
nslookup
```

Set the query type to NS:

```text
set query=ns
```

Enter the domain name:

```text
google.com
```

NSLookup will return the nameserver records associated with the domain.

Exit interactive mode:

```text
quit
```

These records help identify the nameservers responsible for the domain's DNS information.

### Task 5: Retrieve DNS TXT Records

TXT records store text-based information associated with a domain.

They are commonly used for domain ownership verification, email authentication, and other domain configuration purposes.

Execute:

```powershell
nslookup -querytype=txt google.com
```

This retrieves the TXT records published for `google.com`, when available.

TXT records may contain information relating to:

- SPF (Sender Policy Framework).
- Domain verification.
- Email authentication.
- Other domain-specific configurations.

## Key Concepts Covered

This lab explores the following DNS concepts:

- Basic DNS lookups.
- Forward DNS resolution.
- Reverse DNS resolution.
- A, AAAA, and PTR records.
- Mail Exchange (MX) records.
- Nameserver (NS) records.
- DNS TXT records.
- Interactive and non-interactive NSLookup modes.
- DNS resolvers and their role in name resolution.

## Practical Applications in Cybersecurity

DNS enumeration is an important activity during the reconnaissance phase of a security assessment.

NSLookup can help security professionals:

- Identify IP addresses associated with domain names.
- Investigate reverse DNS records.
- Identify mail exchange servers.
- Discover nameservers associated with a domain.
- Examine publicly available TXT records.
- Understand aspects of an organisation's DNS configuration.

## Important Notes

- DNS responses may vary depending on the resolver, domain configuration, and time of the query.
- A reverse DNS lookup does not guarantee that the returned hostname will match the original domain.
- Not every domain publishes all DNS record types.
- DNS enumeration should remain within an authorized assessment scope.

## References

- [Microsoft NSLookup Documentation](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/nslookup)
- [RFC 1035: Domain Names - Implementation and Specification](https://www.rfc-editor.org/rfc/rfc1035.html)
