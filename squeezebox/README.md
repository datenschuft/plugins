# Squeezebox

## Requirements

A properly installed and configured Logitech Media Server is required.

## Supported Hardware

Tested with:
* Logitech Squeezebox Radio
* Squeezelite on Windows, Mac OS X and Linux

Should work with other Squeezebox players as well - please let me know!

## New in version 1.3.0

* The plugin is now a Smart Plugin. You can change the logging level in logging.yaml
* When the end of a playlist is reached the plugin now changes the mode correctly to "stop" 
* When playing a radio station the play mode is now correctly set to "play"
* When starting playing by adding and playing a playlist the play mode is now correctly set to "play"
* The player_id in your conf file is now searched not only in the parent item but also 2 levels further up

## Configuration

### plugin.conf

```
[squeezebox]
    class_name = Squeezebox
    class_path = plugins.squeezebox
#    host = &lt;server&gt;
#    port = &lt;port&gt;
```

Description of the attributes:

* __host__: IP or hostname of the Logitech Media Server if not local
* __port__: Port number of the Logitech Media Server if not 9090

### items.conf

You can use all commands available by the telnet-interface.

For a explanation of all available commands see http://&lt;server&gt;:9000/html/docs/cli-api.html

Up to four identifier strings are used:
* __squeezebox_playerid__: used in the parent item to allow using &lt;playerid&gt; placeholder in the children
* __squeezebox_send__: used to set the item
* __squeezebox_recv__: used to get notifications when running
* __squeezebox_init__: used to query the item at start-up
 
Fields:
* __&lt;playerid&gt;__: gets replaced by the player-id set in the parent item
* __{}__: the value of the item is written to this placeholder (don't use if a fixed/no value is required)

You should verify all your commands manually by using the telnet-interface on port 9090.

```bash
telnet &lt;server&gt;:&lt;port&gt;
listen 1
&lt;playerid&gt; name ?
...
```

```
[Squeezebox]
  squeezebox_playerid = your-players-ID-in-here

  [[Name]]
    type = str
    visu = yes
    squeezebox_send = &lt;playerid&gt; name {}
    squeezebox_recv = &lt;playerid&gt; name
  [[IP]]
    type = str
    visu = yes
    squeezebox_recv = player ip &lt;playerid&gt;
  [[Signal_Strength]]
    type = num
    visu = yes
    squeezebox_recv = &lt;playerid&gt; signalstrength

  [[Power]]
    type = bool
    visu = yes
    squeezebox_send = &lt;playerid&gt; power {}
    squeezebox_recv = &lt;playerid&gt; prefset server power
    squeezebox_init = &lt;playerid&gt; power
    
  [[Mute]]
    type = bool
    visu = yes
    squeezebox_send = &lt;playerid&gt; mixer muting {}
    squeezebox_recv = &lt;playerid&gt; prefset server mute
    squeezebox_init = &lt;playerid&gt; mixer muting
  [[Volume]]
    type = num
    visu = yes
    squeezebox_send = &lt;playerid&gt; mixer volume {}
    squeezebox_recv = &lt;playerid&gt; prefset server volume
    squeezebox_init = &lt;playerid&gt; mixer volume
  [[Volume_Up]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; button volup
  [[Volume_Down]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; button voldown
    
  [[Play]]
    type = bool
    visu = yes
    squeezebox_send = &lt;playerid&gt; play
    squeezebox_recv = &lt;playerid&gt; play
    squeezebox_init = &lt;playerid&gt; mode
  [[Stop]]
    type = bool
    visu = yes
    squeezebox_send = &lt;playerid&gt; stop
    squeezebox_recv = &lt;playerid&gt; stop
    squeezebox_init = &lt;playerid&gt; mode
  [[Pause]]
    type = bool
    visu = yes
    squeezebox_send = &lt;playerid&gt; pause {}
    squeezebox_recv = &lt;playerid&gt; pause
    squeezebox_init = &lt;playerid&gt; mode
    
  [[Current_Title]]
    type = str
    visu = yes
    squeezebox_recv = &lt;playerid&gt; playlist newsong
    squeezebox_init = &lt;playerid&gt; current_title

  [[Genre]]
    type = str
    visu = yes
    squeezebox_recv = &lt;playerid&gt; genre
  [[Artist]]
    type = str
    visu = yes
    squeezebox_recv = &lt;playerid&gt; artist
  [[Album]]
    type = str
    visu = yes
    squeezebox_recv = &lt;playerid&gt; album
  [[Title]]
    type = str
    visu = yes
    squeezebox_recv = &lt;playerid&gt; title
  [[Duration]]
    type = num
    visu = yes
    squeezebox_recv = &lt;playerid&gt; duration

  [[Time]]
    type = num
    visu = yes
    squeezebox_send = &lt;playerid&gt; time {}
    squeezebox_recv = &lt;playerid&gt; time

  [[Playlist_Index]]
    type = num
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist index {}
    squeezebox_recv = &lt;playerid&gt; playlist index
  [[Playlist_Forward]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist index +1
  [[Playlist_Backward]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist index -1
    
  [[Playlist_Name]]
    type = str
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist name {}
    squeezebox_recv = &lt;playerid&gt; playlist name
  [[Playlist_Save]]
    type = str
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist save {}
  [[Playlist_Load]]
    type = str
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist load {}
  [[Playlist_Load_Internetradio]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist load file:///home/robert/playlists/Internetradio.m3u
    
  [[Repeat]]
    type = num
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist repeat {}
    squeezebox_recv = &lt;playerid&gt; playlist repeat
  [[Repeat_Song]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist repeat 1
  [[Repeat_Playlist]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist repeat 2
  [[Repeat_None]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist repeat 0
    
  [[Shuffle]]
    type = num
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist shuffle {}
    squeezebox_recv = &lt;playerid&gt; playlist shuffle
  [[Shuffle_By_Song]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist shuffle 1
  [[Shuffle_By_Album]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist shuffle 2
  [[Shuffle_None]]
    type = bool
    enforce_updates = true
    visu = yes
    squeezebox_send = &lt;playerid&gt; playlist shuffle 0
```
