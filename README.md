# About
This readme helps you output RTSP stream to a virtual webcam device using FFmpeg. Once you've completed the steps in the document, you can use your preferred video conferencing software to stream the video feed from the camera without granting direct access to the RTSP feed. This can be useful for situations where you want to show outside spaces to friends or family without giving them direct access to the camera's feed.

## Warning
Please note that this document is for informational purposes only and should be used in compliance with all applicable laws and regulations, and in good faith.

**You, alone, are responsible for the consequences of your actions and inactions.**

## Install Motion

First, we need to install `Motion`, which is a software package that can capture and process video streams from cameras.

```bash
sudo apt update
sudo apt install motion
```

## Obtain RTSP credentials

Next, we need to obtain `RTSP` credentials. You can find instructions for doing this on the manufacturer's website or by contacting your sysadmin.

## Configure Motion

After installing Motion, we need to edit its configuration file to add the RTSP stream as a video source. The configuration file is located at /etc/motion/motion.conf, and you can edit it using a text editor such as `nano`.

```bash
sudo nano /etc/motion/motion.conf
```

Add the following lines to the configuration file, replacing `username` and `password` with the credentials you want to use to access the stream:

```
log_file /tmp/motion/motion.log

stream_port 8081
stream_localhost off
stream_authentication username:password
```


```
sudo mkdir /tmp/motion
sudo chown motion:motion /tmp/motion
sudo touch /tmp/motion/motion.log
sudo chown motion:motion /tmp/motion/motion.log
```

## Start Motion

We can now start Motion to capture the video stream.

```bash
sudo service motion start
```

You should now be able to access the camera's video stream from the Ubuntu tower by opening a web browser and navigating to http://localhost:8081. You will be prompted to enter the username and password you configured earlier.

## Install v4l2loopback

To output the DVR's feed to a virtual webcam device, we need to install v4l2loopback. This package creates a virtual video device that you can use as a webcam input in your streaming app.

```bash
sudo apt install v4l2loopback-dkms
sudo modprobe v4l2loopback
```

## Start streaming with FFmpeg

Finally, we can use `FFmpeg` to capture the video stream from Motion and output it to the virtual webcam device.

```bash
ffmpeg -i http://localhost:8081 -pix_fmt yuv420p -f v4l2 /dev/video0
```

This command tells FFmpeg to capture the video stream from Motion and output it to the virtual webcam device `/dev/video0`. You can then select `/dev/video0` as your webcam input in your streaming app.

Note: Make sure to customize the script by replacing `username` and `password` with your own credentials, and adjust the configuration settings as needed for your specific use case.

## [Disclaimer](DISCLAIMER)
**This software is provided "as is" and without warranty of any kind**, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software or the use or other dealings in the software.

**The authors do not endorse or support any harmful or malicious activities** that may be carried out with the software. It is the user's responsibility to ensure that their use of the software complies with all applicable laws and regulations.

## License

These files released under the [MIT License](LICENSE).
