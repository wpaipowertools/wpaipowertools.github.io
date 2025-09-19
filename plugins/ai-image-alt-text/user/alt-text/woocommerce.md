---
layout: default
slug: ai-image-alt-text
menu: user
title: WooCommerce Image Alt Text
---
With the [pro](../pro) version of our [WordPress AI Alt Text Generator Plugin](https://www.wpaiplugins.dev/wordpress-image-alt-text-ai-plugin/), you can generate context-aware alt text for WooCommerced product images. This means AI uses both the image itself, as well as the information from the product it's tied to, to generate the most appropriate alt text.

To enable this, go to the **WooCommerce** settings page and toggle on the **WooCommerce Enabled** option.

Here you can also choose which elements from the product you would like to include in the context that is sent to OpenAI, as well as whether or not gallery images should be include (otherwise it's just the featured image).

![Screenshot of the WooCommerce settings page](/img/{{ page.slug }}/iat-woocommerce-settings.png)

Now, when you upload a new image directly via the product edit screen and then use our plugin to generate alt text for that image, it will take into account the selected elements from the product as well as the image.