---
title: Subresource Integrity
slug: Web/Security/Subresource_Integrity
page-type: guide
browser-compat:
  - html.elements.link.integrity
  - html.elements.script.integrity
sidebar: security
---

**Subresource Integrity** (SRI) is a security feature that enables browsers to verify that resources they fetch (for example, from a [CDN](/en-US/docs/Glossary/CDN)) are delivered without unexpected manipulation. It works by allowing you to provide a cryptographic hash that a fetched resource must match.

> [!NOTE]
> For subresource-integrity verification of a resource served from an origin other than the document in which it's embedded, browsers additionally check the resource using [Cross-Origin Resource Sharing (CORS)](/en-US/docs/Web/HTTP/Guides/CORS), to ensure the origin serving the resource allows it to be shared with the requesting origin.

## How Subresource Integrity helps

Websites sometimes choose to rely on a third party such as a {{glossary("CDN", "Content Delivery Network (CDN)")}} to host some of their resources, rather than self-host all their resources. For example, a document served from `https://example.com` might include a resource from another location:

```html
<script src="https://not-example.com/script.js"></script>
```

This comes with a risk, in that if an attacker gains control of the third-party host, the attacker can inject arbitrary malicious content into its files (or replace the files completely) and thus can also potentially attack sites that fetch files from it.

Subresource Integrity enables you to mitigate some risks of attacks such as this, by ensuring that the files your web application or web document fetches have been delivered without an attacker having injected any additional content into those files — and without any other changes of any kind at all having been made to those files.

## Using Subresource Integrity

You use the Subresource Integrity feature by specifying a base64-encoded cryptographic hash of a resource (file) you're telling the browser to fetch, in the value of the `integrity` attribute of a {{HTMLElement("script")}} element or a {{HTMLElement("link")}} element with [`rel="stylesheet"`](/en-US/docs/Web/HTML/Reference/Attributes/rel#stylesheet), [`rel="preload"`](/en-US/docs/Web/HTML/Reference/Attributes/rel/preload), or [`rel="modulepreload"`](/en-US/docs/Web/HTML/Reference/Attributes/rel/modulepreload).

An `integrity` value begins with at least one string, with each string including a prefix indicating a particular hash algorithm (currently the allowed prefixes are `sha256`, `sha384`, and `sha512`), followed by a dash, and ending with the actual base64-encoded hash.

> [!NOTE]
> An **integrity** value may contain multiple hashes separated by whitespace. A resource will be loaded if it matches one of those hashes.

Example `integrity` string with base64-encoded sha384 hash:

```plain
sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC
```

So `oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC` is the "hash" part, and the prefix `sha384` indicates that it's a sha384 hash.

> [!NOTE]
> An `integrity` value's "hash" part is, strictly speaking, a **_cryptographic_ _digest_** formed by applying a particular hash function to some input (for example, a script or stylesheet file). But it's common to use the shorthand "hash" to mean _cryptographic_ _digest_, so that's what's used in this article.

### Tools for generating SRI hashes

#### SRI Hash Generator

The [SRI Hash Generator](https://www.srihash.org/) is an online tool you can use to generate SRI hashes.

#### Using OpenSSL

You can generate SRI hashes from the command-line using **OpenSSL** with a command invocation such as:

```bash
cat FILENAME.js | openssl dgst -sha384 -binary | openssl base64 -A
```

In a Windows environment, you can create a tool for generating SRI hashes with the following code:

```batch
@echo off
set bits=384
openssl dgst -sha%bits% -binary %1% | openssl base64 -A > tmp
set /p a= < tmp
del tmp
echo sha%bits%-%a%
pause
```

To use that code:

1. Save that code in a file named `sri-hash.bat` in the Windows SendTo folder in your environment (for example, `C:\Users\USER\AppData\Roaming\Microsoft\Windows\SendTo`).
2. Right-click a file in the File Explorer, select **Send to…**, and then select `sri-hash`. You will see the integrity value in a command box.
3. Select the integrity value and right-click to copy it to the Clipboard.
4. Press any key to dismiss the command box.

> [!NOTE]
> If OpenSSL is not installed on your system, visit the [OpenSSL project website](https://www.openssl.org/) for information about downloading and installing it. The OpenSSL project does not itself host binary distributions of OpenSSL, but does maintain an informal list of third-party distributions: https://github.com/openssl/openssl/wiki/Binaries.

#### Using shasum

You can generate SRI hashes using [**shasum**](https://linux.die.net/man/1/shasum) with a command invocation such as:

```bash
shasum -b -a 384 FILENAME.js | awk '{ print $1 }' | xxd -r -p | base64
```

- The pipe-through `xxd` step takes the hexadecimal output from `shasum` and converts it to binary.
- The pipe-through `awk` step is necessary because `shasum` will pass the hashed filename in its output to `xxd`. That can have disastrous consequences if the filename happens to have valid hex characters in it — because `xxd` will also decode that and pass it to `base64`.

### Cross-Origin Resource Sharing and Subresource Integrity

For subresource-integrity verification of a resource served from an origin other than the document in which it's embedded, browsers additionally check the resource using [Cross-Origin Resource Sharing (CORS)](/en-US/docs/Web/HTTP/Guides/CORS), to ensure the origin serving the resource allows it to be shared with the requesting origin. Therefore, the resource must be served with an [`Access-Control-Allow-Origin`](/en-US/docs/Web/HTTP/Reference/Headers/Access-Control-Allow-Origin) header that allows the resource to be shared with the requesting origin; for example:

```http
Access-Control-Allow-Origin: *
```

## Examples

In the following examples, assume that `oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC` is already known to be the expected SHA-384 hash (digest) of a particular script `example-framework.js`, and there's a copy of the script hosted at `https://example.com/example-framework.js`.

### Subresource Integrity with the \<script> element

You can use the following {{HTMLElement("script")}} element to tell a browser that before executing the `https://example.com/example-framework.js` script, the browser must first compare the script to the expected hash, and verify that there's a match.

```html
<script
  src="https://example.com/example-framework.js"
  integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
  crossorigin="anonymous"></script>
```

> [!NOTE]
> For more details on the purpose of the `crossorigin` attribute, see [CORS settings attributes](/en-US/docs/Web/HTML/Reference/Attributes/crossorigin).

## How browsers handle Subresource Integrity

Browsers handle SRI by doing the following:

1. When a browser encounters a {{HTMLElement("script")}} or {{HTMLElement("link")}} element with an `integrity` attribute, before executing the script or before applying any stylesheet specified by the {{HTMLElement("link")}} element, the browser must first compare the script or stylesheet to the expected hash given in the `integrity` value.

   For subresource-integrity verification of a resource served from an origin other than the document in which it's embedded, browsers additionally check the resource using [Cross-Origin Resource Sharing (CORS)](/en-US/docs/Web/HTTP/Guides/CORS), to ensure the origin serving the resource allows it to be shared with the requesting origin.

2. If the script or stylesheet doesn't match its associated `integrity` value, the browser must refuse to execute the script or apply the stylesheet, and must instead return a network error indicating that fetching of that script or stylesheet failed.

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Content Security Policy](/en-US/docs/Web/HTTP/Guides/CSP)
- The {{httpheader("Content-Security-Policy")}} HTTP header.
- [A CDN that can not XSS you: Using Subresource Integrity](https://frederikbraun.de/using-subresource-integrity.html)
- [SRI Hash Generator](https://www.srihash.org/)
