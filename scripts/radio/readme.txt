
This directory and its initial contents are created by the simulation each time it is run and this directory is missing.  The purpose of this directory is to hold scripts that are processed by the simulation's Race Radio when certain actions are performed.  There is a separate script file for each action:

Launch.txt
StartDriving.txt
StopDriving.txt
StartSpotting.txt
StopSpotting.txt

The Launch script is loaded and executed each time the simulation is launched.

The StartDriving script is loaded and executed each time you start driving your car.

The StopDriving script is loaded and executed each time you get out of your car (even though your car might be left in, or returned to, your pit stall).

The StartSpotting script is loaded and executed each time you press the [Start Spotting] button, thus declaring your intent to spot for your team's driver.

The StopSpotting script is loaded and executed each time you press the [Stop Spotting] button, thus declaring your intent to no longer spot for your team's driver.



Each script can contain commands that alter the configuration of your Race Radio.  You are free to edit each of these scripts to configure your Race Radio as you wish.  Some of the default scripts might be empty.  Use Notepad (or a similar plain-text editor) to create or modify the scripts.  Generally, you can just double-click on a script file to edit it.

Put each command within a script on a separate line.
The Race Radio will only process around the first 4000 characters in a script, which should be more than sufficient for any reasonable script.



The following commands can be placed within a script.

add @channel
remove @channel
scan on
scan off
transmit @channel
mute all
unmute all
mute @channel
unmute @channel

Where it says "@channel", you can use one of the channel names that are predefined on your radio, or use a custom channel name.  Channel names are at most 14 characters long, and can contain alphabetic characters (which will be converted by the radio to upper-case) and numeric characters.  All other characters are converted to the - character.  Channel names are not case sensitive, so do not worry about trying to match the casing of any of the channel names.

It's perfectly OK to "add" a channel that already exists, or "remove", "transmit", "mute", or "unmute" a channel that does not exist.  The command is just ignored.

add @x		- Adds the channel to your radio.  Your radio can have up to 32 channels.
remove @x	- Removes the channel from your radio.  You can not remove predefined channels.
scan on	 	- Turns your radio's scanning function on.
scan off	- Turns your radio's scanning function off, though some predefined channels are always scanned.
transmit @x	- Sets your radio's transmitter to the specified channel, if you are allowed to transmit on that channel.
mute all	- Mutes all channels that are allowed to be muted (some predefined channels may not be muted).
unmute all	- Unmutes all channels.
mute @x		- Mutes this specific channel (some predefined channels may not be muted).
unmute @x	- Unmutes this specific channel.



The predefined channels on your radio (as of this writing) are:
@ADMIN
@RACECONTROL
@DRIVERS
@CLUB
@TEAM
@ALLTEAMS
@PRIVATE
@SPECTATORS

None of these predefined channels may be removed.  Some other configuration commands are ignored by certain channels.  For example, you are not allowed to mute @RACECONTROL, and @RACECONTROL will always be scanned even if you've turned scanning off.

@ADMIN		- Anyone that has admin rights on the server may use this channel.
@RACECONTROL	- Race control and admins may use this channel.
@DRIVERS	- Only people currently driving a car (but not spectators) may transmit on this channel.
@CLUB		- Anyone on your club may use this channel.
@TEAM		- This channel is restricted to only you and your teammates.
@ALLTEAMS	- All members of all teams may transmit on this channel (but not spectators).
@PRIVATE	- Race control or an admin may speak directly to just you on this channel.
@SPECTATORS	- Anyone in the session as a spectator may use this channel.



Since you might wish to configure your radio differently depending whether certain conditions are met, each command may be prefixed with a conditional test.  Without such a prefix, the command is always executed.


To check whether or not this is a Team Driving session (also known as a Driver-Change session):
if dc
if notdc

To check whether or not this is a Heat Racing session (the overall event, not whether the specific session is a Heat race):
if heats
if notheats

To check whether or not this is either a Qualify or a Race session:
if qual_or_race
if notqual_or_race

To check whether or not you are a spectator in the session:
if spectator
if notspectator

To check whether or not you currently have any teammates connected with you in the session:
if team
if noteam


For example,

	if dc mute @allteams

would be applied only in sessions with driver changes,

	if qual_or_race mute @allteams

would be applied only if the current session is either Qualify or Race,

	if notheats mute @allteams

would be applied only if this is not a Heat racing event,

	if notdc mute @allteams

would be applied only in sessions that do not allow driver changes, and

	mute @allteams

would be applied in all sessions.



In addition to the global scripts, you can have team-specific scripts.  The team-specific scripts are named similarly, but have "Team_#_" prepended, with "#" replaced by the teamID.  For example, if your teamID is 10001, then the file name for the Launch script for your team would be as follows.
Team_10001_Launch.txt

If both a global script and a team-specific script exist (for example Launch.txt and Team_10001_Launch.txt), then the two scripts are combined, with the global script first and the team specific script second, and the combined script is executed.  The combined script must be within the size limit for a radio script.



The default global scripts, as of this writing, are as follows:

Launch.txt
if notdc transmit @allteams
if dc transmit @team

StartDriving.txt
if dc mute @allteams
if qual_or_race transmit @drivers
if qual_or_race mute @allteams

StopDriving.txt
if dc unmute @allteams
if qual_or_race unmute @allteams

StartSpotting.txt
transmit @team
mute @allteams
mute @drivers

StopSpotting.txt
unmute @drivers
unmute @allteams
if notdc transmit @allteams


