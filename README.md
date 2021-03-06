=== wpMandrill ===

Contributors: MC_Will, Mark Nelson
Tags: mandrill, mailchimp, transactional email, email, email reliability, smtp, wp_mail, email templates
Requires at least: 3.0
Tested up to: 4.3
Stable tag: trunk
License: GPLv2

The wpMandrill plugin sends emails that are generated by WordPress through Mandrill, a transactional email service powered by MailChimp.

== Description ==

THIS IS A FORKED VERSION OF THE wpMANDRILL plugin by [MC_Will](https://profiles.wordpress.org/MC_Will/) 
This fork was created to continue development on the plugin to include features I needed that do not exist in the current version. This version was forked from the stable trunk release 1.33.

This plugin uses [Mandrill API](http://mandrillapp.com/api/docs/) to send outgoing emails, with or without attachments, from your Wordpress installation. It replaces the wp_mail function included with WordPress.

Emails are tracked and automatically tagged for statistics within the Mandrill Dashboard. You can also add general tags to every email sent, as well as particular tags based on selected emails defined by your requirements.

You can also use your own templates that have been added to your MailChimp account and shared with your Mandrill account.

There are a few levels of integrations between your WordPress installation and this plugin:

1. The simplest option: Install it, configure it, and wpMandrill will start sending your emails through Mandrill.
1. If you need to fine tune certain emails, you can change any email by creating a filter for the **mandrill_payload** hook.
1. For further customization, we've exposed a function that allows you to send emails from within your plugins, instead of the regular wp_mail function: **wpMandrill::mail**

This plugin is currently released as **beta** for early adopter evaluation and finalizing of the initial feature set.

Spanish translation available.

== Installation ==

1. Upload `wpMandrill` to the `/wp-content/plugins/` directory
1. Activate the plugin through the _Plugins_ menu in WordPress
1. Configure plugin in _Settings > Mandrill_

== Frequently Asked Questions ==

= Why do I need a Mandrill API key? =

In order to use this plugin, you have to provide one of your Mandrill API keys. That's currently the only way we can get access to your Mandrill account.

= Do I need a MailChimp account? =

Nop.

= Are all emails routed through Mandrill? =

Yes. We try to send every single email sent through your WordPress installation. We also try to process your headers and attachments.

If the sending fails for any reason, the plugin will try to send it again using the WordPress wp_mail function.

= What if I need to send some of my emails using the native wp_mail() function?

Use the mandrill_payload filter and add a new parameter called 'force_native' to the $message variable, and set it to true:

$message['force_native'] = true;

= My emails are broken and show weird CSS code =

In version 1.09, we added a setting that allows you to tell the plugin if you want to replace your line feeds by <br/>. Try playing with that switch. 

If it works for certain emails but doesn't work for others, you might want to modify this setting using the **mandrill_nl2br** filter. For example, if you don't want to use this filter for the "forgot password" emails, add something like this to your theme's functions.php file:

function my_function($nl2br, $message) {
	if ( in_array('wp_retrieve_password', $message['tags']['automatic']) ) {
        $nl2br = false;
    }	
	return $nl2br;
}
add_filter('mandrill_nl2br', 'my_function');


= Is there any way to check what's going on? =

If we couldn't send your email through Mandrill, we'll try to leave the Mandrill response in your server's log so that's your first stop.

Additionaly, if you set the WP_DEBUG constant (defined in your wp-config.php file) to true, you'll see some messages added by the plugin in key parts of the process.

= I am getting an Invalid API Key message and I'm sure my API is valid!

Please verify the following:

1. That your API key is active (this can be viewed on the SMTP & API Credentials page in your Mandrill account);
1. That your web server has either cURL installed or is able to use fsock*() functions (if you don't know what this means, you may want to check with your hosting provider for more details);
1. That the domain name you're using above is listed in the Sending Domains for your Mandrill account.

== Request ==

If you find that a part of this plugin isn't working, please don't simply click the Wordpress "It's broken" button. Let us know what's broken in [its support forum](http://wordpress.org/tags/wpmandrill?forum_id=10) so we can make it better. Our [mind-reading device](http://www.youtube.com/watch?v=cCTlonSwePs) still needs some tweaking.

== Localizations ==
wpMandrill is currently localized in the following languages:

* Spanish (es_ES)

== Known Issues ==

* Daily statistics will show data for the first 20 senders (emails) registered.
* Daily statistics will show data for the first 40 tags registered.

If your account has more than 20 senders registered or more than 40 tags used, the detailed daily statisticas might show incompleted data.

== Screenshots ==

1. Settings screen
2. Statistics
3. Dashboard widget
4. Dashboard widget Settings

== Changelog ==
= 1.00 =
* ADDED: Support for multiple CC to types
* ADDED: Support for multiple BCC to types
* Forked wpMandrill at stable version 1.33