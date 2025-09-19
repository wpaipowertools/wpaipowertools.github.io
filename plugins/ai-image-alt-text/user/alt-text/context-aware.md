---
layout: default
slug: ai-image-alt-text
menu: user
title: Context-Aware Alt Text
---
With the [pro](../pro) version of our [WordPress AI Alt Text Generator Plugin](https://www.wpaiplugins.dev/wordpress-image-alt-text-ai-plugin/), you can generate context-aware alt text. This means AI uses both the image itself, as well as the post/page it's tied to, to generate the most appropriate alt text.

To enable this, go to the **Context-Aware** settings page and toggle on the **Context Enabled** option. Here you will also find an option to set a maximum number of words to draw from the associated post/page.

![Screenshot of the context aware settings page](/img/{{ page.slug }}/iat-context-aware-settings.png)

Now, when you upload a new image directly via the post/page edit screen, that image will be tied to that post/page, and, when you use our plugin to generate alt text for that image, it will take into account the content of the post/page as well as the image.

If you find that the alt text is not as related to the content of the post/page as you would like, you can manually adjust the prompt that is sent to OpenAI. Use the **Context Template** option to add your own custom prompt. You can see an example of a custom prompt in the screenshot above.