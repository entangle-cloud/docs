---
icon: lucide/toy-brick
---

# WordPress Integration

To get our AI-powered agents up and running on your website, you’ll first need a **registered account and an active subscription.**

> **Plugin version:** 1.0.0
> **Requires WordPress:** 5.9+
> **Requires PHP:** 7.4+
> **Tested up to:** WordPress 6.7

---

## Overview

The **Entangle plugin** connects your WordPress website to an AI-powered chatbot that answers visitor questions based on your site's own content. Once installed and configured, the chatbot widget appears automatically on every page of your site — no coding required.

The plugin works by injecting three things into your site's HTML:

- A **stylesheet** that styles the chatbot widget
- A **JavaScript module** that powers the chatbot interface
- An **`<entangle-app>`** HTML element that renders the widget on the page

---

## Requirements

Before installing, make sure you have:

- A WordPress site running **version 5.9 or higher**
- **PHP 7.4 or higher** on your hosting server
- An active **Entangle subscription** — [sign up at entangle.ch](https://entangle.ch)
- Your **Script URL** and **CSS URL** (provided after subscribing)
- A WordPress theme that supports `wp_body_open()` (all modern themes do — see [Theme Compatibility](#theme-compatibility))

---

## Installation

There are two ways to install the plugin.

### Option A — Upload via WordPress Admin (recommended)

1. Download the `entangle-injector.zip` file from [entangle.ch](https://entangle.ch) or your Entangle dashboard
2. Log in to your WordPress admin panel
3. Go to **Plugins → Add New**
4. Click **Upload Plugin** at the top of the page
5. Click **Choose File**, select the `entangle-injector.zip` file, and click **Install Now**
6. Once installed, click **Activate Plugin**

### Option B — Manual FTP Upload

1. Unzip `entangle-injector.zip` on your computer
2. Connect to your server via FTP or your hosting file manager
3. Upload the `entangle-plugin/` folder to `/wp-content/plugins/`
4. Log in to your WordPress admin panel
5. Go to **Plugins → Installed Plugins**
6. Find **Entangle Injector** and click **Activate**

---

## Getting Your Credentials

After activating your Entangle subscription at [entangle.ch](https://entangle.ch), you will receive:

| Credential             | Description                          | Example                                 |
| ---------------------- | ------------------------------------ | --------------------------------------- |
| **Script URL**         | URL to your unique JavaScript module | `https://hicafe.co/script/your-uuid.js` |
| **CSS URL**            | URL to your unique stylesheet        | `https://hicafe.co/style/your-uuid.css` |
| **reCAPTCHA Site Key** | Optional — for Google reCAPTCHA v3   | `6LdBIyEs...`                           |

These credentials are unique to your subscription and should not be shared with other websites or third parties.

If you have not yet subscribed or cannot find your credentials, contact [hello@entangle.ch](mailto:hello@entangle.ch).

---

## Configuration

Once the plugin is activated:

1. In your WordPress admin sidebar, click **Entangle**
2. You will see the Entangle settings page with the following fields:

### Script URL

Paste your full JavaScript module URL here.

```
https://hicafe.co/script/1331876c-5fc9-440c-a367-09666bf50c78.js
```

This is injected just before the closing `</body>` tag as a `type="module"` script.

### CSS URL

Paste your full stylesheet URL here.

```
https://hicafe.co/style/343b9d95-fb66-41b5-bba7-898db30ed07d.css
```

This is injected inside the `<head>` tag of every page.

### reCAPTCHA Site Key

Paste your Google reCAPTCHA v3 site key here (optional). Leave blank to skip. See [Google reCAPTCHA Setup](#google-recaptcha-setup) for details.

### Enable Injection

A toggle switch to turn the widget on or off sitewide. When disabled, nothing is injected into your site's HTML — your saved URLs and settings are preserved.

3. Click **Save Settings**

Your chatbot widget is now live on your site.

---

## What Gets Injected

Once configured and enabled, the plugin injects the following into every page of your WordPress site:

### Inside `<head>`

```html
<link
  rel="stylesheet"
  crossorigin
  href="https://hicafe.co/style/your-uuid.css"
/>

<script src="https://www.google.com/recaptcha/api.js?render=YOUR_SITE_KEY"></script>
```

### After opening `<body>`

```html
<entangle-app></entangle-app>
```

### Before closing `</body>`

```html
<script
  type="module"
  crossorigin
  src="https://hicafe.co/script/your-uuid.js"
></script>
```

---

## Enabling & Disabling the Widget

You can turn the widget on or off at any time without losing your saved settings:

1. Go to **Entangle** in your WordPress admin sidebar
2. Toggle the **Enable Injection** switch
3. Click **Save Settings**

When disabled, no scripts, styles, or HTML elements are injected into your site. This is useful for maintenance or testing.

---

## Google reCAPTCHA Setup

reCAPTCHA v3 helps protect the chatbot from spam and bot abuse. To set it up:

1. Go to the [Google reCAPTCHA Admin Console](https://www.google.com/recaptcha/admin)
2. Sign in with your Google account
3. Click **+** to register a new site
4. Set the **reCAPTCHA type** to **Score based (v3)**
5. Add your website domain under **Domains** (e.g. `yoursite.com`)
6. Click **Submit**
7. Copy the **Site Key** (not the Secret Key)
8. Paste it into the **reCAPTCHA Site Key** field in the Entangle settings page
9. Click **Save Settings**

> **Note:** reCAPTCHA is optional. If you leave the field blank, reCAPTCHA will not be loaded on your site.

---

## Theme Compatibility

The `<entangle-app>` element is injected via the WordPress `wp_body_open` hook, which requires your theme to call `wp_body_open()` immediately after the opening `<body>` tag.

**All themes from WordPress.org and most modern commercial themes support this.** You can verify by checking your theme's `header.php` for this line:

```php
wp_body_open();
```

### If your theme does not support `wp_body_open`

The chatbot stylesheet and JavaScript will still load, but the `<entangle-app>` element may not render correctly. In this case you have two options:

**Option 1 — Add it to your theme manually**

Open your theme's `header.php` and add the following immediately after the `<body` tag:

```php
<body <?php body_class(); ?>>
<?php wp_body_open(); ?>
```

**Option 2 — Use a child theme or plugin**

Use a child theme or a hook-injection plugin to add `wp_body_open()` support without editing theme files directly.

If you are unsure, contact [hello@entangle.ch](mailto:hello@entangle.ch) for assistance.

---

## Troubleshooting

### The chatbot widget is not appearing on my site

- Confirm the **Enable Injection** toggle is turned on in the settings page
- Check that both the **Script URL** and **CSS URL** fields are filled in and saved
- Open your browser's developer tools (F12), go to the **Network** tab, and reload the page — confirm the script and stylesheet URLs are loading without errors (status 200)
- Confirm your theme supports `wp_body_open()` — see [Theme Compatibility](#theme-compatibility)

### The widget appears but has no styling

- Check that your **CSS URL** is correct and the file is accessible — paste the URL directly into your browser to verify
- Check for CSS conflicts with your theme by temporarily switching to a default WordPress theme (e.g. Twenty Twenty-Four) to test

### The widget appears but does not respond to messages

- Verify your **Script URL** is correct and the `.js` file loads without errors in the browser console
- Confirm your Entangle subscription is active at [entangle.ch](https://entangle.ch)
- Check the browser console for JavaScript errors

### I see a "Failed to load resource" error in the browser console

- Your Script URL or CSS URL may be incorrect or expired — log in to your Entangle dashboard to retrieve fresh URLs
- Check if your server or a security plugin (e.g. Wordfence) is blocking external scripts — whitelist `hicafe.co` if necessary

### The settings page is not saving

- Make sure you have the **Administrator** role in WordPress
- Try deactivating and reactivating the plugin
- Check if a security plugin is blocking the `options.php` form submission

---

## Uninstalling

To remove the plugin from your WordPress site:

1. Go to **Entangle** in your admin sidebar and toggle **Enable Injection** off — this immediately stops all widget injection
2. Go to **Plugins → Installed Plugins**
3. Click **Deactivate** under Entangle Injector
4. Click **Delete**

> **Note:** Deactivating or deleting the plugin removes all injected code from your site's frontend immediately. Your saved settings (Script URL, CSS URL, reCAPTCHA key) are stored in the WordPress options table and will be removed when the plugin is deleted.

---

## FAQ

**Do I need to know how to code to use this plugin?**
No. Everything is configured through the WordPress admin settings page. No coding is required.

**Can I use the same credentials on multiple WordPress sites?**
No. Each Entangle subscription is tied to a specific website. Each site requires its own subscription and unique credentials. Contact [hello@entangle.ch](mailto:hello@entangle.ch) to discuss multi-site plans.

**Will the plugin slow down my website?**
No. The JavaScript module uses `type="module"` which is non-blocking by default. The stylesheet is a standard `<link>` tag loaded from a fast CDN.

**Does it work with caching plugins like WP Rocket or W3 Total Cache?**
Yes. The plugin uses standard WordPress hooks (`wp_head`, `wp_footer`, `wp_body_open`) which are compatible with all major caching plugins.

**Does it work with Elementor, Divi, or other page builders?**
Yes. The plugin injects code at the theme level, so it works independently of any page builder.

**Does it work on WooCommerce stores?**
Yes. The chatbot widget will appear on all pages including WooCommerce product and checkout pages.

**What happens when I update WordPress?**
The plugin is self-contained and does not depend on any WordPress core internals that change between updates. It should continue to work after WordPress updates without any action needed.

**Can the chatbot be shown only on specific pages?**
The current version injects the widget sitewide. Page-specific display rules are on the Entangle roadmap. Contact [hello@entangle.ch](mailto:hello@entangle.ch) if this is a requirement for your setup.

**My question isn't answered here — how do I get help?**
Email [hello@entangle.ch](mailto:hello@entangle.ch) with a description of your issue and your WordPress version.

---

_Documentation for Entangle Injector v1.0.0 — [entangle.ch](https://entangle.ch)_
