
---
title: Test or Select a Media Device
description: 
platform: Windows
updatedAt: Wed Apr 10 2019 07:02:07 GMT+0000 (UTC)
---
# Test or Select a Media Device
## Introduction

Agora supports the media device test and selection feature, allowing you to check if a camera or an audio device (a headset, microphone, or speaker) works or connects properly to the SD-RTN.

You can use the media device test and selection feature in the following scenarios:

- A host self-checks before starting a live broadcast.
- An online user checks if the media device works.

## Implementation

### Microphone Test

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

// Enumerate all audio devices.
AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpRecordingDeviceCollection = (*lpDeviceManager)->enumerateRecordingDevices();

UINT lCount = (UINT) lpRecordingDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpRecordingDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select an audio capture device.
lpDeviceManager->setRecordingDevice(strDeviceID); // device ID chosen

// Implement the audio volume callback.
virtual void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume) {
        (void)speakers;
        (void)speakerNumber;
        (void)totalVolume;
    }

// Enable the audio volume callback.
rep.enableAudioVolumeIndication(1000, // Callback interval (ms)
	10 // Smoothness
	);

// Start the audio capture device test.
(*lpDeviceManager)->startRecordingDeviceTest(1000);

// Stop the audio capture device test.
(*lpDeviceManager)->stopRecordingDeviceTest();
```

### Camera Test

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

// Enumerate all video capture devices.
AVideoDeviceManager* lpDeviceManager = new AVideoDeviceManager(lpRtcEngine);
IVideoDeviceCollection *lpVideoDeviceCollection = (*lpDeviceManager)->enumerateVideoDevices();

UINT lCount = (UINT) lpVideoDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpVideoDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select a video capture device.
lpDeviceManager->setDevice(strDeviceID); // device ID chosen

// Start the video capture device test. If it succeeds, you will see a preview of the screen.
(*lpDeviceManager)->startDeviceTest(view); // Pass a window handler to it.

// Stop the video capture device test.
(*lpDeviceManager)->stopDeviceTest();
```

### External Playback Device Test

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpPlaybackDeviceCollection = (*lpDeviceManager)->enumeratePlaybackDevices();

UINT lCount = (UINT) lpPlaybackDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpPlaybackDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select an audio playback device.
lpDeviceManager->setPlaybackDevice(strDeviceID); // device ID chosen

// Start the audio capture device test, and you will hear the audio from the external device.
// You do not need to call `enableAudioVolumeIndication`, because you can directly hear the audio.

#ifdef UNICODE
	CHAR wdFilePath[MAX_PATH];
	::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
	(*lpDeviceManager)->startPlaybackDeviceTest(wdFilePath);
#else
	(*lpDeviceManager)->startPlaybackDeviceTest(filePath);
#endif

// Stop the audio capture device test.
(*lpDeviceManager)->stopPlaybackDeviceTest();
```

### API Reference

* [`enableAudioVolumeIndication`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a59ae67333fbc61a7002a46c809e2ec4f)
* [`enumerateRecordingDevices`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ea4f53d60dc91ea83960885f9ab77ee)
* [`setRecordingDevice`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a723941355030636cd7d183d53cc7ace7)
* [`enumeratePlaybackDevices`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aa13c99d575d89e7ceeeb139be723b18a)
* [`setPlaybackDevice`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ee23eae83165a27bcbd88d80158b4f1)
* [`startRecordingDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a9e732d31f179a90d388998f5b86ebf06)
* [`stopRecordingDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a796e7b8a58eb303f18f04e1e9d12a94b)
* [`enumerateVideoDevices`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#aef51744162ec544abf2aaf0488ca062d)
* [`startDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#ac148cafcb191841fd4aa7f5b6166b16d)
* [`stopDeviceTest`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#ae3fe9f7ad1ddf4d5cda5e30d14b9d321)

## Considerations
- You need to ensure that the audio and video capture devices are not used by other applications when testing. 
- Do not call these tests with the `joinChannel` method.
