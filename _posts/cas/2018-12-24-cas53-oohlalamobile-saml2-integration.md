---
layout:     post
title:      Apereo CAS - OohLala Mobile SAML2 Integration
summary:    Learn how to integrate OohLala Mobile with Apereo CAS running as a SAML2 identity provider.
tags:       [CAS,SAML]
---

<div class="alert alert-success">
  <strong>Collaborate</strong><br/>The blog is managed and hosted on GitHub. If you wish to update the contents of this post or if you have found an inaccuracy and wish to make corrections, we recommend that you please submit a pull request to <a href="https://github.com/apereo/apereo.github.io">this repository</a>.
</div>

<div class="alert alert-info">
  <strong>Contributed Content</strong><br/>Paul Spaude of Unicon, Inc was kind enough to contribute this guide.
</div>

Ready Communication's OohLala Mobile provides a mobile community and communication tools for higher education. As a SAML2 service provider, it can be integrated with CAS running as a SAML identity provider and this blog post provides a quick walkthrough of how to make that integration possible.

Our starting position is based on the following:

- CAS `5.3.x`
- Java 8
- [Maven Overlay](https://github.com/apereo/cas-overlay-template) (The `5.3` branch specifically)

# CAS Configuration

First, ensure that your CAS deployment is equipped to act as a [SAML2 identity provider](https://apereo.github.io/cas/5.3.x/installation/Configuring-SAML2-Authentication.html). Next, you may also use the [JSON Service Registry](https://apereo.github.io/cas/5.3.x/installation/JSON-Service-Management.html) to keep track of the relying-party registration record in JSON definition files.

The JSON file to contain the service provider relying-party record would be as follows:

```json
{
  "@class" : "org.apereo.cas.support.saml.services.SamlRegisteredService",
  "serviceId" : "https://integration.oohlalamobile.com/saml/[instance-name]/metadata",
  "name" : "OohLaLa",
  "id" : 1,
  "metadataLocation" : "https://integration.oohlalamobile.com/saml/[instance-name]/metadata",
  "encryptAssertions" : false,
  "signAssertions" : false,
  "signResponses" : true,
  "attributeReleasePolicy" : {
    "@class" : "org.apereo.cas.services.ReturnAllowedAttributeReleasePolicy",
    "allowedAttributes" : [ "java.util.ArrayList", ["givenName","sn","mail","uid"]]
  }
}
```

A few things to point out:

- You will need to adjust the `metadataLocation` to match your instance.
- Make sure CAS has retrieved the allowed attributes (i.e. `givenName`, `sn` etc) listed in the JSON definition file.
- Make sure the SP metadata has the correct entity id, matching the instance.

# Finale

Thanks to Paul Spaude of Unicon, Inc who was kind enough to share the above integration notes. If you too have an integration with a well-known SAML2 service provider, please consider sharing that guide in form of a blog post here as well.

[Misagh Moayyed](https://fawnoos.com)
