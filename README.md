# Craft – Admin Bar
Simple front-end shortcut bar for users logged into [Craft CMS](https://buildwithcraft.com).

![Screenshot](screenshot-bar.png)

## Installation
1. Upload the adminbar/ folder to your craft/plugins/ folder.
2. Enable the plugin in the CP.
3. Either add the Admin Bar through the plugin settings page, or add the tag, `{{ craft.Adminbar.show(entry) }}`, to your template.

## Auto Embed
Using the "Auto Embed" setting will add the Admin Bar to the top of your `<body>` tag. Doing it this way will base the "Edit" button off of the current page entry. Branding colors will use the color selected through the "Default Color" color picker.

For more control, or to add the admin bar to multiple entries, use the Twig embed tag, below.

## Embed Options
Format: `{{ craft.Adminbar.show(currentEntry, color, type) }}`

Embedding the Admin Bar using this tag will let you overwrite the settings found on the plugin settings page.

* **currentEntry** *`entry`*  – Current entry passed in as a TWIG object.
* **color** *`'#d85b4b'`* – The color used for rollovers or highlights. You can change this to better fit the branding of your website. Use any CSS color format.
* **type** *`'bar'`* – Changes the style of the Admin Bar. For now, the only options are `bar` or `none`.
  * `bar` – Creates a black bar that spans 100% the width of the element that it is placed in. It's *slightly* responsive.
  * `none` – Has the same markup as `bar`, but removes all of the CSS, so you may style it however you'd like.

## Adding Links Through Plugins
Links can be added through the `addAdminBarLinks()` method in your main plugin class. Return an array for each link you'd like to add.

```php
public function addAdminBarLinks() {
  return array(
    // an example of a simple url link
    array(
      'title' => 'Craft',
      'url' => 'http://buildwithcraft.com',
      'type' => 'url',
    ),
    // an example of a CP link
    array(
      'title' => 'Entries',
      'url' => 'entries',
      'type' => 'cpUrl',
    ),
    // an example of a url link that passes along some extras
    array(
      'title' => 'Blog',
      'url' => 'blog',
      'type' => 'url',
      'params' => 'foo=1&bar=2',
      'protocol' => 'http',
      'mustShowScriptName' => true,
      'permissions' => array('myPluginPermission', 'thisIsRequiredToo'),
    ),
  );
}
```

* **title** *required*  – The label that will appear for this link in the Admin Bar.
* **url** *required* – The url or path used for the link.
* **type** *required* – The context of the url or path.
  * `url` – Used for relative or absolute URLs.
  * `cpUrl` – Prepends `cpTrigger` to the **url** value for links found within the Control Panel. For example, if you wanted to link to Craft's default Entries page, set **url** to `'entries'` and **type** to `'cpUrl'`. The final url will be `http://example.com/admin/entries`
* **params** – Passes along url parameters, as [documented here](http://buildwithcraft.com/docs/templating/functions#url).
* **protocol** – Changes the url protocol, as [documented here](http://buildwithcraft.com/docs/templating/functions#url). This only supports this string format: `'foo=1&bar=2'`
* **mustShowScriptName** – Appends `index.php`, as [documented here](http://buildwithcraft.com/docs/templating/functions#url).
* **permissions** – An array of required permissions that are needed for this link to be displayed. All permissions in this array will be required.

*Please note: links in the Admin Bar are updated when the user saves the Admin Bar plugin settings. While you can use PHP to determine the argument values and which URLs appear based on your plugin's settings, the links will not update until the user goes back and updates their Admin Bar settings.*

### Plugins using Admin Bar
* [Craft Help](https://github.com/70kft/craft-help)

![Screenshot](screenshot-settings.png)

---

## To Do
* ~~Add options to CP.~~
* Add a new type to be used within multiple entries. [Looking for some typical use case suggestions.](https://github.com/wbrowar/craft-admin-bar/issues/new)
* ~~Automatically add the bar to the top of the `<body>` tag.~~
* ~~Add custom hook for other plugins to add custom links.~~ (turns out it used an Event)
* ~~Add a way to toggle links from other plugins in the CP if users don't want to use them.~~

---

## Releases
##### *1.3.4*
* Added permissions to `addAdminBarLinks()` for plugin authors.
* Added `url()` functions accross the board.

##### *1.3.3*
* Plugin authors can pass along [url() arguments](http://buildwithcraft.com/docs/templating/functions#url) into `addAdminBarLinks()`.
* Fixed some minor CSS issues in the front-end admin bar.

##### *1.3.2*
* @ktbartholomew helped me realize a much simpler way to integrate plugins links. **NOTE: the event has been removed, and this hook is taking its place.**
* Added plugin names on Admin Bar settings page.

##### *1.3.1*
* Fixed a couple of PHP 5.3 errors

##### *1.3.0*
* Added the logged in user photo. Just for kicks. :bust_in_silhouette:
* Added a new "Dashboard" link.
* Added `baseCpUrl` in front of root-relative links.
* Added support for plugins to add links to the Admin Bar.
* Added setting to enable/disable links added by plugins.
* Added example plugin for plugin makers.

##### *1.2.2*
* Changed the hard-coded url for the "Settings" link to use Craft's cpTrigger variable (thanks to @mildlygeeky for the tip).

##### *1.2.1*
* Fixed an error when checking for user permissions (flagged an error in devMode).

##### *1.2.0*
**NOTE:** Craft will ask you to make a one-time database update to accommodate the new options. 
* Added option to make custom links available only to users with the admin role.
* Added option to embed Admin Bar from the plugin settings page.
* Added color picker for default branding color.
* Removed duplicate CSS and JS code for multiple instances of the Admin Bar.

##### *1.1.0*
* Added ability to add custom links to CP settings.

##### *1.0*
* Basic admin bar with Edit, Settings, and Logout buttons.

Please, let me know if this plugin is useful or if you have any suggestions or issues. [@wbrowar](https://twitter.com/wbrowar)