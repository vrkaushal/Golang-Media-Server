# Golang-Media-Server

HLS media streaming server built in Go. 

To Run:

cd to folder
run >go run main.go
this will establish server on port:8080
use link: https://hls-js-dev.netlify.app/demo/?ref=hackernoon.com to test HLS streaming over the server (make sure to change format to HLS)


Next Steps: 

MP3 file in assets folder was converted to HLS appropriate format using FFMPEG in terminal manually.
  - automate this either with a bash script 
  OR
  - node-fluent-ffmpeg is a nodeJS library that extends FFMPEG functionality to a Node application if cant run bash script with Windows
