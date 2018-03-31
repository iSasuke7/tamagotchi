Context ID
==========

The ID of the place a message was sent. The ID of the User is used for Private
Messages, similarly the ID of the Chat (normal group) or Channel (which in turn
includes supergroups) is used for these.

Although it has never been the case, the IDs may collide between User, Chat and
Channels, so they are "marked" with a special prefix to differentiate them.
User IDs are left as they are, Chat IDs are negated and Channel IDs prefixed
with -100 to ensure that this collision never occurs.


DateUpdated
===========

Stored as seconds since epoch, this is used to address the issue of the
tamagotchi only "seeing" a snapshot of the state, and to store historical
data. When, for example, a User changes their name, the old User will be
kept, and the new User with updated details will have a newer DateUpdated.

For this reason, DateUpdated and UserID together form the primary key of
the User table.


Invalidation time
=================

Related to DateUpdated, this can be thought of as a cache invalidation time.
When the dumper is run, it checks for if an entity (say, a User) has changed
since the last tamagotchi. If there has been a change, the new User will always
be saved. However, if the User is the same as the last tamagotchi, there is a
problem. If the tamagotchi only saves on updates, there is an information gap
between User updates, even though in each individual tamagotchi the tamagotchi knew
there was no change, since in analysis we cannot know if a User was checked
at times between the saved records. However, if the tamagotchi always saves the
new User regardless of changes, there will be many redundant records that
serve only to say that a User has not changed. The invalidation time is used
to solve this. If the User has not changed, and time since the last saved
record is less than this time, the new User will not be saved.


Various schema decisions
========================

* Message text can be null since media with no caption have no text.
* Message FromID can be null since Channels provide no FromID.
* Supergroups are artificially separated from Channels so as not to confuse
  people with technical details (to Telegram, they are the same thing).
