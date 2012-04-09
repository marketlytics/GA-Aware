GA Aware
========

Default Installation
--------------------

- Track links to assets - images, pdfs, music, fonts, zips, etc.  See asset_extentions for full list
- Track External links - ex. links to facebook and twitter
- Track hash (# or jump) links
- Tack mailto links
- Track ftp, file, phone number and spotify links
- Track forms that submit to an external domain - ex. external shopping cart searches, open table searches
- Easily track multiple codes at once

Advanced Features
-----------------

- Automatically track facebook and twitter social events
- Automate cross domain configuration
- Automate converting links for cross domain
- Automate converting forms for cross domain
- Custom variable tracking
- Use event method to track 404, 500, etc. page errors - combing with selecting and "page" as the secondary dimension on the event report in the Google Analytics interface to see which pages are 404 and what the referring page was
- Use event method to track form and other page messaging

Demo: http://gaaware.com/demo/

License: MIT (included in files) http://en.wikipedia.org/wiki/MIT_License

Examples
========

Default Implementation
----------------------

REQUIRES jQuery 1.4.2 or newer
REQUIRES init code in the <head> of the document
- Note: The script uses the asynchronous version of the tracking code so even if you put your javascript at the bottom of the page this should be at the top for the best chance of capturing page views
REQUIRES UA as a string or an array of strings

    var gaAware = jQuery(document).gaaware({
    	'UA': 'UA-XXXXX-X'
    });


Cross domain tracking
---------------------

The code below will setup cross domain tracking for domain.com, a.domain.com, b.domain.com, domain2.com and c.domain2.com.
- a and b will be recognized as sub-domains of domain.com.
- c.domain2.com will not be tracked as a child of domain2.com

    var gaAware = jQuery(document).gaaware({
    	'UA': 'UA-XXXXX-X',
    	'domains': [{entity: 'domain.com', subs: ['a', 'b']}, 'domain2.com', 'c.domain2.com']
    });

Enabling Social Events
----------------------

http://code.google.com/apis/analytics/docs/tracking/gaTrackingSocial.html

    var gaAware = jQuery(document).gaaware({
    	'UA': ['UA-XXXXX-X','UA-XXXXX-X'],
    	auto_social: true
    });

How To Test
===========

- Using the browser's web inspector network tab looks for an image loaded called __utm.gif this is how Google Analytics tracks data.
- Data can also be seen by enabling the debug function and watch the web inspector console. This does not work in ie7 and older unless you write another debug_mode
- You can also watch Google Analytics' realtime reporting for some types of tracking
- To test cross domain watch the __utmz cookie for referral site data and compare the __utma cookie to make sure the analytics user id stays the same 

Optional Init Params
====================
domains
- Default: []
- Type: Array

Enables cross domain tracking for domains included, see cross domain documentation for details

include_only

- Default: null
- Type: Array

List of domains to enable tracking on, if the site is cloned for development, testing, staging, etc tracking will disable itself.

exclude_subdomains

- Default: ['preview', 'dev', 'test', 'stage', 'review', 'demo', 'train']
- Type: Array

List of sub-domains that will disable tracking if there is an exact match, useful for automatically ignoring development or testing sites

asset_extentions

- Default: ['pdf', 'txt', 'csv', 'doc', 'docx', 'xls', 'xlsx', 'ppt', 'jpg', 'jpeg', 'png', 'gif', 'psd', 'ai', 'eps', 'zip', 'xml', 'json', 'avi', 'mp4', 'mp3', 'mov', 'mpeg', 'wmv', 'rtf', 'swf', 'flv', 'js', 'css', 'eot', 'svg', 'ttf', 'woff', 'otf']
- Type: Array

List of extensions to automatically track as assets

track_alt_protocols

- Default: ['mailto', 'spotify', 'ftp', 'file', 'tel']
- Type: Array

List of altertnate protocols usually used for linking to applications

vpv_prefix

- Default: '/vpv'
- Type: String

If track_as_events is set to false, events will track as virtual page views, this string will be prefixed to make searching easier in the Google Analytics interface

track_external_links

- Default: true
- Type: Boolean

If true links to external sites not included in the "domains" cross domain definition will be tracked

external_prefix

- Default: 'External'
- Type: String

The title external links will show up under in the Google Analytics interface

asset_prefix

- Default: 'Asset'
- Type: String

The title asset links will show up under in the Google Analytics interface

hash_prefix

- Default: 'Hash'
- Type: String

The title hash(#) links will show up under in the Google Analytics interface

external_form_prefix

- Default: 'Form'
- Type: String

The title forms will show up under in the Google Analytics interface

track_as_events

- Default: true
- Type: Boolean

If true the plugin will track interactions as events as opposed to virtual page views

debug

- Default: false
- Type: Boolean

If true the plugin log debugging messages

debug_mode

- Default: function(message){ console.info(message); }
- Type: Function

A function used to display debug messages if "debug" is set to true.
NOTE: if function(message){ alert(message); } is used page will display debug messages before redirecting

track

- Default: true
- Type: Boolean

If true the plugin will track data

no_track_class

- Default: 'ga_notrack'
- Type: String

CSS class where if found on a link or form, that element will be ignored by the plugin

custom_vars

- Default: []
- Type: Array

Custom variable tracked with the initial page view

auto_social

- Default: false
- Type: Boolean

If true the plugin will include social api javascript files and setup event listeners

social_page_url

- Default: null
- Type: String

Overrides page being tracked as social event for example if the open graph tags track the homepage you may want to set events to that page

enable_facebook

- Default: true
- Type: Boolean

If true, plugin includes Facebook javascript api, prepends the required fb-root div element and attaches events

enable_twitter

- Default: true
- Type: Boolean

If true, plugin includes Twitter javascript api and attaches events

prefer_title

- Default: true
- Type: Boolean

If true, plugin uses link title attribute to label events when available instead of destination url


Methods
=======

These methods will push data to all active tracking codes

Virtual page views
------------------

    gaAware.track_virtual('asd');

Event tracking
--------------

### Params ###

- category (required) String The name you supply for the group of objects you want to track. ex. 'Videos'
- action (required) String A string that is uniquely paired with each category, and commonly used to define the type of user interaction for the web object. ex. 'Play'
- label (optional) String An optional string to provide additional dimensions to the event data. ex. 'Baby\'s First Birthday'
- value (optional) Int An integer that you can use to provide numerical data about the user event. ex. 1 (added into a total in reporting)

    gaAware.track_event('category', 'action', 'label', 1);

Track custom variable
---------------------

http://code.google.com/apis/analytics/docs/tracking/gaTrackingCustomVariables.html

NOTE: Call the _setCustomVar() function when it can be set prior to a pageview or event GIF request.
- In certain cases this might not be possible, and you will need to set another _trackPageview() request after setting a custom variable. This is typically only necessary in those situations where the user triggers a session- or visit-level custom var, where it is not possible to bundle that method with a pageview, event, or ecommerce tracking call.

### Params ###

- index (required) Int The slot for the custom variable. This is a number whose value can range from 1 - 5, inclusive. A custom variable should be placed in one slot only and not be re-used across different slots.
- name (required) String The name for the custom variable. This is a string that identifies the custom variable and appears in the top-level Custom Variables report of the Analytics reports.
- value (required) String The value for the custom variable. This is a string that is paired with a name. You can pair a number of values with a custom variable name. The value appears in the table list of the UI for a selected variable name. Typically, you will have two or more values for a given name. For example, you might define a custom variable name gender and supply male and female as two possible values.
- opt_scope (optional) Int The scope for the custom variable. As described above, the scope defines the level of user engagement with your site. It is a number whose possible values are 1 (visitor-level), 2 (session-level), or 3 (page-level). When left undefined, the custom variable scope defaults to page-level interaction.

    gaAware.track_custom(1, 'Example Var', 'Track', 1);

Social Media Tracking
---------------------

http://code.google.com/apis/analytics/docs/tracking/gaTrackingSocial.html

NOTE: By default social media tracking is disabled because it calls in 3rd party js but enabling in the init code will automate social tracking.  Also, Google+ tracks with default Analytics code

### Params ###

- network (required) String Representing the social network being tracked (e.g. Facebook, Twitter, LinkedIn)
- socialAction (required) String Representing the social action being tracked (e.g. Like, Share, Tweet)
- opt_target (optional) String Representing the URL (or resource) which receives the action. For example, if a user clicks the Like button on a page on a site, the the opt_target might be set to the title of the page, or an ID used to identify the page in a content management system. In many cases, the page you Like is the same page you are on. So if this parameter is undefined, or omitted, the tracking code defaults to using document.location.href.
- opt_pagePath (optional) String Representing the page by path (including parameters) from which the action occurred. For example, if you click a Like button on http://code.google.com/apis/analytics/docs/index.html, then opt_pagePath should be set to /apis/analytics/docs/index.html. Almost always, the path of the page is the source of the social action. So if this parameter is undefined or omitted, the tracking code defaults to using location.pathname plus location.search. You generally only need to set this if you are tracking virtual pageviews by modifying the optional page path parameter with the Google Analytics _trackPageview method.

    gaAware.track_social('facebook', 'like');

E-commerce transaction
----------------------

### Params ###

- order ID - required
- affiliation or store name
- total - required
- tax
- shipping
- city
- state or province
- country

    gaAware.track_transaction(
    	[
    		// Item 1
    		['001', '90', 'Product 1', 'Category 1', '32.95', '1'],
    		// Item 2
    		['001', '92', 'Product 2', 'Category 1', '17.04', '1']
    	],
    	// Transaction
    	['001', 'Example Store', '49.99', '2.30', '5.00', 'Minneapolis', 'Minnesota', 'USA']
    );

Change Log
==========

V1.0
- one account (this.s.UA = '')
- multiple accounts (this.s.UA = [])
- cross domain (this.s.domains.length > 1)
- cross domain links that open in a new window
- cross domain across non combined root level domains (no objects in this.s.domains)
- cross domain across non combined sub domains (no objects in this.s.domains)
- cross domain across combined sub domains (objects in this.s.domains using {entity:'',subs:['']})
	- this resets to domain property to the entity level and doesn't need to pass longer url on links and forms
- setting to set subdomains to track as their parent
- cross domain on multiple accounts
- All cross domain and multiple accounts work in same or new window
- virtual page views for:
	- external links (this.s.track_external_links)
	- assets (this.s.asset_extentions)
	- assets track in AMM preview
- All virtual page views in same window or new or frame name
- All virtual page views work for multiple accounts
- ecommerce (also for multiple accounts)
- forms across multiple domains in same window
- forms across multiple domains in new window / iframe
- forms across multiple domains with multiple accounts same / new window / frame
- In all domain matching ://www. = :// (this.s.domains, links, forms)
- Track event to all active ua accounts through method

V1.1
- Only track specific domains
- Exclude sub domains from tracking

V1.2
- Track virtual page views for # links
- Disable cross domain tracking if this.o.domains is defined but does not contain the current domain
- Bug fix: links with nested html tags caused an error because of event bubbling

V1.3
- Special characters no longer through an error
- Don't track links with href starting with "javascript:"

V1.4
- Adding prefix settings for vpv, asset, hash links, external forms not in cross domain
- Adding asset extensions xlsx, js, css, eot, svg, ttf, woff, otf
- Debug Mode setting for alerting on same page views
- Malto link tracking, extendable for future protocols
- External form submissions not in cross domain definition as vpv

V1.5
- Fixed Bug in get form cross domain tracking
- Fixed bug recognizing asset links in the amm in preview
- Matching cross domains with ?'s and #'s

V1.6
- Added this.s.no_track_class
- Imporved matching for disabled cross domain matching for absolute urls to the same domain
- Hash links don't need to offset default action, interferes with js events

V1.7
- Adding Custom Variable Tracking
- Adding Social tracking method and auto configuration

V1.8
- Fixing social tracking, updated events listened to
- 1.8.1 fixed same domain matching with cross domain turned on
- 1.8.2 asset absolute links to same domain track as asset not external
- 1.8.3 added _setAllowAnchor for GET method forms in cross domain
  - http://code.google.com/apis/analytics/docs/gaJS/gaJSApiCampaignTracking.html
  - https://groups.google.com/a/googleproductforums.com/forum/#!category-topic/analytics/asynchronous-tracking-code-snippet/w_8VlVFjKMM
- 1.8.4 Prefer events to virtual page views
- 1.8.5 link_virtual was double tracking events when multiple codes were active
- 1.8.6 update to work of jQuery.deligate for ajax inserted links
  - Optimized performance to evaluate link on clink instead of all at page load
  - Don't need to wait for $(document).ready
  - links and forms starting with "//" for the href or action
  - Prefer link title tags to href as a setting
  - Adjusted automated event categories

TODO
====

- *. for subdomains
- Cross Domain for Sub directories
- Cross domain for iframes
- Chrome cancels alt protocols in network tab, appears to still track though
  ? wtai://wp/mc;18885551234
  ? wtai://wp/ap;18885551234
  - http://stackoverflow.com/questions/4169976/whats-a-reliable-method-for-setting-up-telephone-links-for-mobile-browsers

References
==========

- Cross Domain: http://code.google.com/apis/analytics/docs/tracking/gaTrackingSite.html
- Commerce: http://www.google.com/support/forum/p/Google%20Analytics/thread?tid=5003ffffedf3ee6c&hl=en
- Commerce: http://code.google.com/apis/analytics/docs/tracking/gaTrackingEcommerce.html
- Cookies: http://code.google.com/apis/analytics/docs/concepts/gaConceptsCookies.html
- Events: http://code.google.com/apis/analytics/docs/tracking/eventTrackerGuide.html