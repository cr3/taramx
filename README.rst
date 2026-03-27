TaraMX
======

.. image:: https://github.com/cr3/taramx/workflows/test/badge.svg
       :target: https://github.com/cr3/taramx/actions

Public SMTP gateway.

Overview
--------

TaraMX is a lightweight public SMTP relay gateway designed to sit on a
VPS with a static IPv4 and IPv6 address. It handles:

- **Inbound SMTP**: Receives mail from the internet on port 25 and
  relays it to a downstream mail server.
- **Outbound SMTP**: Accepts authenticated relay on port 587 and
  delivers to the internet.
- **IP Reputation**: Owns the PTR records and stable IP addresses,
  ensuring deliverability is not affected by the private server's
  dynamic IP.
- **TLS Termination**: Manages Let's Encrypt certificates for
  STARTTLS on port 25 and port 587.

Services
--------

- **postfix** — Relay MTA (no virtual domains, no content filtering)
- **certbot** — Let's Encrypt TLS certificate management
- **unbound** — Local DNSSEC-validating DNS resolver

Setup
-----

1. Configure environment variables:

   .. code-block:: text

       > cp .env.example .env
       > vi .env

2. Obtain initial TLS certificate:

   .. code-block:: text

       > docker compose run -it --rm -p 80 certbot certbot certonly -v --standalone --non-interactive --agree-tos --deploy-hook /deploy-hook.sh -d mx.taram.ca

3. Deploy services:

   .. code-block:: text

       > make deploy

Connectivity
------------

TaraMX communicates with the downstream mail server over the
internet using SMTP with TLS encryption:

- **Inbound**: TaraMX relays incoming mail to the downstream server
  via SMTP with STARTTLS. Set ``RELAY_HOST`` to its hostname or IP.
- **Outbound**: The downstream server authenticates to TaraMX on
  port 587 using SASL credentials (``SUBMISSION_USER`` /
  ``SUBMISSION_PASS``) over TLS.

DNS Configuration
-----------------

Ensure the following DNS records are configured:

- **MX record**: ``taram.ca MX 10 mx.taram.ca``
- **A record**: ``mx.taram.ca A <IPv4>``
- **AAAA record**: ``mx.taram.ca AAAA <IPv6>``
- **PTR records**: Reverse DNS for both IPv4 and IPv6 addresses
- **SPF record**: ``v=spf1 ip4:<IPv4> ip6:<IPv6> -all``
