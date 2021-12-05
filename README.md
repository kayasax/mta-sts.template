# 📩 A Template to host an MTA-STS Policy file on GitHub

This repository provides a template to host your _MTA Strict Transport Security (MTA-STS)_ [[RFC 8461](https://datatracker.ietf.org/doc/html/rfc8461)] policy file on GitHub Pages.

MTA-STS is a security standard to secure e-mail delivery. E-mail servers that send inbound e-mail to your domain will be able to detect that your e-mail server supports SMTP-over-TLS via `STARTTLS` (also known as [Opportunistic TLS](https://en.wikipedia.org/wiki/Opportunistic_TLS)) before opening the actual connection.

In case the sending e-mail server is not able to initiate a secure connection, it will end the connection to enforce transport layer encryption. This mitigates [Man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) DNS and SMTP [downgrade attacks](https://en.wikipedia.org/wiki/Downgrade_attack) that would allow an attacker to read or manipulate e-mail in transit.

## Usage

1. Click on [**Use this template**](https://github.com/jpawlowski/mta-sts.template/generate) to create a copy to your own GitHub profile.
   You may name your repository whatever you like. For simplicity, you can name it `mta-sts.<your_domain.tld>`.

2. Change the file `.well-known/mta-sts.txt` according to your needs.

3. Create a `CNAME` record for `mta-sts.<your_domain.tld>` in your domain's DNS that points to `<user>.github.io` or `<organization>.github.io`.
   See [GitHub Docs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages) to learn more about custom domains on GitHub Pages.

4. Open a browser to `https://mta-sts.<your_domain.tld>` and make sure it does not show any certificate warnings.

5. Create a `TXT` record for `_mta-sts.<your_domain.tld>` in your domain's DNS to enable the MTA-STS policy for your domain.
   You may copy & paste this to your DNS provider:

   ```dns
   #HOST       #TTL    #TYPE    #VALUE
   _mta-sts    3600    TXT      "v=STSv1; id=1634120459"
   ```

   Note that you will need to change the `id=` here whenever you make changes to your `mta-sts.txt` policy file.

6. Validate your setup, for example by using the [MTA-STS validator](https://aykevl.nl/apps/mta-sts/) created by [@aykevl](https://github.com/aykevl/mta-sts).

*Optional (but __highly recommended__):*

7. Create a `TLSRPT` record for `_smtp._tls.<your_domain.tld>` in your domain's DNS to enable reporing

   ```dns
   #HOST         #TTL    #TYPE    #VALUE
   _smtp._tls    3600    TXT      "v=TLSRPTv1; rua=mailto:tls-rua@mailcheck.<your_domain.tld>"
   ```

   Note that the e-mail recipient mailbox shall be on a different domain _without_ MTA-STS being configured.
   It is also quite painful to manually deal with the reports other e-mail providers will send to you. For that particular reason, you may want to consider using a 3rd-party tool like [Report URI](https://report-uri.com/), [URIports](https://www.uriports.com/), or from other commercial providers. You probably want this to be the same tool you might already be using for DMARC reports, like [DMARC Analyzer](https://www.dmarcanalyzer.com/) or [Dmarcian](https://dmarcian.com/).
