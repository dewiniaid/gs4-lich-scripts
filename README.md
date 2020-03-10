# LostRanger's Lich Scripts for GS4

I write a lot of quality-of-life and utility scripts for GS4.  Here's a list of them and a short summary of what they do.

Note that the Lich repository is probably more up-to-date than the Github; see everything I've written with:

`;repo list --author=LostRanger`

## What's Relatively New
(These are repeated in the other lists):

**Tour**: Travel the world without going anywhere.  While ;tour is running, most movement commands only pretend to move you.  This allows you to look at room descriptions and other attributes without actually visiting them.

**DragStop**: Automatically stop dragging a corpse when they stop being a corpse or they arrive at a town or Voln.  (You can manually start dragging again if you need to move them more.)  

**DoctorWho**: Highlights (in monsterbold, for now) when you receive healing from a player, so you can see Who your Doctor was.

**Route2**: This is a troubleshooting script that outputs details on the route that would be taken by ;go2 -- either
from your current room to your destination (`;route2 228`) or between two different rooms (`;route source target`).

**Triage**: Triage creates a Stormfront window displaying the status of lifekeep, injuries, linked clerics and chrisms
for dead bodies.  Only works for clerics and paladins.  Non-Stormfront users can access the same information using the
`TRIAGE` verb (no semicolon) while the script is running. 

**Version**: Version what version of Ruby, Lich, and assorted other things you are using -- along with some other
troubleshooting information.

**ReVerb**: Reverb improves built-in verbs and tries to make it as seamless as possible.  Among other things, it allows
INV to work without RT (Ask Wyrom why this is even a thing), causes GIVE to prioritize players in the current room 
(rather than a realm away), allows GIVE \<player\> ALL to give a player all your silver (without seeing how much that 
is first), prevents PUT from dropping items, and prevents awkward mishaps with SMILE.

**Fixit**: Fixit is an attempt to automatically fix various Lich issues.  It tries things like restarting infomon,
doing `;magic reset`, `SPELL ACTIVE`, `SKILL FULL`, `INFO` in that order, and a handful of other things.  It's intended
as a simple first thing to try if something is misbehaving.

**TLogin**: TLogin is a fork of `;login` that uses the same login information to log you into the GS Test instance.

## Noteworthy Scripts For Everyone

**Foreach**: Quickly perform a series of commands to all items matching a criteria you specify, like moving all boxes 
in your inventory to your locker.  [Detailed Documentation](Foreach.md)

**Recolor**: Change the colors of familiars, demons, animal companions and other friendly NPCs in room descriptions so
you don't think there are monsters in the room.

**XNarost**: eXperimental Narost branch that adds some improvements, mostly a list of human-readable map names with 
useful organization.

**Spellmerge**: Condenses the effects of certain mass spells falling off (and other similar effects) to reduce screen
scroll.  See `;spellmerge help` for more details or `;spellmerge theory` for a detailed explanation of how it works.

**ReVerb**: Reverb improves built-in verbs and tries to make it as seamless as possible.  Among other things, it allows
INV to work without RT (Ask Wyrom why this is even a thing), causes GIVE to prioritize players in the current room 
(rather than a realm away), allows GIVE \<player\> ALL to give a player all your silver (without seeing how much that 
is first), prevents PUT from dropping items, and prevents awkward mishaps with SMILE.   

**TargetID**: Assigns targettable creatures a unique 3-letter tag and allows you to target based off of that tag.
This is intended to let you determine which creature e.g. cast a gas cloud and target accordingly, but in actuality it's
mostly all about the amusing thing of seeing "a mammoth arachnid (WEB)" or "a rolton (RPA) while hunting. 

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

**ExpandedView**: When looking in containers, show item full names ("an opalescent glass jar containing selanthian 
bloodjewels") rather than short names ("an opalescent glass jar").  Only needed if you don't use `FLAG SORTEDVIEW`.

**Rested**: Keeps you online until your head is clear, your mana is full, and (if Dreaven or Iteno are around) you are 
Dreavened.

**No-Pronoun-Links**: Removes links from around `(he|she|his|her|him|it|its|himself|herself|itself)`

**SortedSkills**: Shows SKILLS, SKILLS FULL, SKILLS BASE, or SKILLS BASE FULL output in the same order it appears on
the Excel character training spreadsheet to make it easier to match up issues in your training.

**Triage**: Triage creates a Stormfront window displaying the status of lifekeep, injuries, linked clerics and chrisms
for dead bodies.  Only works for clerics and paladins.  Non-Stormfront users can access the same information using the
`TRIAGE` verb (no semicolon) while the script is running. 

**ReVerb**: Reverb improves built-in verbs and tries to make it as seamless as possible.  Among other things, it allows
INV to work without RT (Ask Wyrom why this is even a thing), causes GIVE to prioritize players in the current room 
(rather than a realm away), allows GIVE \<player\> ALL to give a player all your silver (without seeing how much that 
is first), prevents PUT from dropping items, and prevents awkward mishaps with SMILE.

**DoctorWho**: Highlights (in monsterbold, for now) when you receive healing from a player, so you can see Who your Doctor was.

**TLogin**: TLogin is a fork of `;login` that uses the same login information to log you into the GS Test instance.

**Go2Table**: Take you to the nearest available table.

**InTown**: Automatically perform actions after being in town for awhile, like killing `;spellactive`
 
## Developer Tools

**LogXML**: Logs all of the raw XML sent/received by the game.  Similar to `;log`

**ShowXML**: Opens a Stormfront window that shows XML sent/received by the game in realtime.  (**StormFront/Profanity Only**)

**MenuID**: Clicking on an item link dumps information on what the item ID is and other miscellaneous info.  Also 
allows underscore commands to be directly input in Stormfront.  (**StormFront Only**)   

**Mapnav**: Tools for map database fixing.

**Version**: Version what version of Ruby, Lich, and assorted other things you are using -- along with some other
troubleshooting information.  Can also report what version of a specific script or scripts you have downloaded.

**Fixit**: Fixit is an attempt to automatically fix various Lich issues.  It tries things like restarting infomon,
doing `;magic reset`, `SPELL ACTIVE`, `SKILL FULL`, `INFO` in that order, and a handful of other things.  It's intended
as a simple first thing to try if something is misbehaving.

**Route2**: This is a troubleshooting script that outputs details on the route that would be taken by ;go2 -- either
from your current room to your destination (`;route2 228`) or between two different rooms (`;route source target`).

**Metamap**: Metamap is a tool for specifying information like proper names and categories for maps.  Its metadata is 
used by XNarost and possibly some other scripts.

**Tour**: Travel the world without going anywhere.  While ;tour is running, most movement commands only pretend to move you.  This allows you to look at room descriptions and other attributes without actually visiting them.

## Misc/Unsupported

**Bacon**: Reports whether you're in the right location for your bounty or not.  Sort of.  I blame Elementz and 
Newsby/Luxelle.

**Jarserve2**: Jar inventory management and API.  Unsupported, use at your own risk, but very powerful.

**Lich-Set-Login**: Explicitly sets the launch method Lich should try when launching your client.  Most people won't 
need this.           
