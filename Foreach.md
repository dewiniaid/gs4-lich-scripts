# Foreach 0.7

*Stop the up-up-up-enter!*

## What is it?

`foreach` is a script for sending a series of commands for each item that matches its criteria.

While not a full replacement for "proper" purpose-built Lich scripts, it is still frequently useful.

## I don't want to read docs, can you give me some examples?

* No rogues around?  Move all your boxes to your locker:  
  `;foreach box in inv; move to locker`

* Own a gemcutter and want to cut all the gems in a container?  
  `;foreach gem in red sack; move to gemcutter; turn gemcutter; waitrt; move to blue sack`

* Getting boxes picked?  
  `;foreach box in inv; get item;give item to Rogue;waitfor Rogue offers you;accept item`  
  *(This'll only work if you're fast enough to empty boxes while the rogue is picking)*

* Checking for orb gems?  
  Sorcerer: `;foreach gem in queue bag;get item;prep 704;cast item;pause;put item in reject bag`  
  Bard using `;loresing`: `;foreach gem in queue bag;get item;;loresing purpose noun;pause;put item in reject bag`  
  While the script is paused, put the gem in the orb bag if it's an orb.  ;unpausing it will put it in the reject bag if you're still holding it.

* Bulk reading scrolls?  
  `;foreach scroll in inv; read item`

* Accidentally move a bunch of items to the wrong container using foreach?  
  `;foreach in last; move to container` *(back where they started)*  
  *or* `;foreach in last; move to correct backpack`
  
* Turning in gems or herbs for a bounty?  
  `;foreach name=uncut diamond in inv;get item;sell item`
  
## That's nice.  But what can it REALLY do?

Usage: **;foreach** _[what]_ **in/on** _target_ [**;** _command1_**;** _command2_**;** ...]  
*or:*  **;foreach** _[what]_ **in/on** _target_ [**/** _command1_**/** _command2_**/** ...]  
*or:*  **;foreach** _[what]_ **in/on** _target_ [**|** _command1_**|** _command2_**|** ...]

### What

_what_ allows you to filter the types of items `foreach` acts on.  It can be one of the following :

* **type=**_pattern_   or  **t=**_pattern_  or  simply _pattern_   
   You can specify any type of item that Lich knows about.  Examples include 'gem', 'wand' and 'scroll' 
   (which includes things like palimpests and other 'alternate names' for scrolls).  `;foreach` by itself will show all
   the known types.  `;foreach in inv` with no commands will show the types of all items it finds.
   
   **Note:** This is only as accurate as Lich's own type data.  You may be able to get more accurate type data by downloading and running the `gameobjadd` script at startup.

* **noun=**_pattern_   or  **n=**_pattern_   
   You can specify items that have a specific noun.  This must be an exact match (unless using wildcards).
   
* **name=**_pattern_   or  **m=**_pattern_   
   You can specify items that have a specific name.  This may or may not excludes the article, dpeend
   
* **fullname=**_pattern_   or  **f=**_pattern_   
   You can specify items that have a specific full name.  This is like `name=pattern`, but includes things like long 
   descriptions.  See `;foreach in inv` to get a list of item full names.
   
_pattern_ can be:
  * `one or more words with including spaces`: Must be an exact match
  * `*use*asterisks*for*wildcards`: Asterisks function as wildcards.  `name=*snowflake-cut*` will find anything with 
    'snowflake-cut' anywhere in the name, for instance.
  * `this,or that`: Match any one of the options separated by commas.
  * `blue sapphire,emerald,*diamond,*emerald,uncut ruby`: You can mix the above.
  
### Target

_target_ tells ;foreach where you want to find the items.  You can specify any container exactly like you'd specify it to
a command in game, but there's also a few special options:

* **inv** or **inventory**  
  Examines the contents all containers in your inventory.
  
* **floor** or **ground** or **room**  
  Examines items on the ground.  *The items, not their contents*
  
* **locker** or **loot**  
  Examines the contents of containers that are on the ground.  Which, coincidentally, includes things like the locker
  containers.  It *also* includes the contents of disks, so keep that in mind.
  
* **worn**  
  Examines items in your inventory, *not* their contents.  Want to register everything you are wearing?  
  `foreach in worn; remove item;register item;wear item`
  
* **last**  
  Last is a set of items that matches whatever the last run of foreach found.  You can use this to quickly undo if you
  manage to flub something up, like accidentally moving all of your inventory to your locker rather than just your boxes.
  
  If you specified a _what_ (above section), it will further restrict the list of what foreach is working on   
  
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
is the first command in the list, an implicit `get item` will be automatically added provided that the command refers to
the item in some way.  These commands currently are: **drop**, **place**, **sell**, **put**

In addition, the following convenience shortcuts exist:

* **move** _[what]_ **to** _where_  
  Shorthand for getting _what_ and then putting it in _where_.  If you omit _what_, 'item' is assumed.
  
  **mv** is shorthand for **move**
  
  Newer versions of foreach will use `_drag` (mimicking Stormfront drag-and-drop functionality) to reduce this from two commands to one when they think it is safe to do so.

* **fastmove** _[what]_ **to** _where_  
  This is a somewhat faster version of the above, but it's more prone to failing if you have oddly scripted containers
  involved or tasks that involve roundtime.  It utilizes typeahead -- namely sending the GET and PUT portions at the
  same time.
  
  This can be shortened as **fastmv**, **fmove** or **fmv**

  Newer versions of foreach will use `_drag` (mimicking Stormfront drag-and-drop functionality) to reduce this from two commands to one when they think it is safe to do so.

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
  Equivalent to `put item in container`.  Returns whatever item Foreach is currently processing to whereever it originally found it.
  
#### Control
  
* **echo** _message_  
  Echos the specified text to your client.  Not sent to the game.
  
* **pause**  
  Pauses ;foreach.  This is particularly useful if you're using ;foreach to examine a set of items and making decisions
  about them.  For instance, you can write a script to test gems for orbs, pause after casting/singing, and then manually
  move the gem to the correct container based on the result before unpausing foreach to continue to the next gem.
  
* **waitrt**  
  Waits for roundtime to finish.  *Note that this will wait **forever** if no roundtime is incurred*, just like Lich's 
  `waitrt` function.
  
* **waitfor** _text_  
  Waits to see the specified bit of text.  Just like Lich's `waitfor`, except only one string of text can be matched.
  Matching occurs in text mode (e.g. not the raw XML stream)
  
* **waitre** _pattern_  
  Like **waitfor**, but accepts a regular expression instead.  
  
* **waitmana** _amount_ or **waitmp** _amount_  
  Waits until you have at least this much mana.
  
* **waithealth** _amount_ or **waithp** _amount_  
  Waits until you have at least this many health points.
  
* **waitspirit** _amount_ or **waitsp** _amount_  
  Waits until you have at least this much spirit.
  
* **waitstamina** _amount_ or **waitst** _amount_  
  Waits until you have at least this much stamina.
  
    
  
  
