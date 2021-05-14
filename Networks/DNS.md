# DNS (Domain Name System)

Translates IP addresses to domains names.

> The "root" domain is '`.`'

## Hierarchy

1. **TLD** (Top-Level Domain): ([Full List](https://data.iana.org/TLD/tlds-alpha-by-domain.txt))
    - **gTLD** (Generic Top Level) &rrar; informs the user the domain name's purpose.
        + `.com` (commercial)
        + `.gov` (government)
        + `.org` (organization)
        + `.edu` (education)
        + ...
    - **ccTLD** (Country Code Top Level Domain) &rrar; geographical
        + `.fr` (France)
        + `.de` (Deutschland)
        + ...
2. **Second-Level Domain**
    - Maximum length: 63 characters + TLD.
    - Can only use a-z 0-9 and hyphens. They cannot start or end with hyphens or have consecutive hyphens)
3. **Subdomain**
    - Maximum length: 63 characters.
    - Can only use a-z 0-9 and hyphens. They cannot start or end with hyphens or have consecutive hyphens)

The maximum length of a domain name is 253 characters.

## Records


Record      | Description 
------------|--------------
**A**       | resolve to IPv4 addresses
**AAAA**    | resolve to IPv4 addresses
**CNAME**   | resolve to another domain name
**MX**      | resolve to the address of the servers that handle the email for the domain queried
**TXT**     | free text fields where any text-based data can be stored (can be used for multiple uses such as verifying ownership of the domain name when signing up for third party services, or listing servers that have the authority to send an email on behalf of the domain)

## Requests


1. Looking at **local cache** if the address was previously looked up
2. Asking **Recursive DNS Server**'s local cache (usually provided by ISP) for popular domains (`google.com` for example)
3. **Root DNS Servers** redirects you to the **Top Level Domain Server** that handles the **TLD** you asked for (`.com` for examples) and redirects you the right **Authoritative DNS**. 
4. **Authoritative DNS** (sent back the record to the Recursive DNS Server, where a local copy will be cached for future requests and then relayed back to the original client)

## Tools

- `nslookup` - query Internet name servers interactively
- `dig` - DNS lookup utility

## Useful links

- <https://howdns.works/>