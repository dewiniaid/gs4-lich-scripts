# Foreach 0.10.4

*Stop the up-up-up-enter!*

## What is it?

`foreach` is a script for sending a series of commands for each item that matches its criteria.

While not a full replacement for "proper" purpose-built Lich scripts, it is still frequently useful in its own regard.

## I don't want to read docs, can you give me some examples?

* No rogues around?  Move all your boxes to your locker:  
  `;foreach box in inv; move to locker`

* Own a gemcutter and want to cut all the gems in a container?  
  `;foreach gem in red sack; move to gemcutter; turn gemcutter; waitrt; move to blue sack`

* Getting boxes picked?  
  `;foreach box in inv; get item;give item to Rogue;waitfor Rogue offers you;accept item`  
  *(This'll only work if you're fast enough to empty boxes while the rogue is picking)*

* Checking for orb gems?  
  Sorcerer: `;foreach gem in queue bag;get item;waitmana 4;prep 704;cast item;pause;put item in reject bag`    
  Bard using `;loresing`: `;foreach gem in queue bag;get item;waitfor You remove;waitmana 20;;loresing purpose noun;pause;waitrt?;put item in reject bag`  
  While the script is paused, put the gem in the orb bag if it's an orb.  `;unpause` it will put it in the reject bag if you're still holding it.  

* Bulk reading scrolls?  
  `;foreach scroll in inv; read item`

* Accidentally move a bunch of items to the wrong container using foreach?  
  `;foreach in last;move to container` *(back where they started)*  
  *or* `;foreach in last;get item;move to correct backpack`
  
* Turning in gems or herbs for a bounty?  
  `;foreach name=uncut diamond in inv;get item;sell item`
  
* Don't want to sell ALL of your diamonds for said bounty?  
  `;foreach first 7 name=uncut diamond in inv;get item;sell item`
  
* Doing a long task and want to be able to resume it later (possibly on the same source container?)  
  `;foreach unmarked gem in whereever;get item;do lots of things;mark item;return` as many times as needed, followed by  
  `;foreach marked gem in whereever;get item;unmark item`  
  (`whereever` must be a container in your inventory or your entire `inv`)

* Need to ditch your sorcerer wands but want the wizard ones?  
  `;foreach q=/(bloodwood|twisted|yew|bone|glass|thanot) wand/ in inv;sell`  
  (This uses the new pattern matching introduced in 0.10.5)

     
## That's nice.  But what can it REALLY do?

Usage: **;foreach** _[options]_ _[what]_ **in/on/under/behind** _targets_ [**;** _command1_**;** _command2_**;** ...]  
*or:*  **;foreach** _[options]_ _[what]_ **in/on/under/behind** _targets_ [**/** _command1_**/** _command2_**/** ...]  
*or:*  **;foreach** _[options]_ _[what]_ **in/on/under/behind** _targets_ [**|** _command1_**|** _command2_**|** ...]

### Options

_options_ is a set of optional filters on what matching items `foreach` will operate on.  It can consist of zero or more
of the following:
* **registered** _or_ **unregistered**
  Only filters items that are registered or not registered.  This option is only available if the items in question
  are in your inventory or your premium locker.  (`locker manifest` is used to determine item status in the latter case,
  which is not available for standard lockers.)
* **marked** _or_ **unmarked**
  Only filters items that are marked or not marked as unsellable.  This has the same restrictions as the above.
  You can use this in conjunction with MARKing items as pat of your ;foreach script to prevent operating on the same
  item more than once in situations where you might run ;foreach multiple times.
* **unique**  
  Only the first item with any given full name will be matched.  This is global between all containers, so if you have
  the same item in two different containers only the first item (whichever `foreach` sees first) will be matched.
* **first _N_**  
  Stop after _N_ matching items have been encountered.  (The word `first` is optional)
* **skip _N_** _or_ **after _N_**  
  Skip the first _N_ matching items.
* **sort** or **sorted**  
  Sort items by name within each respective container.  Articles "a", "an", "some" and "the" are ignored when sorting.  
  Containers will still be handled one at a time, and the order of the containers themselves is not affected.
* **reverse** or **reversed**  
  Reverse the order of items within each container.  If used alone, this means the last item in each container in
  processed first and vice versa.  If used with **sorted**, items will be handled in descending alphabetical order
  rather than ascending.
  Containers will still be handled one at a time, and the order of the containers themselves is not affected.
  
Options are applied roughly in the order listed above.  You can combine **first** with **skip**: 
`;foreach first 5 after 10` will match items 11 through 15.

### What

_what_ allows you to filter the types of items `foreach` acts on.  It can be one of the following :

* **type=**_pattern_   or  **t=**_pattern_  or  simply _pattern_   
   You can specify any type of item that Lich knows about.  Examples include 'gem', 'wand' and 'scroll' 
   (which includes things like palimpests and other 'alternate names' for scrolls).  `;foreach` by itself will show all
   the known types.  `;foreach in inv` with no commands will show the types of all items it finds.
   
   You can use `type=none` to explicitly find items that have no defined type according to Lich. 
   
   **Note:** This is only as accurate as Lich's own type data.  You may be able to get more accurate type data by downloading and running the `gameobjadd` script at startup.

* **noun=**_pattern_   or  **n=**_pattern_   
   You can specify items that have a specific noun.  This must be an exact match (unless using wildcards).
   
* **name=**_pattern_   or  **m=**_pattern_   
   You can specify items that have a specific name.  This may or may not excludes the article, dpeend
   
* **fullname=**_pattern_   or  **f=**_pattern_   
   You can specify items that have a specific full name.  This is like `name=pattern`, but includes things like long 
   descriptions.  See `;foreach in inv` to get a list of item full names.
   
* **quick=**_pattern_   or  **q=**_pattern_  
   Identical to `fullname`, except the ends of `pattern` are wildcarded.
   `;foreach q=red,blue ...` is equivalent to `;foreach f=*red*,*blue* ...`      
   
_pattern_ can be:
  * `one or more words with including spaces`: Must be an exact match
  * `*use*asterisks*for*wildcards`: Asterisks function as wildcards.  `name=*snowflake-cut*` will find anything with 
    'snowflake-cut' anywhere in the name, for instance.
  * `this,that`: Match any one of the options separated by commas.
  * `blue sapphire,emerald,*diamond,*emerald,uncut ruby`: You can mix the above.
  * `/(blue|white) crystal/`: You can match against a regular expression.  The slashes are required, and matching is 
    always case-insensitive.
  
### Targets

_targets_ tells ;foreach where you want to look for the items.  You can specify one or more _target_, separated by commas.

Each target can be the name of an actual container in game ("cloak", "my backpack", etc.), or one of the following options:

* **inv** or **inventory**  
  Examines the contents all containers in your inventory.  Note that containers in your hands are not in your inventory.
  
* **floor** or **ground** or **room**  
  Examines items on the ground.  *The items, not their contents*
  
* **loot**  
  Examines the contents of containers that are on the ground -- boxes, disks, errantly left behind backpacks, etc.
  
  **locker**
  Examines the contents of your locker.  *If your locker is not open, ;foreach will open it and close it when it is done*.
  
* **worn**  
  Examines items in your inventory, *not* their contents.  Want to register everything you are wearing?  
  `foreach in worn; remove item;register item;wear item`
  
* **last** or **previous**  
  Last is a set of items that matches whatever the last run of foreach found.  You can use this to quickly undo if you
  manage to flub something up, like accidentally moving all of your inventory to your locker rather than just your boxes.
  
  When using **last**, the container items are reported in will be the container they started the previous run in -- not
  their current location.  So `;foreach in last;move to container` will move all (reachable) items to their original
  locations.
  
* **fastinv** or **qinv**  
  Similar to **inv**, but this uses container contents as currently known by Lich rather than doing an INV FULL.  This 
  allows for faster startup, but with two caveats:
  - Item registration/marked status is unavailable.  (If you're using one of the relevant _options_, this will be
    automatically converted to `inv`)
  - Containers that Lich doesn't know the contents of will be skipped.  Most notably, if you have not LOOKed in a 
    container since you have logged in, that container will be skipped.  Note that `;foreach in inv` does NOT satisfy
    the "looked in a container" requirement, but `;foreach in worn;look in item` probably will.
  
  If you specified a _what_ (above section), it will further restrict the list of what foreach is working on.
  
### Commands

Commands are separated by semicolons (`;`), slashes (`/`) or pipes (`|`).  You can only use type of separator in a given
invocation of Foreach -- it's determined by the first one Foreach sees.  The examples here all use semicolons, but the other two formats may be useful if one of your commands needs to
actually include semicolons (aside from Lich scripts) -- say, the lines of `LORESING`.

If you don't give Foreach any commands to perform, it simply spits out a list of all the matching items and some
relevant stats.  This is useful, but usually you want to do something with items.

Any command you can send to the game can be used with Foreach.  There's also a few shortcuts, conveniences, and some
very basic scripting support (like waiting for a piece of text)

For commands to be worthwhile, there needs to be an option to replace some part of the command with information about
the item.  For this, there's some variable replacement rules -- as noted in the list below:

#### Replacements

Certain words within a command will be replaced with some detail about the current item.  Here's a list.

* **item**  
  The most important replacement.  This is replaced with an exact reference to an item using its unique ID.  It's good
  for commands, but not particularly useful if you want to talk about the item in SAY or ECHO

* **container**
  This is like `item`, but refers to the container the item was found in.  It won't work properly if the item was not
  in a container (e.g. when using the `floor` and `worn` targets
  
  If you're using the `last` target, `container` refers to the container the item was *originally* in.  Thus, the 
  following will dump your inventory into your locker and then put it all back exactly how it was before, aside from
  container orders becoming flipped:
  
  `;foreach in inv;move to locker`  
  `;foreach in inv;move to container`
  
* **noun**
  This will be replaced with the item noun, e.g. `diamond`, `chest`, `scroll`.
  
* **name**
  This will be replaced with the item name, e.g. `uncut diamond`, `enruned mithril chest`, `old scroll`.
  
#### Shortcuts
For some normal game commands, typing `command` by itself is equivalent to `command item`.  Also, if one of these commands
is the first command in the list, an implicit `get item` might be automatically added provided that the command refers to
the item in some way.  These commands currently are: **drop**, **place**, **sell**, **put**, **appraise**, **read**, 
**look**, **inspect** and **analyze** 

In addition, the following convenience shortcuts exist:

* **move** _[what]_ **to** _where_  
  Shorthand for getting _what_ and then putting it in _where_.  If you omit _what_, 'item' is assumed.  If _where_ is
  your locker, `;foreach` tries to open it (once) and, if successful, will close it when done.
  
  **mv** is shorthand for **move**
  
  Newer versions of foreach will use `_drag` (mimicking Stormfront drag-and-drop functionality) to reduce this from two
  commands to one when they think it is safe to do so.

* **fastmove** _[what]_ **to** _where_  
  This is a somewhat faster version of the above, but it's more prone to failing if you have oddly scripted containers
  involved or tasks that involve roundtime.  It utilizes typeahead -- namely sending the GET and PUT portions at the
  same time.
  
  This can be shortened as **fastmv**, **fmove** or **fmv**

  Newer versions of foreach will use `_drag` (mimicking Stormfront drag-and-drop functionality) to reduce this from two
  commands to one when they think it is safe to do so.

* **stash** _[what]_  
  This tries to store _what_ in your defined lootsack.  If it is full, it tries lootsack2 and so on.
  
  You can setup your lootsacks with `;vars` -- e.g. 
  ```
  ;vars set lootsack=backpack
  ;vars set lootsack2=cloak
  ```
  
  Lootsacks that stop being in your inventory while **Foreach** is running will stop having items fed to them.  

* **giveitem** [**to**] _player_
  If this is the *first* command, foreach adds an implicit `get item` before it.  
  Offers the item to the specified _player_ and waits for them to accept it before continuing.  
  
  This is only for players; NPCs don't produce the appropriate 'has accepted your offer' messaging.  That said, you
  don't need to wait for NPCs to accept item offers anyways -- so just use `give` instead of `giveitem` for them.
  
* **appraise** or **appraise item**
  If this is the *first* command, foreach adds an implicit `get item` before it.  
  If you're still holding the item at the end of running all of the commands, it will be returned to its parent 
  container.   
  `appraise` by itself is shorthand for `appraise item`.
  
* **return**
  Equivalent to `put item in container`.  Returns whatever item Foreach is currently processing to whereever it 
  originally found it.  Just like `put item in container`, this only works if the item is still in your hands (which
  you can take advantage of in your commands)
  
#### Control
  
* **echo** _message_  
  Echos the specified text to your client.  Not sent to the game.
  
* **pause**  
  Pauses ;foreach.  This is particularly useful if you're using ;foreach to examine a set of items and making decisions
  about them.  For instance, you can write a script to test gems for orbs, pause after casting/singing, and then manually
  move the gem to the correct container based on the result before unpausing foreach to continue to the next gem.
  
* **sleep _time_**  
  Pauses ;foreach.  This is particularly useful if you're using ;foreach to examine a set of items and making decisions
  about them.  For instance, you can write a script to test gems for orbs, pause after casting/singing, and then manually
  move the gem to the correct container based on the result before unpausing foreach to continue to the next gem.
  
* **waitrt** or **waitrt?**  
  Waits for roundtime to finish.  If you are not currently in RT, `waitrt` assumes you are about to be and waits
  it to happen.  `waitrt?` only waits if you are currently in RT, and might miss RT that you're about to be in (i.e.
  due to a command you sent that the server hasn't received yet.)
  
  These function identical to, and are implemented using, the corresponding functions in Lich.

* **waitcastrt** or **waitcastrt?**
  As above, but waits for cast roundtime instead.  The same limitations and caveats apply.  
  
* **waitfor** _text_  
  Waits to see the specified bit of text.  Just like Lich's `waitfor`, except only one string of text can be matched.
  Matching occurs in text mode (e.g. not the raw XML stream)
  
* **waitre** _pattern_  
  Waits for text matching the regular expression _pattern_.  Note that matching currently applies in XML mode.  
  You can use the `;logxml` or `;showxml` scripts to get an idea of what raw XML is being sent to the client.  
  [Regex101](https://regex101.com/) is a good resource for testing out patterns against text from the game before trying them for real.     
  
* **waitmana** _amount_ or **waitmp** _amount_  
  Waits until you have at least this much mana.
  
* **waithealth** _amount_ or **waithp** _amount_  
  Waits until you have at least this many health points.
  
* **waitspirit** _amount_ or **waitsp** _amount_  
  Waits until you have at least this much spirit.
  
* **waitstamina** _amount_ or **waitst** _amount_  
  Waits until you have at least this much stamina.

## Technical Explanation

If you think `;foreach` is doing something funny, here is a rough overview of the entire logic process from start to finish:

Depending on the level of depth you want, either just read the bold parts, just read the first sentences, or read the whole thing.

1. **Option Parsing**: All of the options are interpreted, validated, and converted into a usable format

2. **Implicit Commands**: The command list is fixed up
   * Shortened versions of some verbs that Foreach has special handling for are expanded to their full versions.  This
     includes shortened versions of `drop`, `place`, `sell`, `appraise`, `register`, `get` and `take`.
   * If `stash` is in the command list, a list of lootsacks is built and the script exits if it can't find any.
   * Any `giveitem` commands resolve to an exact player name in the room (and the script fails if no match is found).  
   * Commands like `place`, `sell`, `stash`, `appraise`, `mark` and `unmark` get the word `item` added to the end unless
     they already have something after them.  (`sell` becomes `sell item`, `sell emerald` is unchanged.)
   * *For the first command only:* An implicit `_remget item` (for worn targets) or `get item` (everything else") is 
     added for commands that are known to only work with an item in hand, such as `giveitem`, `place`, `sell`, `stash`, 
     `appraise`, `register`, `mark` and `unmark`.  This only applies if the command string is `get item`, i.e. 
     `get emerald` skips this check.
     - `_remget` is a hidden Foreach command equivalent to "remove item if its parent container is `_worn`, otherwise 
       get it."  
   * *If there is only one command*: For commands in the above list that don't dispose of an item (so not `drop` or 
     `sell`), an implicit `return` is added to the end of the list
     
3. **Item Scanning**: Foreach builds a list of containers and items that are in them.
   At this point, `foreach` has an empty map of container IDs to names (with fake IDs `_worn` and `_ground` for those
   two targets), an empty map of container IDs to lists of container contents, and an empty list of containers to scan.
   Throughout this process, container names are mapped as they're discovered (mainly for the item listing mode if you
   `;foreach in ...` with no commands), some container IDs are added to the "to scan" list, and each container along
   with its items are added to the "to filter" list.  If you're filtering by status (marked/registered status, e.g. 
   `;foreach unmarked`), a table is made of items and their registration statuses.
   
   * `inv` aborts if you specified at least one command but no item types to avoid affecting every single thing you own
     (unless you did `;foreach ALL in inv`).  Otherwise it reads all of your items (second level results) and their
     containers (first level results).  If you're filtering by status, their status data is saved as well.
   * `last` simply restores the container/item data from the previous ;foreach invocation.  It fails immediately if 
      you're filtering by status.
   * `loot` adds each container on the ground to the "to scan" queue.  It fails immediately if you're filtering by 
      status, as that information cannot be determined for items on the ground.
   * If you are premium, `locker` determines your location and looks at the appropriate `locker manifest`.  If, for 
     some reason, `;foreach` can't determine your current location, it fails if you're filtering by status (only the
     locker manifest can show marked/registered for contents of a locker), and otherwise adds the locker containers to 
     the "to scan" queue.
   * `floor` (and synonyms) places all the contents of the room (GameObj.loot) into the `_ground` container.  Like 
     `loot`, it fails immediately if you're filtering by registration/mark status
   * `worn` places `GameObj.inv` into the `_worn` container.
   * Anything else is treated like a single container: If you are filtering by status and your target is `locker`
     (only possible for non-premium lockers), ;foreach exits immediately with an error.  Otherwise, it LOOKs in the 
     container to get the game to send over its inventory data and determine what its ID and searches for status data
     (if needed, see the "To Scan" section)
     
   Then, if there are any containers to scan:
   * The `sorter` script is paused, if it is running, to avoid spam.
   * Each container is looked in (to get or refresh its inventory)
   * For each container: )
     * Its contents are added to the "to filter" list
     * If you are filtering by status and status data is unknown for this container: (The INV commands here are only done once and data saved for subsequent containers)
       If the container is in your right hand or left hand, an `INV HANDS FULL` to get its registration status
       Otherwise, an `INV FULL` is done, followed by an `INV HANDS FULL` if the container was not found in your inventory.
       If, after all of the above, ;foreach doesn't have status data for the container (meaning it's probably not in your inventory),
       it assumes that it won't have status data for any of its contents either and exits.
   * The `sorter` script is resumed, if it was paused.
       
4. **Item Sorting**: If the SORTED or REVERSED options are specified, they are applied to each container inventory. 

5. **Item Filtering**: For each container and list of items in to_filter, items are filtered based on your criteria, 
   including applying any options (e.g. `UNIQUE`, `MARKED`, `FIRST 5`).  If you're filtering by status and any item is
   not found in the the table, `;foreach` complains loudly and exits.  This particular failure should never occur,
   since of all the related checks should have happened earlier.
       
   Any containers that have items left after the filtering phase are removed from the relevant tables.
   If no remaining items exist, ;foreach exits.
   
6. **Snapshot**: At this point, ;foreach knows every single item and container it is going to work on.  This information
   is saved for later usage in `;foreach in last`, and then the execution phase begins
   
7. **Execution**: For each container:
   * If there are no commands, pretty-print a list of matching contents in that container.
   * Otherwise, for each item in the container, for each command:
     * Replace "item", "noun", "name" and "container" with the item ID, item noun, item name and container ID.
     * If the command starts with `$lich_char` (usually `;`), run it.
     * If this is any of the convenience shortcuts (i.e. MOVE) or control commands (PAUSE), handle it.  Some noteworthy
       things:
       * MOVE/FASTMOVE will try to learn the ID of containers it is moving something to, since `_drag` only works with IDs.
         and allows movements to be done in one command rather than two.  If the IDs are known, it uses `_drag`.  It also
         has its own logic for adding an implicit 'item' if needed since it is not at the end of the command.
       * STASH also tries to use _drag when possible, which is almost all the case since lootsack IDs are determined at startup
         and the IDs of 'item' are always known.
     * Some commands define a "run this before the next command" procedure (usually consisting of 'waitrt?' and
       sometimes waiting for a prompt).  Most commands (including anything sent natively to the game) will execute 
       and clear this, but some control commands (like `waitfor`, `waitre`, `pause` and `sleep`) ignore it and let
       the next command in the list resolve it.  This prevents various issues like trying to `waitfor` a message that
       would scroll by while in roundtime, or the script waiting to pause (and thus preventing it from being unpaused
       until you are out of roundtime)

## Changelog

### Version 0.10.5 (2019-06-20)
  * Properly unsilence ourselves when we send actual commands.  Previously, people who trusted Foreach could not
    see the commands it sent.  Now, only commands related to determining your initial inventory are squelched.
  * You can now `;foreach q=/pattern/ in ...` to do regular expression matches.

### Version 0.10.4 (2019-06-20)
  * Made multiple fixes to 'giveitem'.  It should actually be recognized as a command again.
  * Added a new 'fastinv'/'qinv' target, which is similar to 'inv' but skips the INV FULL step.  This allows for
    slightly faster startup at the cost of being unable to check marked/registered status and being potentially
    unaware of the contents of containers that you have not previously LOOKed in.

### Version 0.10.3 (2019-06-13)
  * Overhauled Stormfront status bar updates.  They should no longer significantly slow down the script.  
    Additionally, status bar updates are less frequent (except when they are the result of the script pausing
    or resuming)
  * Running ;foreach while ;foreach is already running (i.e. with ;force) no longer clobbers the statusbar in Stormfront.
  * Immediate !commands now delay every 5 consecutive immediate commands rather than on every fifth item.
  * `;foreach ... in backpack on table` should now work correctly.
    - Note that a quirk in GS4's item parsing code means you will frequently need to use 'get item in container'
      syntax when using foreach in this way.

### Version 0.10.2 (2019-06-12)
  * Sorting is now case-insensitive.
  * Read, Look, Analyze and Inspect now add an implicit 'item' if needed.
  * Giveitem now searches for a matching PC in the room at startup and autocompletes to that PC's full name.  
    * It will error out immediately if no matching PCs are found or if more than one matching PC is found.  
    * It will no longer fail when you do not type the character's name entirely with proper capitalization.  
  * **Unspported:** Commands can be prefixed with a "!" to ignore various checks on usability in favor of speed.
    *Use this at your own risk!*

### Version 0.10.1 (2019-06-11)
  * Fix marked/unmarked/registered/unregistered filters not working in some cases.

### Version 0.10 (2019-06-10)
  * Foreach now reports its status in the shortcut bar in Stormfront, and can be paused, resumed, or killed from that location.
  * You can now look under/behind containers instead of just in/on them.
  * Legacy premium locker searching now looks ON armor stands and weapon racks rather than IN them.    
    (This code only triggers if LOCKER MANIFEST fails for whatever reason).
  * Pattern matching is no longer case-sensitive  
    `;foreach q=elan*pack in pack` should now match "an Elanthian Guilds voucher pack"
  * You can now `;foreach sorted ...` to iterate over items sorted by their full name within their containers.  
    * Articles a/an/some/the are ignored when sorting, so "some ambrominas leaf" sorts before than "a clear zircon"
    * This does not affect the order containers are traversed. 
  * You can now `;foreach reversed ...` to iterate over items backwards within their containers.
    * When combined with `sorted`, this does items in descending order.
    * This does not affect the order containers are traversed.
  * You can now specify multiple targets in a single `;foreach command`  
    Example: `;foreach gem in backpack,cloak,Wyrom disk,locker ...`
  * `;foreach type=none ...` will now match items that have no explicit type defined.

### version 0.9.9 (2019-05-24)
  * Add `UNMARK` shortcut for `MARK <item> REMOVE`
  * Improved documentation, including updating the online documentation
  * Convenience shortcuts for in-game verbs should now trigger when abbreviated:
    DRop, PLACe, SELl, APpraise, SELl, and REGister.
  * If the first command is `get`, `take`, `register`, `mark` or `unmark`, an implicit `get item` or
    `remove item` will be added beforehand.  This behavior already existed for drop, place, sell and appraise.
  * If the *only* command is `appraise`, `register`, `mark`, or `unmark`, items will be `RETURN`ed
    afterwards in addition to the implicit `get`/`remove` item at the start.
  * Shortened in-repo changelog.

### Version 0.9.3 (2019-05-24)
  * No longer times out if you `;foreach in empty container` and have FLAG SORTEDVIEW ON
  * Improved error messaging when targeting empty, closed or nonexistant containers.
  * Fix `;foreach quick` when using multiple patterns (`;foreach q=red,blue in ...`).  It now functions as 
  `;foreach f=*red*,*blue* in...` rather than `;foreach f=*red,blue* in ...`.
  * `move to locker` automatically opens your locker if needed just like `foreach in locker` does.

### Version 0.9.2 (2019-05-22)
  * Now correctly works with locker manifests, even if there's nothing on your armor stand.

### Version 0.9.1 (2019-05-21)
  * You can now `;foreach quick=phrase in ...` or `;foreach q=phrase in ...` as an alternate method for matching items
    This is equivalent to `;foreach fullname=*phrase* in ...` (note wildcards!)

    This feature was documented in 0.9, but was not implemented in that release.

### Version 0.9 (2019-05-20)
  * You can now further filter displayed items with `;foreach [un]registered ... in ...` and
    `;foreach [un]marked ... in ...`
  * `;foreach in locker` will now open your locker if it is not already, and will close your locker when it exits
    if it originally opened it.
  * `;foreach in locker` in a premium locker will now try to use your locker manifest to avoid needing to scan
    multiple containers.  This adds a substantial startup speed boost.  If this attempt fails, it is still
    improved in that it will no longer examine the curtain, your disk, or any other non-locker containers in the
    room.
  * Added some more pretty formatting to `;foreach help`  (Stormfront only)
  * Make a second attempt at improving compatibility with the Profanity FE

### Version 0.8 (2019-05-09)
  * `;foreach in locker` should now work for non-premium lockers.
  * Improve compatibility with the Profanity FE
  * Fix an issue which caused Foreach to stall briefly in some cases.
  * No longer prompts to install xmlpatch on first launch
  * Add `waitcastrt`, `waitcastrt?` and `waitrt?` commands
  * Add `sleep X` command, where X is a duration in seconds.
  * Add `UNIQUE`, `FIRST #` and `AFTER #` options.

### Version 0.7 (2017-07-24)
  * Add `waitmana`, `waithealth`, `waitspirit`, and `waitstamina` commands.
  * Change suggestions for GameObjAdd and GameObjAddMore to use xmlpatch instead.
  * Automatically migrade to xmlpatch on startup if GameObjAddMore is present.

### Version 0.6.2 (2017-06-29)
  * Fix an issue where every command after a script would be interpreted as a script.

### Version 0.6.1 (2017-06-29)
  * Reminder to self to do better testing
  * Fix foreach not working on anything but item types bug introduced in 0.6

### Version 0.6 (2017-06-29)
  * Add first-time setup walkthrough.
  * Add `STASH`, which tries storing the item in your lootsacks (in order)
  * `MOVE` and `FASTMOVE` now internally use _DRAG when they can, which reduces them to one command instead of two.
  * `DROP`/`DROP ITEM` now internally uses _DRAG when it is the first command in the list.
  * Increased inventory command timeout from 5 seconds to 15.

### Version 0.5.4 (2017-06-25)
  * Another small bugfix to preemptive `GET` commands.

### Version 0.5.3 (2017-06-25)
  * Small bugfix to preemptive `GET` commands.

### Version 0.5.2 (2017-06-25)
  * Now properly handles containers with `FLAG SORTEDVIEW ON`.

### Version 0.5.1 (2017-06-25)
  * Cosmetic bugfix to help summary.

### Version 0.5 (2017-06-24)
  * `;foreach in inventory` now uses and parses `INV FULL` rather than the tedious scanning process and relying on
    Lich's container management.  This is MUCH faster than the old approach.
  * You can now use `t=`, `n=` and `m=` as shorthand for `type=`, `noun=` and `name=`, respectively
  * Added `fullname=` and `f=` to matches an item's FULL name.  (This was actually an unadvertised addition in an
      earlier update)
  * You can now filter on multiple values (`;foreach noun=diamond,emerald,sapphire in ...`)
  * `MOVE TO container`  and  `MOVE ITEM TO container` will no longer attempt to grab the item first if you're already
      holding it.
  * `MOVE TO GROUND/FLOOR` and `MOVE ITEM TO GROUND/FLOOR` will now place items on the ground.
  * Added `GIVEITEM [TO] <player>` command, which gives the matched item to a player and waits for them to accept.
      Roughly a shortcut for `give item to player; waitfor player has accepted`
  * Added `RETURN`, which returns the item to its original container (or the floor) if you are still holding it
  * `DROP`, `PLACE`, `SELL` and `APPRAISE` will now expand to `DROP ITEM`, `PLACE ITEM`, `SELL ITEM` and `APPRAISE ITEM`
      respectively.
  * `DROP ITEM`, `PLACE ITEM`, `SELL ITEM`, `APPRAISE ITEM`, `GIVEITEM` will now `GET` the item first if they are the first
      command in the list.
  * `APPRAISE ITEM` will return the item to its orginal container if it is the only command.
  * Overhauled documentation, including a link to web-viewable documentation

### Version 0.4.3 (2017-06-23)
  * Added MV shortcut for MOVE
  * Added FASTMOVE, FMV and FASTMV which are slightly faster methods for moving items but may be a bit fragile.

### Version 0.4.2 (2017-06-23)
  * Support negative container IDs.  Allows use on pawnshop tables, among other things.

### Version 0.4.1 (2017-06-21)
  * Cosmetic fixes to help messaging.

### Version 0.4 (2017-06-21)
  * Add PAUSE command, which announces the current item and then pauses the script.  Unpausing the script picks up
      where it left off.
  * Added a safeguard against operating against every single item in your inventory.
  * Added foreach in last;... which operates against the same set of items the last copy of the script found.
      Handy if you want to e.g. undo moving items, and the list can be additionally filtered as well.
  * Added prettier formatting for Stormfront users.
  * Item lists now include links for Stormfront users.
  * When scanning a large number of containers, make periodic progress updates.
  * When processing a large number of items, make periodic progress updates.
  * Fix a cosmetic bug where container scanning might cause excessive amounts of blank lines to be sent to the client.

### Version 0.3.1 (2017-06-20)
  * Actually re-enable code disabled by accident in 0.3.  So foreach actually works again.

### Version 0.3 (2017-06-20)
  * Added convenience shortcut commands MOVE, WAITRT, WAITFOR, WAITRE, and ECHO.  See help for examples.
    Example of move: ;foreach item in loot;move to backpack

### Version 0.2.1 (2017-06-19)
  * Fix 'on ground/on floor', and add 'room' as an alias

### Version 0.2 (2017-06-19)
  * Add support for invoking Lich scripts.  (foreach gem in inv;;script item)
  * Add improvements to handling commands that create Roundtime.
  * Using foreach with no commands now produces a somewhat better inventory list.
  * Minor text fixes.

### Version 0.1 (2017-06-18)
  * Initial release