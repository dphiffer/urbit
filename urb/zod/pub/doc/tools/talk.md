# `:talk`

`:talk` messaging interface.

`:talk` is the Urbit appliance for chatter and notifications.
For less sophisticated users, Urbit *is* just `:talk`.  If you
see `:talk` as "like Slack, but distributed," or "like IRC, but
persistent and encrypted," you're not completely wrong.

`:talk` is an unusual messenger in two ways.  One: by default, it
multiplexes all content streams into a single flow.  Most UI
researchers agree that context-switching is cognitively expensive
and leads to surfing the Internet.  (`:talk` is also used for
your system notifications.)

Two: text lines are limited to 64 ASCII bytes, no uppercase.
This restriction is mobile-friendly and reduces the aesthetic
impact of low-quality content.

Messages in `:talk` are called "posts".  Posts go to "stations,"
which are just like IRC or Slack channels.  Any urbit can host or
subscribe to any number of stations. 

`:talk` is not a text-only messenger; it's designed to support
arbitrary content in posts, from URLs to images to long-form
text.  (Only URLs right now.)  However, any message on `:talk`
has to be able to summarize itself in a 64-byte text line.

There are four kinds of station: a write-only "mailbox" for
direct messages, an invite-only "party" for private conversation,
a read-only "journal" for curated content, and a public-access
"board" for general use or abuse.

While there's obviously no central `:talk` server for all of
Urbit, and thus no such thing as a truly global station space,
active Urbit stars cooperate to federate, manage and mirror a
collectively-managed namespace, very like Usenet.  These
"federal" stations are generally public-access boards.

### Quickstart

Let's post something!  At the default `:talk` prompt
```
~tasfyn-partyv:talk: 
```
type the message:
```
~tasfyn-partyv:talk: hello, world.
```
And hit return.  Don't worry, no one but you will see this.  The
`:` means you're posting to yourself.  You'll get the post:
```
~tasfyn-partyv: hello, world.
~tasfyn-partyv:talk: 
```
It's boring to post to yourself.  Let's join a station:
```
~tasfyn-partyv: ;join /urbit-test
```
(`/urbit-test` is a federal station, meaning it's hosted by your
star (for `~tasfyn-partyv`, `~doznec`).  The `/` notation is just 
an abbreviation for `~doznec/urbit-test`.)

You'll see:
```
---------:talk| %porch subscribed to /urbit-test, called `>`
---------:talk| rules of /urbit-test:
---------:talk|   test posts only.  no shitposting.  no pedos/nazis.
       ~doznec> ~tasfyn-partyv admitted to %urbit-test
~tasfyn-partyv:talk>
```
Notice the character assignment - stations you're subscribed to are
assigned [consistent ASCII glyphs](#-station-glyphs), which you'll 
see in the log when you hear from these stations, and on the prompt
when you're talking to them.

Post a line to `/urbit-test`:
```
~tasfyn-partyv:talk> hello, world
```
You'll see, echoed back at you through `~doznec`:
```
~tasfyn-partyv:talk> hello, world
```
And of course, anyone else in `/urbit-test` will see it as well.
But you don't care about `/urbit-test`, so leave it:
```
~tasfyn-partyv:talk> ;leave
```
You'll see:
```
---------:talk| %porch has left /urbit-test, called `>`
```
Everyone else will see:
```
       ~doznec> ~tasfyn-partyv has left %urbit-test
```

Now you're ready to use `:talk` for real!  List the federal
groups currently available with
```
~tasfyn-partyv:talk> ;list
```
For general discussion about Urbit, we recommend `/urbit-meta`.

### Basic usage

#### Input conventions

There are three kinds of inputs you can type at the `:talk`
prompt: lines, URLs, and commands.

A line is 64 bytes of ASCII lowercase and spaces.  If the line
starts with '@', it's an action (IRC `/me`).

The `:talk` interface will let you keep typing past 64 bytes, but
insert a Unicode bullet-point character in an appropriate space
in your post, to show you the prospective linebreak.  Your essay
will be posted in multiple lines.

A URL is any valid URL.  A command is any line starting with `;`.

#### Source annotation

Any post in your flow is shown with its author, together with a
glyph that shows how the post reached you.  A post can reach you
in one of three ways: 

Any post you see reached you in one of three ways.  Either it was
sent directly to just you; to you and others; or to a station you
subscribe to.

Posts to just you are `:`.  Posts to you and others (a multiparty
conversation) are `*`, unless you've bound this conversation to a
glyph.  Posts to a station use that station's glyph.

You can see a list of glyph bindings with `;what`.  Write
`;what >` to see what station `>` is bound to, or
`;what /urbit-test` to see if `/urbit-test` has a binding.

#### Audience selection

Audience selection is important in a multiplexed communicator!
The audience is always shown in your prompt.  If there's a glyph
for it, it's shown as the glyph:
```
~tasfyn-partyv:talk> 
```
Otherwise, the audience is shown in parens:
```
~tasfyn-partyv:talk(~wictuc-folrex) 
```

`:talk` works fairly hard to get the audience right and minimize
manual switching.  But to manually set the audience, the command
is simply `;station` - eg, `;~wictuc-folrex` for a direct post;
`/urbit-test` or `~doznec/urbit-test` to post to a federal
station, `%mystation` to post to a station on your own ship.
For a station bound to a glyph, `;` then the glyph; eg, `;>`.

You can post a line and set the audience in one command, eg:
```
;~wictuc-folrex this is a private message
```

You can configure your audience in a number of ways, which are
applied in priority order.  From strongest to weakest:

- if typing a post, the audience when you started typing.
- if you activated a post (see below), the post you activated.
- if you manually locked the audience (see above), that audience.
- audience of the last post received.
- audience of the last post sent.

You can clear any audience setting layer by moving your cursor to
the start of the line and pressing backspace (whether the line is
empty or not).  Posting a line clears the typing and activation
configurations.

#### Post activation and numbering

Every post can summarize itself in 64 bytes.  But some posts
contain more information, which is not displayed by default.
Displaying this "attachment" is an opt-in operation.  In the
post, it's marked by an underscore `_`, instead of a space,
between source and content.

The conventional example is a URL.  When you post a URL:
```
~tasfyn-partyv:talk> http://foobar.com/moo/baz
```
This will appear in the flow as:
```
~tasfyn-partyv>_foobar.com
```
meaning that `~tasfyn-partyv` posted a link to `foobar.com`,
on the station or conversation whose glyph is `>`.

The effect of activating a post depends on the post.  For a link,
the full URL is shown and (system permitting) put into the OS's
clipboard, or even automatically navigated to.  Even for a text
post, activating shows the full audience, for complex audiences.

Posts in your `:talk` flow are numbered; the numbers are printed
every five posts, as
```
----------[5955]
```
You can specify a post to activate in two ways: by absolute or
relative position.  Absolute position is a direct history number:
```
;5955
```
If you use fewer digits than are in the current flow number, the
high digits are defaulted "deli style" - if the current number is
5955, typing `;3` means `;5953`, and `;140` means `;5140`.  To
actually activate post `3`, write `;0003`.

A unary sequence of `;` characters looks backward from the
present.  `;` activates the most recent post; `;;` the second
most recent; etc.

#### Nicknames

Sometimes you know your Urbit friends by other names, on or
offline.   Use the `;nick` command to assign or look up
nicknames.

`;nick` with no arguments lists all nicknames; `;nick
~tasfyn-partyv` looks up a nickname; `;nick curtis` searches in
reverse; `;nick ~tasfyn-partyv curtis` creates a nickname.
All nicknames must be 14 characters or less, lowercase.

Of course, nicknames are strictly local - like the names on
entries in a phonebook.  Sometimes in a post you want to mention
someone you know by a nickname.  Just type `~curtis`, and `:talk`
will replace it magically with `~tasfyn-partyv` (or beep if no
`~curtis` is bound).

#### Presence

You'll see presence notifications when people enter or leave
stations you're subscribed to.

`;who` lists everyone in all your stations.  `;who station`
lists everyone in that station.

#### Typing indicator

If one or more urbits in your audience is typing, `:talk`'s
presence system will detect it and change the prompt:
```
~tasfyn-partyv [~wictuc-folrex...]> 
```

#### Creating and managing stations

To create your own mailbox, party, journal or board:
```
;create party %myfunparty
;create journal %serious-journal
;create board %bizarre-board
```
etc.

Every form of station has an exception list; to block
`~wictuc-folrex` from your default mailbox `%porch`,
```
;block %porch ~wictuc-folrex
```
To invite people to `%myfunparty`:
```
;invite %myfunparty ~wictuc-folrex, ~sondel-forsut
```
To ban from `%bizarre-board`:
```
;banish %bizarre-board ~wictuc-folrex
```
To appoint a coauthor of `%serious-journal`:
```
;author %serious-journal ~sondel-forsut
```

#### Station Glyphs

Station are assigned out of the list `:|}>`, then
randomly out of it and the sets `-+*.`, ``,=`'^\/``,
`$%&@`, and `{<[]()`, in decreasing order of probabilty. 
Alphanumeric characters and `!#?;~_` are reserved.
