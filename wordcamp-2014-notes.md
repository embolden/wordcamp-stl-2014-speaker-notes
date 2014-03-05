#WordCamp St. Louis 2014 Speaker Notes
--------------------------------------------------------------------------------

##Mike Hanson - Bluehost Outreach Team

###How WordPress stores data in the wp_options table
Option Name - 64 characters
Option value - longtext (4gigs)

Storing long term data - Defaults, configuration, version, install dates

###Updating options
update_option( 'key', 'value' );
get_option( 'key' ); Return value or false

Storing short term data - User entered values, API requests, cart items, anything with an expiration

Time in WP - Availabile constants.  Easier than doing the math ( 60 * 60 * 24 for a week ).
MINUTE_IN_SECONDS, HOUR_IN_SECONDS, DAY_IN_SECONDS, WEEK_IN_SECONDS, YEAR_IN_SECONDS

###Storing transients ( short term data )
set_transient( 'transient_key', 'value', DAY_IN_SECONDS );
get_transient( 'transient_key' ); Return value or false
Transients save two keys - one with the expiriation, one with data

###Storing theme specific settings
set_theme_mod( 'theme_key', 'value' );
get_theme_mod( 'theme_key' ); Return value or false
Key in database - theme_mods_{$themeslug} - serialized array

###Serialized data
set_option( 'key', $array );
$format = get_option( 'key' );
$format['value'];

Helper functions - Setters & Getters. Good for larger (more than 1 file/300ish lines) plugins.

###Organizing transients
No update_ function, must set_ like it is the first time, must set expiration

###Force transient removal
Options - Remove it on admin login or some other hook, wp_cron to remove it every DAY_IN_SECONDS

###Advanced usage in multisite
Set/get functions for transients and options have a multisite equivilent
Prefix for options is different

###When not to use these
When the info is specific to a user or post

###Clean up on Uninstall
delete_option( 'key' );

###Slide deck
https://speakerdeck.com/mikehansenme/wordpress-junk-drawer-and-how-to-organize-it

--------------------------------------------------------------------------------

##Myke Bates - Powerful Deployment Techniques

###Downfall to FTP
potentially dangerous, no versioning, no automation

###Choices
- Capistrano - Ruby based, v3 broke v2, poor documentation, very powerful
- DeployHQ http://deployhq.com/
- Deploydo
- CI (Continuous Integration) - Trapus, CCNET
- Migrate DB Pro - Handles DB migration. https://deliciousbrains.com/wp-migrate-db-pro/
- Beanstalk

###Rocketeer - http://rocketeer.autopergamene.eu/
PHP Based deployment
####Getting Started
- Comfortable in terminal
- Basic Linux-fu
- VPS Server (or root level access. no cpanel or whm)
- Basic git commands

(Mike proceeds to run through the examples and blow everyone's mind)

###Slide deck
http://www.slideshare.net/mykebates/wcstl-2014-powerful-deployments

--------------------------------------------------------------------------------

##Joshua Ray - Adding Automation to your theme development workflow

###Slidedeck
https://t.co/pveP1sFBPJ

###YeoPress
https://github.com/wesleytodd/YeoPress - Requires node.js and Yeoman

`yo wordpress`

Asks for:
- WordPress url
- table prefix
- DB information(host, name, user, password)
- use Git?
- Submodule?
- Custom directory structure
- Install a custom theme?

###Grunt
(Gulp is an alternative)

`npm install -g grunt-cli`
(-g is global flag)

Josh's boilerplate: https://github.com/Ollo/boiler
Josh's boilerplate's gruntfile: https://github.com/Ollo/boiler/blob/master/Gruntfile.js

`grunt taskname` will run task from grunt file and `watch` will continue to poll

###Git
(Everyone uses git)

####Webhooks
http://developer.github.com/webhooks/

####Dandelion
https://github.com/scttnlsn/dandelion
Incremental git repo Deployment
`gem install dandelion`
Uses a yaml config, can work with S3, has own exclude (regardless of git)
`dandelion deploy`
Checks against remote before deploying.

###QA
####Working with a team?
- Root relative URLs (doesn't work with mutlisite)
- Shared database that everyone connects to

--------------------------------------------------------------------------------

##Carrie Dills - WordCamp St. Louis 2014 - A Shared Experience

Everyone has knowledge to share

Sharing knowledge grows business
2011 -  8% online leads
2012 - 27% online leads
2013 - 43% online leads

Share your network - 'I Happen to know a guy'

Get over yourself - Look for the value in others and don't worry about the return

Listen - Invest - Look - Introduce - Follow Up

Networks create opportunities

Crowdtitlt, Happiness Bar, Time, Kiva

--------------------------------------------------------------------------------

##Adam Silverstein - Revising WordPress Revisions

###Slide deck
https://speakerdeck.com/adamsilverstein/revising-wordpress-revisions

- Revisions are awesome!
- Rewriting a feature is hard
- Code (backbone and php) is spiffy!
- Ongoing improvements

Revisions started in WordPress 2.5

###Use Cases
- Single user, makes mistake, goes back to find recent version
- Two users, corretions and feedback
- Multiple users - complex editorial workflow

###Mockups
- Balsamiq mockups - http://balsamiq.com/products/mockups/

###Bugs & Enhancements
- Addressed longstanding bugs
- #16215 - Displayed incorrect author.  Fixed and updated old data
- #9843 - Duplicate autosave/revision.
- Currently working on post-meta revisions
- Final version used same diff code with a different style

Added 'Revisions' to the post meta box.  Shows number of revisions and alerts people that revisions are available.

New default behavior shows diff between two (word for next to each other) revisions.  Option availble to select two arbitrary revisions and compare.

Had to remove Matrix easter egg. :(

###Code
- JS interface leveraged Backbone & Underscores
- PHP serves data
- List of revisions with meta, initial selected revision diff
- Mark Jaquith wrote JS to halve number of requests if previous request fails

###Backbone Code
- Backbone.Model - base model is a single diff
- Backbone.Collection - Diff in a collection of all diffs. Handles load/syncing individual diffs
- Backbone.Views - Controls, tickmarks, metabox, tooltip, checkbox, buttons, slider, diff
- Backbone.Router - Keeps url consistant (Backbone dropped support for query string during dev)
- Templating library adapted from media library, generalized into wp.template

###Tickmarks
- Using jQuery UI slider.  Does not support tick marks by default
- Browser rounding made the math interesting. (Some round up, some round down)
- RTL languages needed special consideration.  Slider is flipped.

###Re-writing a feature is hard
- Intense testing of single handle and two handle mode
- Accessibility review - colors, keyboard only, screen readers
- Browser testing - Compatibility to IE7
- Multiple users, autosaves, and restoring a revision
- Testing with several hundred revisions (original version was slow with lots of revisions)
- Testing RTL mode
- Original tickets had over 140 comments in 3 months

###Extending
- Revisioning post meta #24908
- Revisions Plus
- Code Revisions (Google summer of code project) http://wordpress.org/plugins/code-revisions/

###QA
####Can people comment on revisions because they're a post object?
Not currently.  Possibly available in 'Edit Flow' https://wordpress.org/plugins/edit-flow/

####How long did it take?
6 months

####Why does the ajax call return static HTML instead of parsing on client side?
Reusing code that was already inside of WordPress.
Diff engine was not the main focus of the project, but the usability of the revision system.

####Did you run into limitations with WP localize script?
Yes!  Lead to a patch to fix booleans and integers.
Might be useful to create a new function to prepare variables.

####What fallbacks are in place for users without JS?
Because of the small use case, the feature is disabled for users without JS

--------------------------------------------------------------------------------

##Developer Round Table
- Sam Hotchkiss
- Konstantine - Works for Automattic theme team
- Rachel Baker - 10up developer. WordPress API team.  WordPress/BuddyPress contrib
- Michael - Automattic designer. Work on Core, VaultPress, .Com sites

###JSON Api - When will this be part of core? What to do in the meantime
Not going to be in 3.9.  Trying for 4.0.  Currently usable but has bugs.
.9 will have the oAuth authentication.  Currently sends info in plaintext
Rachel is a contributor

###What is the best thing about WordPress? Biggest arugment against?
Downfall - Something DB heavy and display/user light.
Great for being able to dev a site from scratch in a short amount of time.
RoR stole some thunder from PHP and people are coming back (Laravel).
Use of PHP makes it so popular (php powers 80% of web).
Content entry is the best feature.

###What feature would you take from another platform?
-Routing
-MVC

###No API for import/export widget?
Sounds like a great plugin.
https://wordpress.org/plugins/widget-settings-importexport/

###Media in WordPress.  Is there a good folder/maintenance system?
Used a plugin called media-manager

###Two Factor Authentication - Coming to .org?
Probably - yes.
DuoSecurity - https://www.duosecurity.com/ https://wordpress.org/plugins/duo-wordpress/

###Should I create my own tables or use built in?
Using APIs are considered to be more future proof.  Moving custom can be a challenge.
WordPress.com has 50-60 million sites with minimal extra tables.
Not never, but rarely.

###Where do I start with getting started with PHP development?
The Codex.  Moving to help.wordpress.com
Learn PHP.
Read through the files in the wp-includes folder.  Look at source code and functions

###Getting away from tons of theme options?
Decisions over options.  Use sane defaults.
Use Theme Customizer. Otto Wood's posts are a great starting point
Konstantine's opinion: If option does not change the front end of theme - should not be a theme option
Use WordPress default conventions for building admin pages
TwentyFourteen uses the theme customizer
Settings API for non-theme options

###Is there a standardization for translating theme customizer tools from one theme to another?
No.  Data is stored as theme mods - which are theme specific.  Would have to use settings instead.

###What is the different between DiggDigg and JetPack?
DiggDigg handles social media sharing.
JetPack is a bundle of features that are WordPress.com features.  Has a social sharing module.

###Any plans to separate JetPack parts?
From a user standpoint it is a good idea, but the relation is that everything in Jetpack is from .Com
Try Photon - Serves retina images, routes images through WordPress CDN
Jetpack freenode channel

###If you had 1 default that you wish could be changed, what would it be?
- URL structure
- Pings/trackbacks
- Random table prefix
- TinyMCE kitchen sink (3.9 has new TinyMCE)

###What are some features in 3.9 and 4.0 that you're looking forward to?
- WordPress JSON API http://wordpress.org/plugins/json-api/
- Front end editing https://wordpress.org/plugins/wp-front-end-editor/
- Widget Customizer http://wordpress.org/plugins/widget-customizer/

###What is your favorite 3rd Party Plugin?
- Advanced Custom Fields
- Gravity Forms

###Handling moving sites
- Search Replace DB
- Set URL in wp_config
- Root relative URLs plugin http://wordpress.org/plugins/root-relative-urls/

Missed some questions while showing ACF to Randy

###Goto resource for blog or book about WP?
- Anything Nacin or Otto writes
- Core trac mailing list
- Non WP - CSStricks, MDN, Paul Irish
- WP Hackers mailinglist

--------------------------------------------------------------------------------

##Rachel Baker - Code with Care

###Speaker Deck
https://speakerdeck.com/rachelbaker/code-with-care-write-secure-themes-and-plugins

###Code
https://github.com/rachelbaker/wcstl-demo

From 2011 to 2013 went from knowing nothing about security to it being her job.
Job is to audit 3rd party code for enterprise organizations

###Types of attacks
- XSS - Cross site scripting.
- CSRF - Cross site request forgery	

1. Filter input
2. Escape outpt
3. Verify data source
4. Profit!

###Filter Input
- Don't trust data from any source
- Understand the context
- Validate expectation, never assume
- Correct formatting issues
- Process inputfilters before saving to db

`sanitize_text_field`
Strips tags, whitespace, bad utf characters, remove linebreaks

`sanitize_email`
Has is_email function also. (Not RFC compliant)
https://codex.wordpress.org/Function_Reference/is_email

`wp_filter_kses`
KSES Strips Evil Scripts

`wp_filter_post_kses`
Used for non-admins when saving post content

`wp_strip_all_tags`
Remove all markup

`esc_url_raw`
Makes data safe to save in DB

###Escape Output
- Distrust data from any source
- Understand the context of the data being displayed
- Correct formatting issues
- Encode for display

`esc_attr`
Used to escape text string

`esc_html`
Used to escape HTML output

`wptexturize`
`convert_chars`
`wpautop`
Automattically puts `<p>` tags around text

`esc_url`
Output proper link

Escape values on the frontend or backend - anytime you're displaying

###Verify Data Source
- Create a nonce field or value anytime data will be processed from a form, AJAX request, or URL
- Check the referring source of a processing request
- Confrom the presence and validity of a nonce before processing data

`wp_nonce_field`
Displays a hidden field with the nonce value

`check_admin_referrer`
Check validity of current nonce

###QA
####How can we handle a client's proprietary embed codes?
Add custom oEmbed - https://codex.wordpress.org/Function_Reference/wp_oembed_add_provider

####How do you recommend using a nonce with a 3rd party API with ajax?
When doing AJAX in WP - use a nonce.
If the data is not coming from WP itself, nonce is unavailable.
`wp_remote_post` can be used to send data to app

--------------------------------------------------------------------------------

##Doug Stewart - Making Your Whole Life Easier with WP-CLI

###Slide deck
https://speakerdeck.com/zamoose/making-your-whole-star-life-easier-with-wp-cli

###WP-CLI - WordPress Command Line Interface
Often preferred by advanced users as they provide a concise way to interact

###Install WordPress
`wp core download`
Will cache for subsequent installs.  Can pass --path or run in folder you want

###Install Multisite
`wp core multisite install`
`wp site create`
Handle multi-site installation
Bug - htaccess doesn't get written

###Create User
`wp user generate`
Generates 100 dummy users

###Create dummy content
`curl http://loripsum.net/api/5 | wp post generate --post_content --count=100`

`wp scaffold`
- plugin - setup plugin name and readme
- childtheme - create child theme referencing parent
- _s - clean copy of _s
- CPT - code you need for CPT
- custom taxonomy - code you need for taxonomy

`wp search-replace`
Will go through serialized arrays but not objects by default

`wp db`
Has access to wp_config.  optimize, repair, import, export, CLI

`wp theme-test`
Pull theme unit test

`wp export` / `wp import`
Gets full XML import/export

##Where do I get it?
http://wp-cli.org

###Support
####Lots of hosts have it pre-installed
- Amimoto
- Bluehost
- Dreamhost
- Media Temple
- NearlyFreeSpeech.Net
- SiteGround
- Synthesis
- TVC.Net
- WordPress.com VIP

No WordPress Engine (also no SSH)

####Various WP Environments
- Varying Vagrant Vagrants (VVV)
- vip-quickstart

###Topics for Advanced Study
- Puppet WP - Wrapper for CLI
- Saltstack-WP - Wrapper for CLI
- Extening WP-CLI with your own commands
	- Backupbuddy and other backup plugins create their own CLI command
	- Can run with server cron
- Package your own commands
- Community packages

Will read doc-blocks for your custom commands. Incentive to document

###QA
####Will this save time if you only have 1 site?
Yes!  Lots of useful plugin and user functions.

####Could you shell script a default setup?
Will read a yaml config and do defaults

####Tips for working with plugins not on .org repo
You can specify file paths.  HTTP methods might be blocked. (fopen)