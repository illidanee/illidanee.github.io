---
layout: post
title: PCM音频采集和播放
categories: 
 - Audio
tags:
 - Audio
---

* content
{:toc}

> 客户要语音聊天的功能，之前没有做过这方面的东西，参考了很多文章，最后整理一下，下次忘了好拿出来看看。也希望给和我一样不懂的宝宝们一些参考。




## 参考

* [Waveform Structures](https://msdn.microsoft.com/en-us/library/dd743836%28v=vs.85%29.aspx)
* [Waveform Messages](https://msdn.microsoft.com/en-us/library/dd743835%28v=vs.85%29.aspx)
* [Waveform Functions](https://msdn.microsoft.com/en-us/library/dd743834%28v=vs.85%29.aspx)

## 音频采集和播放

```c
#pragma once

#include <windows.h>
#include <mmsyscom.h>

#pragma comment(lib, "winmm.lib")

#define SAMPLE_PER_SEC 8000
#define BIT_PER_SAMPLE 16
#define AUDIO_BUFFER_MAX_LEN (SAMPLE_PER_SEC * BIT_PER_SAMPLE / 8 / 10)
#define BUFFER_NUM 8

namespace illidan
{
	class Audio
	{
	private:
		WAVEINCAPS m_WICAP;
		WAVEOUTCAPS m_WOCAP;
		HWAVEIN m_WI;
		HWAVEOUT m_WO;
		WAVEFORMATEX m_WIFEX;
		WAVEFORMATEX m_WOFEX;
		WAVEHDR m_WIH;
		WAVEHDR m_WOH;
		char ALLDATA[BUFFER_NUM][AUDIO_BUFFER_MAX_LEN];
		int m_CurInNo;
		int m_CurOutNo;

	public:
		static void CALLBACK WaveInProc(
			HWAVEIN   hwi,
			UINT      uMsg,
			DWORD_PTR dwInstance,
			DWORD_PTR dwParam1,
			DWORD_PTR dwParam2
			);

		static void CALLBACK waveOutProc(
			HWAVEOUT  hwo,
			UINT      uMsg,
			DWORD_PTR dwInstance,
			DWORD_PTR dwParam1,
			DWORD_PTR dwParam2
			);

	public:
		Audio();
		virtual ~Audio();

		int WaveInInit();
		int WaveInStart();
		int WaveInStop();

		int WaveOutInit();
		int WaveOutStart();
		int WaveOutStop();

		char* GetWaveInDataBuffer();

	};
}
```

```c
#include "Audio.h"


namespace illidan
{
	void CALLBACK Audio::WaveInProc(
		HWAVEIN   hwi,
		UINT      uMsg,
		DWORD_PTR dwInstance,
		DWORD_PTR dwParam1,
		DWORD_PTR dwParam2
		)
	{
		switch (uMsg)
		{
		case WIM_OPEN:

			break;
		case WIM_DATA:
		{
			Audio* pAudio = (Audio*)dwInstance;
			if (pAudio == 0)
				break;

			LPWAVEHDR pWIH = (LPWAVEHDR)dwParam1;
			if (pWIH == 0)
				break;

			MMRESULT res = waveInUnprepareHeader(hwi, pWIH, sizeof(WAVEHDR));
			if (res != MMSYSERR_NOERROR)
				break;

			pAudio->m_WOH.lpData = pAudio->ALLDATA[pAudio->m_CurInNo];
			pAudio->m_WOH.dwBufferLength = AUDIO_BUFFER_MAX_LEN;
			pAudio->m_WOH.dwFlags = 0;
			res = waveOutPrepareHeader(pAudio->m_WO, &pAudio->m_WOH, sizeof(WAVEHDR));
			if (res != MMSYSERR_NOERROR)
				break;

			res = waveOutWrite(pAudio->m_WO, &pAudio->m_WOH, sizeof(WAVEHDR));
			if (res != MMSYSERR_NOERROR)
				break;

			pAudio->m_CurInNo = (pAudio->m_CurInNo + 1) % BUFFER_NUM;
			if (pAudio->m_CurInNo == pAudio->m_CurOutNo)
				break;

			pWIH->lpData = pAudio->ALLDATA[pAudio->m_CurInNo];
			pWIH->dwBufferLength = AUDIO_BUFFER_MAX_LEN;
			pWIH->dwFlags = 0;
			res = waveInPrepareHeader(hwi, pWIH, sizeof(WAVEHDR));
			if (res != MMSYSERR_NOERROR)
				break;

			res = waveInAddBuffer(hwi, pWIH, sizeof(WAVEHDR));
			if (res != MMSYSERR_NOERROR)
				break;

			res = waveInStart(hwi);
			if (res != MMSYSERR_NOERROR)
				break;
		}
			break;
		case WIM_CLOSE:

			break;
		}
	}

	void CALLBACK Audio::waveOutProc(
		HWAVEOUT  hwo,
		UINT      uMsg,
		DWORD_PTR dwInstance,
		DWORD_PTR dwParam1,
		DWORD_PTR dwParam2
		)
	{
		switch (uMsg)
		{
		case WOM_CLOSE:
			break;
		case WOM_DONE:
		{
			Audio* pAudio = (Audio*)dwInstance;
			if (pAudio == 0)
				break;

			LPWAVEHDR pWOH = (LPWAVEHDR)dwParam1;
			if (pWOH == 0)
				break;

			MMRESULT res = waveOutUnprepareHeader(hwo, pWOH, sizeof(WAVEHDR));
			if (res != MMSYSERR_NOERROR)
				break;

			pAudio->m_CurOutNo = (pAudio->m_CurOutNo + 1) % BUFFER_NUM;
		}
			break;
		case WOM_OPEN:
			break;
		}
	}

	Audio::Audio()
		: m_CurInNo(0), m_CurOutNo(0)
	{

	}

	Audio::~Audio()
	{

	}

	int Audio::WaveInInit()
	{
		UINT devNum = waveInGetNumDevs();
		if (devNum == 0)
			return -1;

		MMRESULT res = waveInGetDevCaps(WAVE_MAPPER, &m_WICAP, sizeof(WAVEINCAPS));
		if (res != MMSYSERR_NOERROR)
			return -2;

		m_WIFEX.wFormatTag = WAVE_FORMAT_PCM;
		m_WIFEX.nChannels = m_WICAP.wChannels;
		m_WIFEX.nSamplesPerSec = SAMPLE_PER_SEC;
		m_WIFEX.wBitsPerSample = BIT_PER_SAMPLE;
		m_WIFEX.nAvgBytesPerSec = m_WIFEX.nChannels * m_WIFEX.nSamplesPerSec * m_WIFEX.wBitsPerSample / 8;
		m_WIFEX.nBlockAlign = m_WIFEX.nChannels * m_WIFEX.wBitsPerSample / 8;
		m_WIFEX.cbSize = 0;

		res = waveInOpen(&m_WI, WAVE_MAPPER, &m_WIFEX, (DWORD_PTR)WaveInProc, (DWORD_PTR)this, CALLBACK_FUNCTION);
		if (res != MMSYSERR_NOERROR)
			return -3;

		return 0;
	}

	int Audio::WaveInStart()
	{
		m_WIH.lpData = ALLDATA[m_CurInNo];
		m_WIH.dwBufferLength = AUDIO_BUFFER_MAX_LEN;
		m_WIH.dwFlags = 0;
		MMRESULT res = waveInPrepareHeader(m_WI, &m_WIH, sizeof(WAVEHDR));
		if (res != MMSYSERR_NOERROR)
			return -1;
		
		res = waveInAddBuffer(m_WI, &m_WIH, sizeof(WAVEHDR));
		if (res != MMSYSERR_NOERROR)
			return -2;

		res = waveInStart(m_WI);
		if (res != MMSYSERR_NOERROR)
			return -3;

		return 0;
	}

	int Audio::WaveInStop()
	{
		MMRESULT res = waveInStop(m_WI);
		if (res != MMSYSERR_NOERROR)
			return -1;

		//res = waveInReset(m_WI);
		//if (res != MMSYSERR_NOERROR)
		//	return -2;

		Sleep(1000);

		res = waveInClose(m_WI);
		if (res != MMSYSERR_NOERROR)
			return -3;

		return 0;
	}

	int Audio::WaveOutInit()
	{
		UINT devNum = waveOutGetNumDevs();
		if (devNum == 0)
			return -1;

		MMRESULT res = waveOutGetDevCaps(WAVE_MAPPER, &m_WOCAP, sizeof(WAVEOUTCAPS));
		if (res != MMSYSERR_NOERROR)
			return -2;

		m_WOFEX.wFormatTag = WAVE_FORMAT_PCM;
		m_WOFEX.nChannels = m_WOCAP.wChannels;
		m_WOFEX.nSamplesPerSec = SAMPLE_PER_SEC;
		m_WOFEX.wBitsPerSample = BIT_PER_SAMPLE;
		m_WOFEX.nAvgBytesPerSec = m_WOFEX.nChannels * m_WOFEX.nSamplesPerSec * m_WOFEX.wBitsPerSample / 8;
		m_WOFEX.nBlockAlign = m_WOFEX.nChannels * m_WOFEX.wBitsPerSample / 8;
		m_WOFEX.cbSize = 0;

		res = waveOutOpen(&m_WO, WAVE_MAPPER, &m_WOFEX, (DWORD_PTR)waveOutProc, (DWORD_PTR)this, CALLBACK_FUNCTION);
		if (res != MMSYSERR_NOERROR)
			return -3;

		if (m_WOCAP.dwSupport & WAVECAPS_VOLUME)
		{
			res = waveOutSetVolume(m_WO, 0xFFFF);
			if (res != MMSYSERR_NOERROR)
				return -4;
		}

		return 0;
	}

	int Audio::WaveOutStart()
	{

		return 0;
	}

	int Audio::WaveOutStop()
	{
		MMRESULT res = waveOutReset(m_WO);
		if (res != MMSYSERR_NOERROR)
			return -1;

		Sleep(1000);

		res = waveOutClose(m_WO);
		if (res != MMSYSERR_NOERROR)
			return -3;

		return 0;
	}

	char* Audio::GetWaveInDataBuffer()
	{
		return ALLDATA[m_CurInNo];
	}
}
```


