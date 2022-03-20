---
title: WordPress 5.0 Update is Available, New Editor, New Theme
tags:
  - WordPress
id: '3651'
categories:
  - - Development
date: 2018-12-07 07:14:57
languages:
  zh-CN: https://guozeyu.com/2018/12/wordpress-5-0-update/
---

The official version of WordPress 5.0 has been released on 2018-12-07. Users of self-built WordPress can upgrade in the web management page of the WordPress background. The two most obvious features of this update are: 1. The background management uses a new editor, and 2. The theme of 2019 has been added, which adapts to this new editor.
<!-- more -->

This time 5.0 is a major version update, unlike the previous 4.9 and 4.8 updates, which can be seen from the version number.

## Editor

The first impression of this new block editor (Gutenberg Editor) is that it is more concise, this new editor has more white space. When you use the editor for the first time, you will feel a little uncomfortable, because the new editor does not have the familiar toolbar. In its place is a simple Add Block button and some other basic operations.

In the new version of the editor, every paragraph, image, subtitle, quote, etc. is a "block". You can do custom actions for each block, such as setting a different font size, font color, and even background color and custom CSS for each natural segment. And all of this is visualized through the "Components" tool on the right.

You can drag blocks directly to achieve effects such as dragging natural segments.

### Classic Block

The new editor is compatible with the boss editor. To use the old editing mode, you can do this by inserting one or more Classic Blocks. This classic block is on par with other blocks in the new editor, such as natural paragraph blocks, subtitle blocks, etc. Classic blocks can contain one or more natural paragraphs, subheadings, and for simple typography, it can replace the latest block editor. When using classic blocks, you can still see the familiar toolbar.

![Classic Block](https://cdn.ze3kr.com/6T-behmofKYLsxlrK0l_MQ/3c1dd583-f116-4d73-dadf-b81986f82b00/large)

Classic Block Screenshot

When editing previously published articles and pages, the original editing mode is still used by default, that is, **a** classic block is included in the new editor.

### Supported Legacy Blocks

The original blocks are all corresponding to HTML tags and have relatively few functions. Classic blocks are used for compatibility with previous generation editors. To use the new features, you need to learn to use these new blocks.

* Paragraph: corresponding to the original `<p>`
* Heading: Corresponds to the original `<h1>` ~ `<h6>` heading.
* List block (List): corresponds to the original `<ul>` or `<ol>` list.
* More: Used to display "Read more" on the home page, corresponding to the original `<!--more-->`.
* Image block (Image): corresponds to the original `<img>`.
* Shortcode: In the new version, you can use the Shortcode block to insert Shortcode, which can make the editing interface more concise.
* Quote block (Quote): corresponds to the original `<blockquote>`. The new version of the application block supports citations (Citation).
* Video block (Video): corresponds to the `video` Shortcode in the previous version of the WordPress editor.
* Separator: Corresponds to the original `<hr>`.
* Multi-line code block (Preformatted): corresponds to the original `<pre>`.
* Code block (Code): also supports multi-line, corresponding to the original `<pre>` and `<code>` combination.

### Some New Blocks

Many new blocks have been added in the new version. By using these blocks, you can directly insert some content you want visually without editing the source code or using plugins.

* Custom HTML block (Custom HTML): Although you can use Edit as HTML in many blocks, it is recommended to use this block if you really need to insert custom HTML.
* Blank block (Spacer): Insert a certain height of blank space in the article.
* Gallery: Insert multiple photo galleries.
* Audio (Audio)
* File (File)
* Background Image (Cover): Use this block to add text on top of the inserted image.
* Table: You can finally insert tables visually in WordPress.
* Important Quote (Pull Quote): Different from the style of the quote block, it is also a quote.
* Verse: Similar to a multi-line code block, it is also implemented using `<pre>`. It can realize the monospace display of English characters.
* Button (Button): You can add button-style hyperlinks.
* Columns
* Paging (Page Break)
* Media & Text: Used to display media and text left and right.

The following could have been added as Widgets in the web menu. It is now also possible to add directly in the article as a block.

* Archive
*   Classification
*   latest articles
*   latest comment

In addition, there is now an Embed function, which can directly embed third-party content, such as Twitter, YouTube, SoundCloud, etc.

## 2019 Themes

In order to better match all the typographic features of the new editor, WordPress also launched a new 2019 default theme. [Details](https://make.wordpress.org/core/2018/10/16/introducing-twenty-nineteen/)

![Screenshot of article interface](https://cdn.ze3kr.com/6T-behmofKYLsxlrK0l_MQ/b26e6735-e9c6-46a4-3c61-63dfb9f93d00/large)

Screenshot of the theme homepage

Just like the default themes in the past, the new WordPress themes are also very versatile. However I feel that the new 2019 theme is not as clean as the previous 2017 theme (the one this blog is using).

![Screenshot of article interface](https://cdn.ze3kr.com/6T-behmofKYLsxlrK0l_MQ/691e4199-50da-4c57-d46a-1399b5801b00/large)

## Should it be Upgraded? How to Upgrade?

WordPress does not guarantee continued security updates for older versions, so you should upgrade to the latest 5.0 version. However, in fact, as can be seen from the version history of WordPress, WordPress is still maintaining 3.7 (released on 2013-10-24) and all subsequent versions. So even if you don't update to version 5.0, you may continue to receive security updates.

It should be noted that every time you update a WordPress version, especially a major version update, some functions in the source code will change, which means that there is no guarantee that your plugin will work correctly in the new WordPress. You should check that the plugins you have enabled work correctly on the latest version, or that an update for the new version has been released. If you're a plugin developer, you should have a site with a beta version of WordPress installed, and it's long overdue for your plugin to work with the latest version.

Be sure to backup your site before upgrading. For WordPress, you need to back up the WordPress code directory and database contents.
