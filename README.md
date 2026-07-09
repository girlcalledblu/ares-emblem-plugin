## Overview
This plugin allows you to add custom styling to character pages and icons based on demographics, groups, or a custom field. This plugin requires some comfort with revising the `custon_char_fields.rb` file, as well as the handlebar (hbs) files in the ares-webportal directory. I've tried to give very thorough walkthrough here, but if you need help, you can find me on the Discord server!

### Plugin Install

First, install the plugin.

In the game, run plugin/install <github url>.

### Configuration: Aresmush Directory

In `aresmush/plugins/profile/custom_char_fields.rb`, you will need to add the following line to `def self.get_fields_for_viewing(char, viewer)`:

`emblem: (Character.derive_emblem(char) || "").gsub(" ","-")`

If you have no other fields in this file, it should look like this:

```      def self.get_fields_for_viewing(char, viewer)
        return {
          emblem: (Character.derive_emblem(char) || "").gsub(" ","-")
        }
      end
```
