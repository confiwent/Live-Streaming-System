# Live-Streaming-based-on-ffmpeg
This is a tutorial for building a live streaming system by ffmpeg and ffserver

This system is built on the Ubuntu system, the first thing you should do is installing ffmpeg with a command ``` sudo apt-get install ffmpeg```.

This system consists of four elements: 1) input source; 2)codec; 3) video server; 4) player.

## Video server configuration
ffserver is a powerful and convienient tool for transcoding and distributing video streaming. You can click the following website for more details:[ffserver doc](https://trac.ffmpeg.org/wiki/ffserver)

You just need a simple command to run the ffserver: ``` ffserver -f /etc/ffserver.conf``` with a configuration document customized by yourself. The example of mine shows above. 

Note that the ```.ffm``` file, defines you video cache for live stream on the server, connects the coded video stream from the codec.

## Connecting the input sources and coding
Also, the command for receiving video raw data, coding and feeding to server is simple:
```ffmpeg <inputs> <coding parameter> <feed URL>```

The parameter "<feed URL>" has got the following form:
```http://<ffserver_ip_address_or_host_name>:<ffserver_port>/<feed_name>```
  
If you want to connect a webcam directly, you should just repalce the ```inputs``` by the webcam path, such as ```/dev/video0```.

A example for this operate:
```ffmpeg \
    -f v4l2 -s 320x240 -r 25 -i /dev/video0 \ ## video inputs
    -f alsa -ac 1 -i hw:0 \ ## audio inputs
    -c:v libx265 -crf 28 -c:a aac -b:a 128k\ ##coding
    http://localhost:8090/feed1.ffm  ## output ```
