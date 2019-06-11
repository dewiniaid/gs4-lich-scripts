# LostRanger's Lich Scripts for GS4

I write a lot of quality-of-life and utility scripts for GS4.  Here's a list of them and a short summary of what they do.

Note that the Lich repository is probably more up-to-date than the Github; see everything I've written with:

`;repo list --author=LostRanger`

## Noteworthy Scripts For Everyone

**Foreach**: Quickly perform a series of commands to all items matching a criteria you specify, like moving all boxes 
in your inventory to your locker.  [Detailed Documentation](Foreach.md)

**Recolor**: Change the colors of familiars, demons, animal companions and other friendly NPCs in room descriptions so
you don't think there are monsters in the room.

**XNarost**: eXperimental Narost branch that adds some improvements, mostly a list of human-readable map names with 
useful organization.

**Spellmerge**: Condenses the effects of certain mass spells falling off (and other similar effects) to reduce screen
scroll.  See `;spellmerge help` for more details or `;spellmerge theory` for a detailed explanation of how it works.

**Go2Shop**: Travel to a player shop by name.  The playershop does not need to be in the map database.

**Survey**: Quick and dirty script to look at everything in the room, ideal for examining merchant events.

**XMLPatch**: Tries to add missing information to Lich's type data for identifying what objects are.  Largely obsolete,
but still required (as of June 10, 2019) for familiars and animal companions to be properly identified unless you run
`;recolor`

**Inspect**: `;inspect item` is like `inspect item` but adds information on what the relative terms actually mean. 
For instance, rather than showing "a significant amount", ;inspect will show "a significant amount (100-119 lbs)"

**Herbs2**: Like `;herbs` but highlights rows that apply to your current injuries.

**Box**: Convenience tools for locksmiths and customers that would prefer to use their own brain when dealing with boxes.

**EGet**: GETs an item from one of your containers that matches the text specified.  This does not need to match the
actual item adjective, so `;eget frosted cookie` and `;eget almond cookie` will both get "a frosted almond cookie" 

**QVerb**: Using similar logic to EGet, `;qverb rub black crystal` will pull out a black crystal, rub it, and return it
to where it came from (if it still exists).  Useful for aliases (`;alias add --global ;disk=;qverb rub black crystal`)

**Findwarcamp**: Tries to find a Grimswarm warcamp using a smarter search algorithm than similar scripts.  Best if used
when you know you're near one but haven't narrowed down the exact location.

**Loglore**: Logs loresinging, ASSESS, INSPECT, ANALYZE and the Abandoned Inn Crystal results.

**Song-Timer**: Displays warnings at 2m/1m/30s/15s before songs renew to allow manual intervention if desired.  Best
when combined with highlights and sound effects.

## Developer Tools

**LogXML**: Logs all of the raw XML sent/received by the game.  Similar to `;log`

**ShowXML**: Opens a Stormfront window that shows XML sent/received by the game in realtime.  (**StormFront Only**)

**MenuID**: Clicking on an item link dumps information on what the item ID is and other miscellaneous info.  Also 
allows underscore commands to be directly input in Stormfront.  (**StormFront Only**)   

**Mapnav**: Tools for map database fixing.

## Misc/Unsupported

**Bacon**: Reports whether you're in the right location for your bounty or not.  Sort of.  I blame Elementz and 
Newsby/Luxelle.

**Jarserve2**: Jar inventory management and API.  Unsupported, use at your own risk, but very powerful.

**Lich-Set-Login**: Explicitly sets the launch method Lich should try when launching your client.  Most people won't 
need this.           
