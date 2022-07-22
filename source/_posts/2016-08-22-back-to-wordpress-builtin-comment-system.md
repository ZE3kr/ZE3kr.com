---
title: Back to WordPress' Built-In Comment System
tags:
  - WordPress
id: '1922'
categories:
  - - Development
date: 2016-08-22 11:32:57
languages:
  zh-CN: https://guozeyu.com/2016/08/back-to-wordpress-builtin-comment-system/
---

Recently, Disqus has been very unstable by the wall of a certain country, so it has used the comment system that comes with WordPress, but this comment system does not have an email reminder when the commenter is replied. I have my own sending server (AWS SES) system, so in theory, this effect can be achieved with plug-ins. But I have seen a lot of plug-ins. Basically, the operation pages are too complicated, and the reply emails usually do not support Chinese. I only need a simple reply system, so I don’t need to be so troublesome, so I just developed one myself, and finally realized this perfectly. Function.

The feature of this feature I developed is: when the commenter is replied, the email title is "Re: \[article title\]", so that when a commenter's message is replied by multiple people, it will be automatically sent to the local email client It is classified into one category; and the commenter can reply to the email directly after receiving the email, and will **directly** send an email to the sender of the reply (it will not be displayed on the website, and I can't see it, it will be two people's private chat).

The content of the email is concise, without extra useless things, and will not be recognized as Spam.

<img src="https://cdn.yangxi.tech/6T-behmofKYLsxlrK0l_MQ/1af77d8d-e266-4b68-78e2-9ee5bd3ff001/extra" alt="Example of delivery" width="1338" height="1302"/>

All code has been put on [GitHub Gist](https://gist.github.com/ZE3kr/8c51a6349462935cefd2e636e96e93f8).

```
<?php
function tlo_comment_mail_notify($comment_id) {
	global $comment_author;
	$comment = get_comment($comment_id);
	$parent_id = $comment->comment_parent ? $comment->comment_parent : '';
	$spam_confirmed = $comment->comment_approved;
	$from = $comment->comment_author_email;
	$to = get_comment($parent_id)->comment_author_email;
	if (($parent_id != '') && ($spam_confirmed != 'spam') && $from != $to && $to != get_bloginfo('admin_email') ) {
		$blog_name = get_option('blogname');
		$blog_url = site_url();
		$post_url = get_permalink( $comment->comment_post_ID );
		$comment_author = $comment->comment_author;
		$subject = 'Re: '.html_entity_decode(get_the_title($comment->comment_post_ID));
		$headers[] = 'Reply-To: '.$comment_author.' <'.$comment->comment_author_email.'>';
		$comment_parent = get_comment($parent_id);
		$comment_parent_date = tlo_get_comment_date( $comment_parent );
		$comment_parent_time = tlo_get_comment_time( $comment_parent );
		$message = <<<HTML
<!DOCTYPE html>
<html lang="zh">
	<head>
		<meta content="text/html; charset=utf-8" http-equiv="Content-Type">
		<title>$blog_name</title>
	</head>
	<body>
		<style type="text/css">
		img {
			max-width: 100%; height: auto;
		}
		</style>
		<div class="content">
			<div>
				<p>$comment->comment_content</p>
			</div>
		</div>
		<div class="footer" style="margin-top: 10px">
			<p style="color: #777; font-size: small">
				&mdash;
				<br>
				Reply to this email to communicate with replier directly, or <a href="$post_url#comment-$comment_id">view it on $blog_name</a>.
				<br>
				You're receiving this email because of your comment got replied.
			</p>
		</div>
		<blockquote type="cite">
			<div>On {$comment_parent_date}, {$comment_parent_time}，$comment_parent->comment_author &lt;<a href="mailto: $comment_parent->comment_author_email">$comment_parent->comment_author_email</a>&gt; wrote：</div>
			<br>
			<div class="content">
				<div>
					<p>$comment_parent->comment_content</p>
				</div>
			</div>
		</blockquote>
	</body>
</html>
HTML;
		add_filter( 'wp_mail_content_type', 'tlo_mail_content_type' );
		add_filter( 'wp_mail_from_name', 'tlo_mail_from_name' );
		wp_mail( $to, $subject, $message, $headers );
	}
}
add_action('tlo_comment_post_async', 'tlo_comment_mail_notify');

function tlo_comment_mail_notify_async($comment_id) {
	wp_schedule_single_event( time(), 'tlo_comment_post_async', [$comment_id] );
}
add_action('comment_post', 'tlo_comment_mail_notify_async');
// add_action('comment_post', 'tlo_comment_mail_notify');

function tlo_mail_content_type() {
	return 'text/html';
}
function tlo_mail_from_name() {
	global $comment_author;
	return $comment_author;
}

function tlo_get_comment_time( $comment ) {
	$date = mysql2date(get_option('time_format'), $comment->comment_date, true);

	return apply_filters( 'tlo_get_comment_time', $date, $comment );
}
function tlo_get_comment_date( $comment ) {
	$date = mysql2date(get_option('date_format'), $comment->comment_date);

	return apply_filters( 'tlo_get_comment_date', $date, $comment );
}
```