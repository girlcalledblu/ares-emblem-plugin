# Overview
This plugin allows you to add custom styling to character pages and icons based on demographics, groups, or a custom field. This plugin requires some comfort with revising the `custon_char_fields.rb` file, as well as the handlebar (hbs) files in the ares-webportal directory. I've tried to give very thorough walkthrough here, but if you need help, you can find me on the Discord server!

### Plugin Install

First, install the plugin.

In the game, run plugin/install <github url>.

## Configuration: Aresmush Directory

In `aresmush/plugins/profile/custom_char_fields.rb`, you will need to add the following line to `def self.get_fields_for_viewing(char, viewer)`:

`emblem: (Character.derive_emblem(char) || "").gsub(" ","-")`

If you have no other fields in this file, it should look like this:

```
      def self.get_fields_for_viewing(char, viewer)
        return {
          emblem: (Character.derive_emblem(char) || "").gsub(" ","-")
        }
      end
```
## Configuration: Ares-Webportal Directory

Choose whether or not you want both character profiles and icons to be customized based on your chosen emblems, or just one or the other. Then update the following files in the `ares-webportal` directory.

### Character Profiles

Navigate to `ares-webportal/app/templates/char.hbs`.

In this file, you will want to add a new line right at the top of the file:

`<div class="emblem_{{this.model.char.custom.emblem}}">`

Scroll down to line 50, and add a new line with your closing div, `</div>`. You know you've put the div in the right spot if it is just above a `<hr>` followed by `<b>Tags:</b>`.

This create a wrap-around for your profiles so you can add the necessary styling.

### Icons

Navigate to `ares-webportal/app/components/char-icon.hbs`.

In here, you are just adding another class to the div that already exists at line 1. Your final div should look like this:

`<div class="char-icon-container emblem_{{this.model.char.custom.emblem}}">`

This creates a wrap-around for your icons so you can add the necessary styling.

## Configuration: Emblem Config

The last step is to set up your in-game config for emblem. Navigate to the plugin's YML file in the Game Setup menu. You will want to decide if your emblem is based on a demographic or a group (or if you want to assign them all with custom fields).

If you're going to be using a demographic, you'll set the FIELD_TYPE to "Demographic." If you want to use a group, you'll set the FIELD_TYPE to "Group." If you want to customize this field, you'll set FIELD_TYPE to "Custom." Once you've decided on your FIELD_TYPE, you can then tell the config the exact field you want your Emblem to look at. For example, if you want your emblem to be set based on Faction, you will set your FIELD_TYPE to Groups and your FIELD to Faction. You can keep your FIELD entry blank if you are using custom fields.

For example, here's the setup if you are using Faraday's default groups:

```
---
emblem:
  field_type: Group
  field: Faction
```

In Faraday's default setup, this will assign Navy or Marines to each character's emblem. You could even, again using Faraday's defaults as examples, set it to Colony and that will assign one of the starting Colonies to each character's emblem.

<b>Please note,</b> if you are using free-form demographics as a field, it will create a lot of different emblem types and base it entirely on each player's individual response (for example, if you have two booksellers and one spells it Bookseller and another spells it Book Seller, it will create a different emblem). Use demographics with care!
