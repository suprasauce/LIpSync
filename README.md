# Lip Sync
## Description

This project uses [Wave2Lip](https://github.com/Rudrabha/Wav2Lip) model to  lip sync a audio file to a target video. Please see to that this project uses model from [this](https://github.com/Rudrabha/Wav2Lip) repository which is cloned in ```Wave2lip/``` directory in the current respository. I have changed some files which can be seen in [this](https://github.com/suprasauce/LIpSync/commit/a1c56bec1a7c373ef5208ac1dd5a661b25a4950b) commit.

## SetUp (Ubuntu)
In terminal paste the following commands:
- Create virtual env using [conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/linux.html)
```python
conda create -n wav2lip python=3.6
```
- Activate env
```python
conda activate wav2lip  
```
- Install relevant dependencies. Go to the root directory of the project and paste the following commands one by one.
```python
cd Wav2Lip
pip install -r requirements.txt
```
- Download the pretrained Wav2lip model from [here](https://iiitaphyd-my.sharepoint.com/:u:/g/personal/radrabha_m_research_iiit_ac_in/EdjI7bZlgApMqsVoEUUXpLsBxqXbn5z8VTmoxp55YNDcIA?e=n9ljGW) and past it in ```Wave2Lip/checkpoints/```. After pasting it should be like ```Wave2Lip/checkpoints/wave2lip_gan.pth```


- Face detection [pre-trained model](https://www.adrianbulat.com/downloads/python-fan/s3fd-619a316812.pth) should be downloaded to ```Wave2Lip/face_detection/detection/sfd/s3fd.pth```.

## Running the model
### Input to the model present in```sample/input```
- [Input audio](https://github.com/suprasauce/LIpSync/blob/main/sample/input/audio.wav)
- [Input video](https://github.com/suprasauce/LIpSync/blob/main/sample/input/video.mp4) 
- Assuming you are in ```Wave2Lip/```, run the following command
```python
python inference.py --checkpoint_path checkpoints/wav2lip_gan.pth --face ../sample/input/video.mp4 --audio ../sample/input/audio.wav --pads 0 0 0 0
```
- ```--checkpoint_path``` is the location of the pretrained model to use
- ```--face``` is the location of the target video
- ```--audio``` is the location of the audio to be lip synced

Following args are optional, these are passed in order to improve the results:
- ```--pads``` is the padding to be added to the detected face bounding box. --pads \<top> \<bottom> \<left> \<right>
### Output 
- ouput produced by model will be stored at ```Wave2lip/results`
- [ouput](https://github.com/suprasauce/LIpSync/blob/main/sample/output/output.mp4)

## Evaluation
 - For evaluation of the generated lip synced video, the authors of Wav2Lip model have devised new metrics namely LSE-D (Lip Sync Error Distance) and LSE-C (Lip sync Error Confidence).
 -  A lower LSE-D denotes a higher audio-visual match, i.e., the speech and lip movements are in sync.
 -  A lower confidence score denotes that there are several portions of the video with completely out-of-sync lip movements.
 - Please follow [this](https://github.com/Rudrabha/Wav2Lip/tree/master/evaluation) guide to evaluate the generated video LSE-D and LSE-C metric.
 - I am posting the above metrica of the generated video present in ```sample/ouput/``` here:
```
| LSE-D       | LSE-C       |
|-------------|-------------|
| 6.445222    | 9.083874    |
| 6.88925     | 6.717366    |
| 7.3815765   | 7.0750103   |
```
- Since the generated video contained some instances where there were no faces present, so accordingly 3 timestamps were automatically extracted by the evaluation framework. Hence 3 rows in the above table
- For the reference Wav2Lip model was trained on LRS2 Dataset. Please find the metrics for this below:


