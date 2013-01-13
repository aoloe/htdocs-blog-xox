---
title:Low-tech time-laps movie
date:29.10.2012
---
I've been looking for a way to stream from our last week translation sprint in Brescia and I've finally found a very simple and cheap solution.

## Specifications:

- An old laptop (my old eeePC with Debian Testing)
- A cheap Webcam (if possible not the built-in one)
- fswebcam, ncftpput, a cheap web space and ffmpeg
- fswebcam, and ffmpeg
- ncftpput, a cheap webspace if you want to "stream" the images

## Capturing the images

- I wrote a small script that uses fswebcam to take a snapshot every n seconds:

    \#!/bin/bash
    fswebcam -d v4l2:/dev/video1 -i 0 -r 640x480 -l 5 --jpeg 30 --no-banner capture.jpg --exec ./upload.sh

  - I'm using the external webcam (that's why it's video1 and not viedo0)
  - 640x480 is already pretty big for what i need...
  - I want a snapshot every 5 seconds
  - I want the snapstho to be very low quality (30% jpeg quality)
  - The snapshot is saved into the capture.jpg file
- A second script (<kbd>upload.sh

    \#!/bin/bash
    ncftpput yourserver webcam/ capture.jpg
    now=`date +%Y%m%d_%H%M_%S`
    thishour=`date +%Y%m%d_%H`
    cp capture.jpg capture_$now.jpg

  Of course, if you only want to have a timelaps movie, without a live view, you don't have to upload the file to the webserver!
- I've tried to put the webcam as high as possible!

## Showing the stream

You simply have to add an HTML file on your webserver (in my case <kbd>/index.html</kbd>) that shows the uploaded images and reload it through some javascript at the same interval as the snapshot are taken:

    &lt;html&gt;
    &lt;head&gt;
    &lt;title&gt;webcam&lt;/title&gt;
    &lt;script&gt;
    var url = "capture.jpg";
    function srcreload() {
        var image = document.getElementById("capture");
        var random = "?" + Math.random();
        image.src = url + random;
        setTimeout(srcreload, 5000);
    }

    setTimeout(srcreload, 5000);

    &lt;/script&gt;
    &lt;/head&gt;
    &lt;body&gt;
    &lt;title&gt;webcam&lt;/title&gt;
    &lt;img id="capture" src="capture.jpg" /&gt;
    &lt;/body&gt;
    &lt;/html&gt;

If you're on a shared hosting, make sure that that you're not producing too much traffic upstream (your uploads) or downstream (your visitors looking at your images).

## Creating the time-laps movie

Once your event is over and you have all the images in a directory:

    mencoder "mf://capture*.jpg" -o capture.avi -mf fps=16 -ovc lavc -lavcopts vcodec=mjpeg

There are lot of ways to create the movie. Just pick one!

With ffmpeg, I've finally converted the AVI movie into WEBM for uploading it to the web. It will be online very soon (I want to add some titles...)
