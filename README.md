# python-dvr
Python library for configuring a wide range of IP cameras which use the NETsurveillance ActiveX plugin
XMeye SDK

## DeviceManager.py
DeviceManager.py is standalone tkinter and console interface program souch as original DeviceManager.exe
it possible work on both systems - if no TK - it starts with console interface

## DVR-IP, NetSurveillance  or "Sofia" Protocol
The NETSurveillance ActiveX plugin uses a TCP based protocol refered to simply as the "Digital Video Recorder Interface Protocol" by the "Hangzhou male Mai Information Co".

There is very little software support or documentation other than through tools provided by the manufacturers these cameras, which leaves many configuration options inaccessible.

*Command and response codes can be found here:*

https://gist.github.com/ekwoodrich/a6d7b8db8f82adf107c3c366e61fd36f

## Basic usage

```python
from dvrip import DVRIPCam
from time import sleep

host_ip = '192.168.0.100'

cam = DVRIPCam(host_ip, "admin", "")
if cam.login():
	print("Success! Connected to " + host_ip)
else:
	print("Failure. Could not connect.")

time = cam.get_time()
print("Camera time:", time)

# Reboot test
cam.reboot()
sleep(60) # wait while camera starts

# Login again
cam.login()
# Sync camera time with PC time
cam.set_time()
cam.close()
```

## Camera modes/settings

```python
params = cam.get_info("Camera")
print("Main params: ", params)

# Get videochannel color parameters
colors = cam.get_info("AVEnc.VideoColor.[0]")

# Change picture title
cam.channel_title(["Backyard"])

# Change IR Cut
cam.set_info("Camera.Param.[0]", { "IrcutSwap" : 0 })
```


## Upgrade camera firmware

```python
# Optional: get information about upgrade parameters
print(cam.get_upgrade_info())

# Do upgrade
cam.upgrade("General_HZXM_IPC_HI3516CV300_50H20L_AE_S38_V4.03.R12.Nat.OnvifS.HIK.20181126_ALL.bin")
```

## Acknowledgements

*Telnet access creds from gabonator*

https://gist.github.com/gabonator/74cdd6ab4f733ff047356198c781f27d
