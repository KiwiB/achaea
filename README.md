# Nexus Scripts for Achaea
This is a collection of scripts for Achaea for use in the Nexus client.

To download any of the packages, go to the the Releases section above or click [here](https://github.com/samueldcorbin/achaea/releases/latest).

##Important notes
* The onLoad function in each package includes documentation on aliases and API in a comment at the top.
* Several of these scripts rely on advanced JavaScript features that are not supported by older browsers. If something isn't working, make sure you're using the latest version of Chrome or Firefox
* Some of the packages rely on each other. Make sure to check the REQUIRES section for any package you install. The required packages must appear in the same order in Nexus that they do in the list (you can reorder packages in Nexus by grabbing the dots on the left side of the package name in Reflex Packages and dragging).
* If you follow the package ordering in the table of contents below, there should be no conflicts.
* Some of the scripts require minimal configuration. Make sure to check the SETUP section for any script you install.
* After installing any of these scripts, save and restart Nexus to get them to start working.

##Packages
|                 Package Name                | Description                                                                                       |
|:-------------------------------------------:|---------------------------------------------------------------------------------------------------|
| [Disable sounds](#disablesounds)            | Sets volume to 0 at login.                                                                        |
| [Map display patch](#mapdisplaypatch)       | Changes player marker from flag to room outline and replaces exit text with area name.            |
| [Display notice patch](#displaynoticepatch) | Adds support for multi-chunk display notices.                                                     |
| [Tracking gmcp](#trackinggmcp)              | Provides an event infrastructure for GMCP messages.                                               |
| [Tracking defences](#trackingdefences)      | Tracks player defences.                                                                           |
| [Tracking rift](#trackingrift)              | Tracks the contents of the rift.                                                                  |
| [Golden touch](#goldentouch)                | Keeps your pack worn and your gold inside it, picks up gold dropped by your kills, and adds aliases for dealing with gold and shopping. |
| [Numpad movement](#numpadmovement)          | Keybinds for numpad movement.                                                                     |

<a name="disablesounds"></a>
##Disable sounds
Sets volume to 0 at login.

<a name="mapdisplaypatch"></a>
##Map display patch
Changes player marker from flag to room outline and replaces exit text with area name.

<a name="displaynoticepatch"></a>
##Display notice patch
Adds support for multi-chunk display notices.

####API
```
display_notice(TEXT)  
    Displays TEXT.  
display_notice(TEXT, FG_COLOR)
    Displays TEXT in the color of FG_COLOR.
    Example of printing "Oh no!" in red:
        display_notice("Oh no!", "red");
display_notice(TEXT, FG_COLOR, BG_COLOR)
    Displays TEXT in the colour of FG_COLOR on a BG_COLOR background.
    Note: Use "" for default colours.
    Example of printing "Oh no!" in red on blue:
        display_notice("Oh no!", "red", "blue");
    Example of printing "Oh no!" in default text colour on blue:
        display_notice("Oh no!", "", "blue");
display_notice(TEXT, FG_COLOR, BG_COLOR, TEXT2, FG_COLOR2, BG_COLOR2...)
    Displays TEXT in the colour of FG_COLOR on a BG_COLOR background followed by TEXT2 in the colour of FG_COLOR2 on a BG_COLOR background.
    Note: If the last FG_COLOR or BG_COLOR is unspecified or any FG_COLOR or BG_COLOR is "", the colours will be the last defined FG_COLOR or BG_COLOR (they will not revert to default colours).
    Note: Set FG_COLOR to "reset" to return text and background colour to default (accompanying BG_COLOR will be ignored). There is no way to revert text or background colour independently.
    Example of printing "Oh no!" in red on blue followed by " Anything but that!" in orange on green.
        display_notice("Oh no!", "red", "blue", " Anything but that!", "orange", "green");
    Example of printing "Oh no!" in red on blue followed by " Anything but that!" in orange on blue.
        display_notice("Oh no!", "red", "blue", " Anything but that!", "orange");
    Example of printing "Oh no!" in red on blue followed by " Anything but that!" in default colours.
        display_notice("Oh no!", "red", "blue", " Anything but that!", "reset");
    Example of printing "Oh no!" in red on blue followed by " Anything but that!" in default text colour on blue:
        display_notice("Oh no!", "red", "blue", "", "reset", "", " Anything but that!", "", "blue");
    Example of printing "Oh no!" in red on blue followed by " Anything but that!" in orange, then " This is madness!" in default colours, then " Why?!" in red.
        display_notice("Oh no!", "red", "blue", " Anything but that!", "orange", "", " This is madness!", "reset", "", " Why?!", "red");
```

<a name="trackinggmcp"></a>
##Tracking gmcp
Provides an event infrastructure for GMCP messages.

####API
```
tracking_gmcp.subscribe(GMCP_METHOD, MY_FUNCTION)
    Adds an event listener that calls MY_FUNCTION (passing it gmcp_args) whenever a gmcp message of type GMCP_METHOD (i.e., "Char.Vitals") is received.
    Returns: a handle that can be used to remove the subscription with handle.remove()
    Example of adding a listener that prints health on every Char.Vitals GMCP message:
        let print_hp = tracking_gmcp.subscribe("Char.Vitals", function (vitals) {
            print("HP: " + vitals.hp);
        });
    Example of removing the above listener:
        print_hp.remove();
```

<a name="trackingdefences"></a>
##Tracking Defences
Tracks player defences.

####REQUIRES
Tracking gmcp

####API
```
tracking_defences.check_defence(DEFENCE)
    Returns: true if the player has DEFENCE, otherwise false
    Example of checking for thirdeye defence:
        tracking_defences.check_def("thirdeye");
```

<a name="trackingrift"></a>
##Tracking rift
Tracks the contents of the rift.

####REQUIRES
Tracking gmcp

####API
```
tracking_rift.check_rift(ITEM)
    Returns: amount of ITEM in player's rift
    Example of checking amount of bisemutum:
        tracking_rift.check_rift("bisemutum");
```

<a name="goldentouch"></a>
##Golden Touch
Keeps your pack worn and your gold inside it, picks up gold dropped by your kills, and adds aliases for dealing with gold and shopping.

####REQUIRES
Display notice patch  
Tracking gmcp

####SETUP
Set pack_number in the package's onLoad to the ID number of the container for your gold (Ctrl+F "PACK NUMBER"). Make sure to keep it up to date if you change packs.

####ALIASES
```
gg
    Picks up gold off the ground (and automatically puts it in your pack).
gold
    Displays the current amount of tracked gold.
shop for ITEM1 ITEM2...
    Automatically checks wares for ITEM1, ITEM2, etc. when you enter a shop.
shop
    Toggles shopping on and off.
spend AMOUNT (gold) on ACTION
     Gets AMOUNT gold from your pack and does ACTION (then automatically puts remaining gold in your pack).
     NOTE: This isn't just for spending gold in shops! Use this for anything involving gold, like "spend 50 on give gold to Sarapis" or "spend 100 on drop gold"
     NOTE: If you're too lazy to look at the exact price, just make sure AMOUNT is larger than what what is needed for ACTION. Any unspent gold will automatically be put back in your pack.
split gold with PERSON1 PERSON2 PERSON3...
     Toggles off gold tracking and splits the tracked gold between yourself, PERSON1, PERSON2, PERSON3, etc.
     NOTE: If you don't want to include yourself, split the gold with one less person and then give it to them (using the spend alias).
track gold
    Toggles keeping a running total of gold picked up off the ground.
    NOTE: Toggling resets the total.
```

####API
```
golden_touch.check_tracked_gold()
    Displays the current amount of tracked gold.
golden_touch.get_gold()
    Picks up gold off the ground (and automatically puts it in your pack).
golden_touch.shop_for(ITEMS)
    Automatically checks wares for ITEM1, ITEM2, etc. when you enter a shop.
    NOTE: ITEMS is a string of items separated by spaces
    Example of shopping for a vial and a pipe:
        golden_touch.shop_for("vial pipe);
golden_touch.spend_gold(AMOUNT, ACTION)
    Gets AMOUNT gold from your pack and does ACTION (then automatically puts remaining gold in your pack).
    NOTE: If you're too lazy to look at the exact price, just make sure AMOUNT is larger than what what is needed for ACTION. Any unspent gold will automatically be put back in your pack.
    Example of getting 200 gold and requesting a letter:
        golden_touch.spend_gold(200, "request letter");
golden_touch.split(AMOUNT, PERSON1, PERSON2, PERSON3...)
     If AMOUNT, splits the tracked gold between yourself, PERSON1, PERSON2, PERSON3, etc. Toggles off tracking and uses tracked gold if AMOUNT is not specified.
     Example of splitting 100 gold between yourself, Sarapis, and Tecton:
         golden_touch.split_gold(500, "Sarapis", "Tecton");
     Example of splitting tracked gold between yourself, Sarapis, and Tecton:
         golden_touch.split_gold("Sarapis", "Clementius");
golden_touch.toggle_shopping()
    Toggles shopping on and off.
golden_touch.track_gold()
    Toggles keeping a running total of gold picked up off the ground.
    NOTE: Toggling resets the total.
```

<a name="numpadmovement"></a>
##Numpad Movement
Keybinds for numpad movement.

####SETUP
Delete the existing Numpad movement package that Nexus automatically installs. Then add this one, make sure the box on it is checked to enable it, save your client, and restart.

####KEYBINDS
```
.
    Toggles use of numpad for movement.
1
    Sends "sw".
2
    Sends "s".
3
    Sends "se".
4
    Sends "w".
5
    Sends "look".
6
    Sends "e".
7
    Sends "nw".
8
    Sends "n".
9
    Sends "ne".
/
    Sends "in".
*
    Sends "out".
+
    Sends "u".
-
    Sends "d".
```

####API
```
numpad_movement.toggle_numpad_movement()
    Toggles use of numpad for movement.
```
