# Golang-Media-Server

HLS media streaming server built in Go. 

To Run:

cd to folder;
run >go run main.go;
this will establish server on port:8080;
use link: https://hls-js-dev.netlify.app/demo/?ref=hackernoon.com to test HLS streaming over the server (make sure to change format to HLS)
link to server to enter in demo test: http://localhost:8080/outputlist.m3u8


Next Steps: 

MP3 file in assets folder was transcoded to HLS appropriate format using FFMPEG in terminal manually.
  - automate this with a bash script 
  OR
  - node-fluent-ffmpeg is a nodeJS library that extends FFMPEG functionality to a Node application (golang binded ffmpeg libraries also can be found online) if cant run bash script with Windows
  
System Design:

Provision a server with FFMPEG installed, you want one with high CPU resource here. Provision another empty server for playback, this will be a simple API.

Create a web server that handles video upload. Deploy to the server created in 1.

On upload finished, the web server can fire an async worker that will supervise transcoding process.

The async worker will initiate FFMPEG (with system call) to start transcoding the uploaded video into different web streaming formats that you need. You usually want two formats, DASH (for Android and Web) and HLS (for iOS). Generally the outputs of this step are the video/audio segments and the manifest file.

When FFMPEG has finished transcoding the files, the worker then can upload the results into S3 or any other object storage (and maybe the raw video as well for archiving). Then notify playback server that there is a new video ready for streaming, the S3 url pointing to the manifest files included and recorded.

Clean up the video files and the transcoding artifacts, you no longer need them since they are uploaded to S3.

When a client wants to playback a video, it asks playback server for the video manifest. If it's Android, return the DASH manifest. If it's iOS, return the HLS manifest.

In client, you can simply load the manifest into the supported video player (e.g. Android - Shaka player, iOS - the native video player).
