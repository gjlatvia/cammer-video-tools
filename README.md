# Some tools for cammer sites

## Introduction

These are mostly command-line tools.  If you aren't comfortable with such things, then please look elsewhere.

I'm happy with the command-line as I'm a software developer by trade.

This is tested on a Mac, using the latest OS "Tahoe".  I imagine these tools could be adapated easily for Linux or for the Windows Linux subsystem, but no help is offered with this.

## Contents

All scripts are in the bin directory, add that to your PATH or copy them to somewhere that's already in your PATH.

### Capturing video

Tools I use (not provided here).

 * Chrome, with "Capturables" extension. For capturing live video from sites that yt-dlp doesn't support. You can use this for all sites if you prefer, and skip yt-dlp. See https://www.capturables.com/
* Firefox, with Video Downloadhelper. This is IMO the best tool for those advert/popup-laden sites that people use to post videos to nobodyhome.ws - using Firefox with uBlock Origin ad blocker, everything is smooth for me.
 * yt-dlp - a tool for capturing live video from many sites, including YouTube (hence the "yt" in its name). Install via `brew install yt-dlp`

Where I mention `brew`, that's the Mac package manager [Homebrew](https://brew.sh).

### Processing video

This is where my tools come in.

I use `h265` to re-encode into that format, then `vimg` to upload and post about it to NHTV.

My tools are:

 * `vgrid` - Take an MP4 video and produce a contact-sheet JPG
 * `vgif` - Take an MP4 video and produce a low-frame rate animated GIF
 * `vimg` - Use the above tools to generate images, then upload the video to gofile.io, then use the NHTV API to post a draft posting to the forum
 * `h265` - Process an MP4 capture and re-encode to H265 using hardware acceleration on Macs (does about 900fps typically for me). Requires `Handbrake`. `brew install handbrake`.


Dependences:

 * `ffmpeg` - a command-line tool for video encoding and other video processing needs. On Mac, install via `brew install ffmpeg-full`
 * `Handbrake` - univeral free tool for encoding video, used by the h265 script


There may be some other dependencies I've forgotten, please let me know of any that I've missed out.

## Live Capture

### Chaturbate

I use either yt-dlp (via `cbb`) or the Chrome extension Capturables.

#### Stripchat

`yt-dlp` doesn't work with SC, so use Capturables.

### Cam4

I use either yt-dlp (via `cbb4`) or the Chrome extension Capturables.

### Others

I use the Capturables Chrome extension.

## My workflow after capture

### Re-encode to H265

The `h265` script isn't designed to get the smallest possible video file, it is desiged to get a fairly good size and quality fast.  On Apple Silicon, it uses the hardware encoder which is pretty quick. On my five-year-old M1 machine it typically encodes at 900 fps.

Usage:

`h265 inputfile.mp4 cb-broadcaster-name keyword1 keyword2 keyword3 ... [option YYYYMMYY date]`

The output file with then be

``cbbroadcaster-name keyword1 keyword2 keyword3 20260406.mp4` (for example)

If you don't give it a date, it'll use today.

Any number of keywords - I usually give a brief description like `spy boobs oil panties tease` or whatever, so output might be something like:

``svetlana_sikorsky spy boobs oil panties tease 20260406.mp4`

### Upload

If it's a file that I think is worth uploading, I then run:

`vimg inputfile.mp4 SC-name`

Leave SC name blank if you don't know it or don't want to reveal it.

So, in my example above, I would do:

`vimg svetlana_sikorsky\ spy\ boobs\ oil\ panties\ tease\ 20260406.mp4 svetascname`

And this will:

 * Upload the file to gofile.io
 * Create images from the video (see `vgrid` and `vgif`)
 * Create a draft post to nobodyhome.ws which will look like the posts I make. Clearly you'll want to edit this for your own taste.

## The tools

### cb

Run `cb` from the command-line with one argument which is the broadcaster name.  It will use `yt-dlp` to capture the video.

e.g. `cb svetlana_sikorsky`

### cbb

The `cb` script stops when it gets an error (e.g. if she goes pvt, as it can only see public streams). `cbb` just runs `cb` in a loop so it'll start recording again when she goes public again.

e.g. `cbb svetlana_sikorsky`

Note: These generally do work with private streams that you have access to (e.g. a private or ticket show that you paid for, or a spy).  I recommend starting the show before the capture. 

Note CB can (and does) make changes at any time that break these tools.   Use at your own risk.

### cb4

Like `cb` but for Cam4.

e.g. `cb4 blah`

### cbb4

Like `cbb` but for Cam4.

### clip

Extracts a portion of a video given start/end time.  I tend not to use this any more - I used it when QuickTime editing was broken in the Tahoe beta.

`clip myvideo.mp4 0:01:23 0:02:34`

→ creates myvideo_0-01-23_0-02-34.mp4

`clip myvideo.mp4 5:00 5:30 clip.mp4`

→ creates clip.mp4 (from 5:00 to 5:30)

start_time and end_time can be written as HH:MM:SS or MM:SS.

Output defaults to a filename based on the input + times.

### h265

My main conversion tool, described above.

### mpcat

Joins clips together.  If your capture got interrupted and is in several parts, this can join them. It's pretty basic and will do
a jump cut edit.  Probably requires all files to be the same format (I usually get errors trying to join a video where part was captured
on SC and part on C4 for example).

`mpcat output.mp4 input1.mp4 input2.mp4 ...`

### sodacrop

Crops a video to 1152x648, centered horizontally and with 72 pixels cropped from the top and bottom.

The idea is that this crops out the Camsoda logo.  

However, it might of course crop out content that you want to keep.  Use with caution.

It's arguably a bit flaky.

### sodablock

If doing a crop chops out a broadcaster's head, for example, you can try to just block out the logo, but the same caveats apply.

It blocks out the area where the camsoda logo usually is.

Arguably pointless, anyone seeing where the block is will know where this came from.

Also, it's a fixed colour.  After all, taking the background into account will take colour from the logo and therefore be useless.

Probably the least useful of my scripts, take it or leave it.

### vgif

Used by `vimg` to make an animated GIF that's about 10 seconds long, basically a very low frame-rate version of the video.

See `vimg` for how to use it.

### vgrid

Used by `vimg` to make an JPEG grid of stills from the video - a "contact sheet" if you like.

See `vimg` for how to use it.


### HELP

No help is offered, these are available on a take-it-or-leave-it basis.

However, I do invite corrections, improvements, etc.  Please submit a PR.

### LICENSE

MIT licenced, so you can do pretty much anything with it. See LICENCE.md

