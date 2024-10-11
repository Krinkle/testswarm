## 1.1.0 / 2018-06-04

Complete list of issues solved in the 1.1.0 milestone:

* <https://github.com/jquery/testswarm/milestone/2?closed=1>

### Added

* api: Add human-friendly help page with overview of available actions. ([ed523a6](https://github.com/jquery/testswarm/commit/ed523a6494663056154ea871a4533cebd296d6ce))
  Example at <http://swarm.jquery.org/api.php>.
* Page: Implement partial responses via a new `X-Swarm-Partial` header. ([a569c99](https://github.com/jquery/testswarm/commit/a569c99a19665682fb94781073c1d12798b14d48))
* Inject: Add support for Jasmine. ([#167](https://github.com/jquery/testswarm/issues/167))
* JobPage: Add pagination for previous/next job in same project. ([#65](https://github.com/jquery/testswarm/issues/65))
* scripts: New `purge.php` script – purge old test results from the database. ([2785458](https://github.com/jquery/testswarm/commit/2785458af4c86b9a8010c60b52bf714c4d1a5dd5))
* scripts: New `abortPendingRuns.php` script - help reset pending runs and re-runs that
  belong to expired jobs (e.g. no longer available on the build server). ([08e2ea6](https://github.com/jquery/testswarm/commit/08e2ea618bb03c053d5c56ce4cd899f40aa5283c))
* scripts: Add `--delete` option to `manageProject.php` script. ([31191f8](https://github.com/jquery/testswarm/commit/31191f87fbfaba2c839afc455aa4c4b01d53d122))
* scripts: Add `--password` option to `manageProject.php` script. ([557663f](https://github.com/jquery/testswarm/commit/557663f3b2463310bac4b8b764a735dd62bf0834))

### Improved

* Database: Convert to `php-mysqli` lib, from deprecated `php-mysql`. ([d4b79ed](https://github.com/jquery/testswarm/commit/d4b79edcecd1decdebf1dcdad8b3247e2d55834c), [32f37de](https://github.com/jquery/testswarm/commit/32f37de8a8d9e4bc3f2417cf0fcc3e45547edb9c))
* JobPage: Make result updates more efficient. ([#215](https://github.com/jquery/testswarm/issues/215))
* RunPage: Show browser info and raw user-agent as part of "Browser not needed" error. ([29a895a](https://github.com/jquery/testswarm/commit/29a895adc77cde68e4eb5e24ed9b7be0fc5269dc), [438da1a](https://github.com/jquery/testswarm/commit/438da1a8885bd3826c4ca26543da6881acaf13f4))
* Update jQuery from 1.7.2 to 1.12.4. ([43a2374](https://github.com/jquery/testswarm/commit/43a2374bacca976bb3915d89d5ab3b8da0a4ff2b))

### Fixed

* BrowserInfo: The Android icon looks weird. ([#306](https://github.com/jquery/testswarm/issues/306))
* Inject: Fix QUnit 2.0 support. ([#313](https://github.com/jquery/testswarm/issues/313))
* JobPage: Fix alignment of loading indicator. ([3f716ca](https://github.com/jquery/testswarm/commit/3f716caf01ec83e9a2194f1bd0f6b5d4d7e66674))
* api: Catch invalid format error. ([18f067a](https://github.com/jquery/testswarm/commit/18f067a02e69b8f9935f443437fe15d5027eaaef))
* scripts: Fix stty "invalid argument" warning when entering secrets. ([67fcb7d](https://github.com/jquery/testswarm/commit/67fcb7dcc07128c268f6272fb60ea6ad6b061370))
* Page: Fix multiple active pages bug when one project's name is a subset of another.
  ([2fb1458](https://github.com/jquery/testswarm/commit/2fb1458fed8e37078bfb830929225f7e905d84be))
* prettyDate: Use singular instead of plural for "1 week".
  ([a2bd083](https://github.com/jquery/testswarm/commit/a2bd083684d2dcd7ac222e9047c324bc6cf0ac3d))

## 1.0.0 / 2015-10-18

This major release features a complete rewrite of the frontend and backend code base, and
adds several major features such as a brand new API, automatically synchronized runclient
settings and a new interface powered by Twitter Bootstrap.

TestSwarm has also incorporated several optimizations for a continous integration workflow
where TestSwarm is used with Jenkins and BrowserStack (or similar services) by providing
detailed information about the swarm's state through `api.php?action=swarmstate`. This new API
action lists all supported browsers including how many clients are online for each user agent
and if and how many pending runs there are. This makes it easy to then use this information to
automatically start or terminate browsers as needed with, for example, the [BrowserStack
API](https://github.com/browserstack/api),
[node-browserstack](https://github.com/scottgonzalez/node-browserstack) and [testswarm-browserstack](https://github.com/clarkbox/testswarm-browserstack).

The main breaking change is in the database schema. The `users` table has been replaced with a
cleaner `projects` table. The passwordless run-time clients are no longer stored in the same table.
Any remaining meta data (freeform client name) is now stored in the `clients` table. More about
this change is detailed at [issue #148](https://github.com/jquery/testswarm/issues/148#issue-4018312).
You are recommended to start with a fresh install. However, user credentials may be manually migrated
by taking note of the authentication and password hash (in the `users.auth` column) and re-inserting
it into the projects database after installing TestSwarm 1.0.

Complete list of issues solved in the 1.0.0 milestone:

* <https://github.com/jquery/testswarm/milestone/1?closed=1>

### Configuration changes
* PHP version requirement raised to 5.4.0.
* Settings are now loaded from `/config/localSettings.json` (instead of `/testswarm.ini`).
* The default `cooldownSleep` setting has changed from 15 seconds to 0 seconds.
* The default `nonewrunsSleep` setting has changed from 30 seconds to 10 seconds.
* The database schema has been re-constructed from the ground up. The schema is
  non-upgradable and will have to be re-created. Use the update.php or install.php script
  to quickly re-create your database. All data will be lost when upgrading from < 1.0.0!

### New features

* Refactor front-end skin, powered by Twitter Bootstrap. (#82, #145)
* Refactor backend with no globals into OOP with Context. (#127)
* Refactor state/content/logic scripts into Page/Action classes. (#132)
* Refactor the user agents backend storage and client detection. (#143)
* Implement RESTful JSON API, including various new actions (#115, #105, #116)
* Implement browser client settings framework. (#107)
* Implement client Ping system. Used to keep track of which clients are online, and
  which may have lost network connection, or crashed, etc. Also used to keep the
  client-side configuration up-to-date to allow long-lived runner clients that
  without having them continue with old run-time configuration values.
* Implement basic caching for expensive operations, using flat files in `/cache/`.
* Implement Debug mode for API responses (`format=debug`).
* Implement Debug mode for Database queries.
  Disabled by default for performance and security reasons,
  enable by setting `debug.dbLogQueries` to `true` in the settings file.
* Implement REPL for TestSwarm PHP environment as maintenance script.
* Add "swarmstate" API for getting the current status of the swarm. (#139)
* Add "info" API to get current TestSwarm version and configuration,
  also exposes as `<meta name="generator">` on HTML pages. (#150)
* Add "projects" API for listing all accounts that can submit jobs. (#149)
* Page: Add support for setting a custom robots policy. (#146)
* Page: Set noindex policy on non-standard urls and internal results. (#146)
* Page: Implement simple error pages with status 500 for early fatal exits.
* Page: Individual project pages are now accessible directly from the main
  navigation menu (in a new Projects dropdown menu).
* Create "Clients" page, showing all currently connected clients. (#72)
* Create "Info" page, shows current version info with links to GitHub.
* Create "Result" page, linked to from Job pages, showing the run results including
  navigation links for the run and meta data about the client and the run.
  (Replaces the old "Runresults" page.)
* Config: Use JSON for configuration files. (#172)
* Job: Add button for automatically resetting all failed jobs. (#222)
* Job: Add a confirmation step to the interface for Reset job/Delete job. (#199)
* Inject: Add support for Mocha. (#216)
* Add maintenance script for installling the database. (#158)
* Add maintenance script for updating an old database schema. (#142)
* Add maintenance script for quering the browserSets configuration.
* Add maintenance script for migrating user agent IDs.
* Add maintennace script for resetting project tokens. (#281)
* Security: Protect against clickjacking attacks. Pages now send proper
  X-Frame-Options headers. (#207)
* Security: Fix CRSR vulnerabilities. The API and GUI no longer perform
  actions on behalf of a user based on cookies/sessions. All actions now require
  tokens. This is handled transparently between GUI and API. Third party users
  of the API may have to update their code to send the authToken for requests
  that previously didn't require it. (#209)
* Security: Allow settings file to be stored outside docroot.
  Though settings are still loaded from `config/localSettings.json` by default,
  there is now a small PHP-file serving as intermediary which can be modified
  to point to somewhere else instead. As being a PHP file, its source won't be
  shown, even if the webserver's Deny rules for /config are misconfigured. (#232)

### Bugs fixed

* (#99) state=getrun should protect against broken/incomplete `runs` database entries.
* (#122) state=job inaccessible as GET view for HTML form.
* (#1) Server/client timezone difference should be accounted for.
* (#92) Tinder should show current browsers (regression in 0.2.0).
* (#95) Browsers are wrongly ordered (i.e. IE10 before IE6 instead of after IE9).
* (#118) Clean up username mess.
* (#18) Detect database failure early and abort with friendly notice.
* (#110) User agent matching should prefer start instead of end.
* (#19) User signup doesnt check for erroneous return result, users not created.
* AddjobPage has been fixed and can now be used again from the browser.
* Fixed bug in Signup where it would initiate a session for a username, even if the INSERT
  query for the users table failed and the username remains unregistered.
* (#162) JobPage with no "new" runs but some "in progress" should still ajax refresh.
* (#166) Apply natsort for user agents in the User API.
* (#182) JobPage AJAX needs error handler to fix infinite "Updating..".
* (#189) Shouldn't distribute runs that are being run already.
* job.js should keep refreshing even when everything is complete and "reset" happens.
* (#191) Preserve other window.onerror handlers (if there are any).
* (#210) When not logged in, dblclick for Wiperun on job pages should not make an
  API request, as it would just respond with "Not authorized".
* (#78) Replace generic "Chrome" user agent ID with regular versioned ones. Google Chrome
  versions are now testable like any other browser.
* (#254) Don't strip trailing slash from the root path under NGINX.
* Avoid stale and incompatible responses for static assets from a CDN by adding
  content hashes to urls for static files.

### Other changes

* Update jQuery from 1.3.2 to 1.7.2. (#121)
* Link to ScoresPage in the site menu. (#70)
* Drop redundant HTML5 attributes.
* Prefer HTTPS for urls to third-party domains that support both HTTP and HTTPS.
* Add `lang="en" dir="ltr"` to `<html>`.
* Remove old Perl example files in `/scripts/ that were no longer used or maintained. (#141)
* Use [ua-parser](https://github.com/tobie/ua-parser) as user agent parser. (#187)
* Default settings are now stored in `/config/settings-default.json` (instead of
  hardcoded in `init.php`).
* The HomePage now includes all information from the Swarmstate API (including the number
  of pending jobs for each user agent).
* Improve error handling for when database connetion can't be established.
* Expose runs/max values in the Job API. (#165)
* Conform with jQuery JavaScript Style Guide. (#289)
* Adopt updated browser logos from <https://github.com/alrra/browser-logos>,
  including new icons for Android, iOS, Windows 10, Edge, Yandex, and more.
* How jQuery Foundation and Wikimedia Foundation use TestSwarm with Jenkins and BrowserStack: [Automated Distributed Continuous Integration for JavaScript](./docs/Automation.md)

## 0.2.0 / 2012-03-07

### User agents

* (#97) Added Firefox 8
* (#97) Added Firefox 9
* (#101) Added Firefox 10 Beta
* (#104) Update Firefox 10.
* (#97) Added Preso 2.10
* (#102) Added iOS 5 / Mobile Safari.


## 0.1.0 / 2011-11-22

* (#94) First tagged version.
