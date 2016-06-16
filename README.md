Streaming with ffmeg
===================
# streaming using script FFMEG work fine with  twitch and livecoding


Work fine in Ubuntu **15.04** - **15.10** and **16.04**

Question in [AskUbuntu](http://askubuntu.com/questions/784981/alternatives-obs-open-broadcaster-software/786110#786110)

***1*** Install ***ffmpeg****
    
    sudo apt-get update
  
    sudo apt-get install ubuntu-restricted-extras
  
    sudo apt-get install ffmpeg
  
  
  
***2*** Create file.sh*
  
    script.sh
  
 ***3*** Paste code*

    INRES="1366x768" # input resolution
         OUTRES="1366x768" # output resolution
         FPS="30" # target FPS
         GOP="30" # i-frame interval, should be double of FPS, 
         GOPMIN="15" # min i-frame interval, should be equal to fps, 
         THREADS="4" # max 6
         CBR="1000k" # constant bitrate (should be between 1000k - 3000k)
         QUALITY="ultrafast"  # one of the many FFMPEG preset
         AUDIO_RATE="44100"
         STREAM_KEY="***************************"
         #SERVER="rtmp://live.twitch.tv/app/"
         #SERVER="rtmp://live.justin.tv/app/"
         SERVER="rtmp://usmedia3.livecoding.tv:1935/livecodingtv/"
         
         ffmpeg -f x11grab -s "$INRES" -r "$FPS" -i :0.0 -f alsa -i pulse -f flv -ac 2 -ar $AUDIO_RATE \
           -vcodec libx264 -g $GOP -keyint_min $GOPMIN -b:v $CBR -minrate $CBR -maxrate $CBR -pix_fmt yuv420p\
           -s $OUTRES -preset $QUALITY -tune film -acodec libmp3lame -threads $THREADS -strict normal \
           -bufsize $CBR "$SERVER$STREAM_KEY"

  
  
***4*** Assign Permissions*

    sudo chmod +x script.sh



***5*** Execute Script*

    sudo sh script
