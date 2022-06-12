# security-headers
Security Headers Documentation

## Overview
Adding security headers to your .htaccess file can help to secure your website and its data. This article explains how to add the following security headers.

- Content-Security-Policy
- Strict-Transport-Security (HSTS)
- X-Frame-Options
- Cross-site Scripting protection (XSS)
- X-Content-Type-Options
- Referrer Policy
- Feature-Policy
- CORS headers

## Adding an .htaccess file on your web server
The examples in this article assume your site is on an Apache server and you are adding headers to your site's .htaccess file. View the following article for an overview of what an .htaccess file is and how to add one to your site.

## 1) Content-Security-Policy
The Content-Security-Policy header specifies approved sources of content that the browser may load from your website. When you whitelist approved content sources, you thereby help to prevent malicious code from loading on your site. This is a way to help reduce XSS risks. 

View the following page for further details:
- [https://content-security-policy.com/](https://content-security-policy.com/)

This example allows any asset to be loaded only from your website.
<pre><code>Header set Content-Security-Policy "default-src 'self'"</code></pre>

This example allows any asset to be loaded from your domain over HTTPS on port 443 only.
<pre><code>Header set Content-Security-Policy "default-src https://example.com:443"</code></pre>

You can then test if it's active by running the following curl command via SSH:
<pre><code>curl -I https://example.com</code></pre>
If you see the following expression, it means you have been successful.
<pre><code>Content-Security-Policy: default-src https://example.com:443</code></pre>

### Resolving insecure site and mixed-content warnings
If your website has any assets that load over http, your site will display an SSL warning in the URL bar of your browser to notify the visitor that the connection is not safe.

The following code upgrades all requests to insecure resources automatically. This fixes the SSL warning in your browser.
<pre><code>Header always set Content-Security-Policy "upgrade-insecure-requests;"</code></pre>

## 2) Strict-Transport-Security (HSTS)
Strict-Transport-Security headers tell the browser to ONLY interact with the site using HTTPS and never HTTP. View the following pages for further details.

- [HTTP Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)
- [HTTP Strict Transport Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)

You can enable this in your .htaccess file with the following code:

<pre><code>Header set Strict-Transport-Security "max-age=31536000;includeSubDomains;"</code></pre>

You can then test if it's active by running the following curl command via SSH:
<pre><code>curl -I https://example.com</code></pre>
If you see the following expression, it means you have been successful.
<pre><code>Strict-Transport-Security: max-age=31536000;includeSubDomains;</code></pre>

## 3) X-Frame-Options
This header helps to protect your visitors against clickjacking attacks. Add this header on pages that should not be allowed to render a page within a frame. View the following links for further information:

- [https://keycdn.com/blog/x-frame-options](https://keycdn.com/blog/x-frame-options)
- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)

This example completely disables the ability to load any page in a frame.
<pre><code>Header always set X-Frame-Options DENY</code></pre>

This example only allows your website to embed an iframe on your pages.
<pre><code>Header always set X-Frame-Options SAMEORIGIN</code></pre>

You can then test if it's active by running the following curl command via SSH:
<pre><code>curl -I https://example.com</code></pre>
If you see the following expression, it means you have been successful.
<pre><code>X-Frame-Options: SAMEORIGIN</code></pre>

## 4) Cross-site Scripting protection (XSS)
The X-XSS-Protection header helps to protect your visitors against Cross-site Scripting attacks. View the following article for further details:

- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection)

In this example, the value 1 is used. This enables XSS filtering (usually default in browsers). If a cross-site scripting attack is detected, the browser will sanitize the page (remove the unsafe parts).
<pre><code>Header set X-XSS-Protection "1"</code></pre>

In this example, the value 1; mode=block is used. Rather than sanitizing the page, the browser will prevent rendering of the page if an attack is detected.
<pre><code>Header set X-XSS-Protection "1; mode=block"</code></pre>

You can then test if it's active by running the following curl command via SSH:
<pre><code>curl -I https://example.com</code></pre>
If you see the following expression, it means you have been successful.
<pre><code>X-XSS-Protection: 1; mode=block</code></pre>

## 5) X-Content-Type-Options
This header blocks content sniffing that could transform non-executable MIME types into executable MIME types. View the following article for further details:

- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options)

<pre><code>Header set X-Content-Type-Options nosniff</code></pre>

You can then test if it's active by running the following curl command via SSH:
<pre><code>curl -I https://example.com</code></pre>
If you see the following expression, it means you have been successful.
<pre><code>X-Content-Type-Options: nosniff</code></pre>

## 6) Referrer-Policy
This header controls how much referrer information from your site is sent to another server. For example, if a link on your site opens a different website, that website's server records your domain name as the referrer of that link. With this policy, you can control what referrer information is sent to that external server. View the following link for further details.

- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy)

This example does not send any referrer information.
<pre><code>Header set Referrer-Policy: no-referrer</code></pre>

You can then test if it's active by running the following curl command via SSH:
<pre><code>curl -I https://example.com</code></pre>
If you see the following expression, it means you have been successful.
<pre><code>Referrer-Policy: no-referrer</code></pre>

## 7) Feature-Policy
The Feature-Policy header controls which browser features are allowed on your website. This policy allows the website owner/developer to restrict specific APIs the site can access in the browser. Here are a few examples:

- Change the default autoplay behavior on videos.
- Restrict the site from using a camera or microphone.
- Disable the Geolocation API.

This is important if the site allows third-party content as it helps to control what those third-party apps may attempt to do with the user's browser when someone visits your website. View the following links for further information.

- [https://developers.google.com/web/updates/2018/06/feature-policy](https://developers.google.com/web/updates/2018/06/feature-policy)
- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Feature-Policy](https://developers.google.com/web/updates/2018/06/feature-policy)

This example blocks the Geolocation API in the browser from functioning on your site.
<pre><code>Header set Feature-Policy: "geolocation none"</code></pre>

You can then test if it's active by running the following curl command via SSH:
<pre><code>curl -I https://example.com</code></pre>
If you see the following expression, it means you have been successful.
<pre><code>Feature-Policy: geolocation none</code></pre>

## 8) CORS Headers
View the following links for further information.
- [https://help.dreamhost.com/hc/en-us/articles/360037198972-CORS-headers](https://help.dreamhost.com/hc/en-us/articles/360037198972-CORS-headers)
