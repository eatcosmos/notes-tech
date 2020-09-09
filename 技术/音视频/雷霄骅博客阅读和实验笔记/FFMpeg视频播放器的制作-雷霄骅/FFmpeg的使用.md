ffmpeg.exe用于视频的转码

码率是什么？

http://ffmpeg.org/ffmpeg.html

ffmpeg  -ss 5 -i cuc_ieschool.mkv -r 5 -t 5 -b:v 100k -s 1280x720 -ar 8000 cuc_ieschool.mp4 # -ss要写在-i前面



