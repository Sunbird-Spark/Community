# Verifiable credentials and certificates

#### Certificate issuance

Certificates in Spark are issued automatically when a learner meets the completion criteria for a course batch.&#x20;

A certificate:

* Is generated as an `SVG` file
* Contains the learner's name, course name, batch name, issue date, and a unique certificate ID
* Is stored in **Sunbird RC** (Registry and Credentialing)
* Is publicly verifiable through a unique verification URL

#### Certificate download

Learners can download their certificate as a `PDF` from the User Report page or learning passbook. The `SVG` is converted to `PDF` locally in the browser.

#### Certificate reissue

If a learner's certificate was issued incorrectly, such as because of a name mismatch, administrators can trigger a certificate reissue from the batch management interface.

#### Public verification

Every certificate has a public verification URL. Anyone, including employers and institutions, can visit that URL to confirm that the certificate is genuine and platform-issued. No account is required.

{% hint style="info" %}
**Upcoming:** Sunbird RC `v2`, with support for the `W3C Verifiable Credentials (VC)` standard, is planned for the April to June 2026 release. `RC v1` is the current implementation.
{% endhint %}
