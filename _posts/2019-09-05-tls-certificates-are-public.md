---
title: "By the way, the list of SSL/TLS certificates issued to you (including subdomains) is public!"
layout: post
category: en
lang: "en"
---

*(First published on [Medium.com](https://linasvaliukas.medium.com/by-the-way-the-list-of-ssl-tls-certificates-issued-to-you-including-subdomains-is-public-5537ef1f11f5))*

With the advent of [Let's Encrypt](https://letsencrypt.org/), the price of having a SSL/TLS-secured website has dropped to zero, and pretty soon every flower pot will be shipping with a OpenSSL build! But did you know that all the certificates, including lists of alias subdomains for the non-wildcard ones, are publicly audited information? Are you comfortable with the fact that others can find out about your production / staging environments (in the form of subdomains) simply by querying a public database?

Long story short, Google has decided to patch up some "structural flaws" with how certificates get issued, who can issue what certificate and to whom, in an attempt to curb rogue or hacked CAs. What they came up with is the [Certificate Transparency project](https://www.certificate-transparency.org/) that publishes [logs](https://www.certificate-transparency.org/known-logs) of issued SSL/TLS certs which browsers then check to spot the nasty ones.

Aside from browsers, there exist a few aggregators which collect said logs and make them searchable. About a year ago, I’ve used <https://crt.sh/> for running queries against a list of recently issued certificates, but now the website seems to be kind of broken, so you might want to try out a commercial alternative that I have googled up just today:

**<https://censys.io/certificates>**

This tool is somewhat limited though, e.g. it lets you run up to 10 queries without registering:

![](images/2019-09-05-tls-certificates-are-public/censys-certs.png "I'm not sure if there's any basis to me blurring out the publicly available data, but I won't take the risk!")

When you issue yourself a SSL/TLS certificate through, say, Let's Encrypt, which includes both the domain and its subdomains, the domain (e.g. `example.com`) gets set as certificate's Common Name (CN), and both the main domain and subdomains (`example.com`, `foo.example.com` and `bar.example.com`) get registered under certificate's Subject Alternative Name (SAN). You can search for those on the Censys's tool by picking the `parsed.extensions.subject_alt_name.dns_names` parameter - with that, you'd be effectively querying both CNs and SANs.

For example, let's check out whether someone issued themselves a certificate for their RabbitMQ deployment! Enter the following query into the search box:

```
parsed.extensions.subject_alt_name.dns_names: rabbitmq*
```

We're getting somewhere:

![](images/2019-09-05-tls-certificates-are-public/censys-rabbitmq.png)

It turns out that quite a few users issued CA-signed certificates for their RabbitMQ deployments! Are they secure though? RabbitMQ has a web management UI plugin which is accessible at port 15672 by default, with `guest:guest` being the default credentials. Some users managed to change those:

![](images/2019-09-05-tls-certificates-are-public/rabbitmq-secured.png)

...yet others didn't:

![](images/2019-09-05-tls-certificates-are-public/rabbitmq-not-secured.png)

I've also managed to find a RabbitMQ deployment with what appeared to be credit card numbers being processed as AMQP messages, but I didn't check whether it was real data nor I'm going to post a screenshot with those, for legal reasons.

Some other things to search for to satisfy one's curiosity:

* Other popular tools with no auth by default or simple credentials, e.g. Solr, Elasticsearch, Redis.
* Plethora of IOT gadgets.
* Recently set up Wi-Fi routers.
* Big-name domains, e.g. companies which do unsolicited credit ratings of all US citizens and occasionally get into hot water over their lacking security practices.

So, make sure that your operations aren't more public that you'd like them to be! Prevent internal services from being accessed publicly, use self-signed certificates, or encrypt communication using a different OSI level altogether. Take care!
