# TalkingStackchan

The detail is [my articles](https://www.techlife-hacking.com/?p=1985).

## Tested PC Spec
OS : Ubuntu 20.04  
GPU : Geforce RTX 2080Ti  

## Settings

install pytorch  
Install Pytorch with matching GPU, CUDA and cuDNN versions.  
[Pytorch](https://www.techlife-hacking.com/?p=1325)  

```
# install transformers
pip install transformers

# install whisper
sudo apt update && sudo apt install ffmpeg
pip install git+https://github.com/openai/whisper.git

# install pyaudio
sudo apt-get install portaudio19-dev
pip install pyaudio

# install pvporcupine
pip install pvporcupine

# install alexa_like_whisper
pip install git+https://github.com/tech-life-hacking/AlexaLikeWhisper.git

# install alexa_like_whisper
pip install openai

```

To use pvporcupine, you need to register to [PICOVOICE](https://console.picovoice.ai/) and get a API Key.
And download a model file(.ppn) and place it in TalkingStackchan/model.

## Usage

Edit ACCESS_KEY, KEYWORD_PATH, M5_IPADDRESS, openai.api_key
and run example.py

```python
if __name__ == "__main__":
    # Modelsizes on whisper
    MODELSIZES = ['tiny', 'base', 'small', 'medium', 'large']

    # AccessKey obtained from Picovoice Console (https://console.picovoice.ai/)
    ACCESS_KEY = ""
    KEYWORD_PATH = ['']

    # Recording Time(s)
    RECORDING_TIME = 5

    alexa_like = alexa_like_whisper.AlexaLikeWhisper(ACCESS_KEY, KEYWORD_PATH, MODELSIZES[3], RECORDING_TIME)

    # Parameters on communication about stackchan
    M5_IPADDRESS = ''
    M5_PORTNUMBER = 50100
    sender = Sender()

    openai.api_key = ""
    m5state = M5State()

    try:
        while True:
            result = alexa_like.run()
            m5state.change_state(result)
            result = m5state.run(result)

            sender.publish(result, M5_IPADDRESS, M5_PORTNUMBER)

    except KeyboardInterrupt:
        sender.close()

```