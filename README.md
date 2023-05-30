# Digital Twin

## Authors:
Imre Molnár

Zsolt Csibi

## Supervisor: 
Bruno Carlos Dos Santos Melício 

# 1. Introduction

Aim: 
 - Create a digital representation of a person (digital twin)
 - There are multiple reasons why is this helpful:
   - Filming industry (what if the actor passes away)
   - Chance to live forever (digital footprint)
   - Create a digital twin that can interact with others 
   - Video game avatar (voice, gestures)

# Goal

Create a digital twin, which should work like:
1. Take an input video of a person
2. Split the video and audio
3. Fine-tune a Deep Neural Network to learn (clone) the voice in the audio
4. Generate a new audio with the cloned voice based on a text prompt
5. Use a Deep Neural Network to sync an input video with the new cloned audio
6. Output a video of a person saying that text with the person’s voice and face

# 2. Data

## We created our own videos:

 - We don’t have to deal with copyright, legal issues
 - We have an endless source of videos
 - We could make our friend say funny things
 - We did not found suitable videos for the purpose of the task
 - We can control how the people appear on the video (the model really depend on it)

## Why is a video needed?

 - We need audio for  the voice cloning (it’s already included)
 - We need video to accurately map voice to the new video
 - For 3D modelling videos also better
 - Video is better for lip syncing

# 3. Methods

![pipeline](./docs/digitalTwinpipeline.drawio.png)

## Voice Cloning  - TTS
 - Creation of an artificial simulation of a person’s voice.
 - Library for advanced Text-to-Speech generation

## Lip Syncing - Wav2Lip
 - Move the lips silently in synchronization with a pre-recorded soundtrack
 - Producing accurate lip movements on a static image or videos of specific people seen during the training

# 4. Experiments 
## Technologies we tried
### Video generation:

- 3DDFA [https://github.com/cleardusk/3DDFA](https://github.com/cleardusk/3DDFA)
- Thin Plate Spline Motion Model [https://github.com/yoyo-nb/Thin-Plate-Spline-Motion-Model](https://github.com/yoyo-nb/Thin-Plate-Spline-Motion-Model)
- First Order Model [https://github.com/AliaksandrSiarohin/first-order-model](https://github.com/AliaksandrSiarohin/first-order-model)
- Wav2Lip [https://github.com/Rudrabha/Wav2Lip](https://github.com/Rudrabha/Wav2Lip)

### Audio generation:
- Real Time Voice Cloning [https://github.com/CorentinJ/Real-Time-Voice-Cloning](https://github.com/CorentinJ/Real-Time-Voice-Cloning)
- TTS [https://github.com/coqui-ai/TTS](https://github.com/coqui-ai/TTS)

## Thin Plate Spline Motion Model
- It makes a video based on an image and a reference video
- It was also very slow
- We used the vox.pth.tar
pretrained model
- It depends on the face
position on the reference
video

![mate](./docs/mate.gif)
![voldemort](./docs/voldemort.gif)

## First Order Model
- We still have nightmares after this
- Results are bad, also a relatively slow method

![trump](./docs/trump.gif)
![csato](./docs/csato.gif)

## Wav2Lip

- Can create lip synced video from image
- We used the wav2lip_gan.pth pretrained model

### Lots of hyperparameter:

- pads - we can move the detected bounding box around the face
- no_smooth - does not smooth face
- resize-factor - resize video (the model works better for 720p and 480p videos)

## Real Time Voice Cloning

[sample](./docs/7dia.wav)

 - It did not handle noisy recordings.
 - It was very slow.
 - Needs a lots of training data.
 - The generated audio sounds like a robot.
 - There are no hyperparameters.
 - It needs a really long reference audio to be acceptable

## TTS

[sample](./docs/8dia.wav)

- TTS provided pretty good results regarding audio.
- The generated audio sound pretty close to the sample.
- The audio generation is also fast.
- It was also easy to setup.
- For the reference videos we used the pretrained multilingual models.

# 5. Results (TTS + Wav2Lip)

- It gave us a pretty good result
- It’s highly dependent on where the face is on the recording
- Better results with 720p and 480p landscape videos
- Higher resolution videos gave artifacts

# 6. Conclusion

 - Overall we managed to make a pipeline that creates an accurate avatar
 - The method is not general enough, because it is highly dependent on the face position on the image
 - The voice cloning generates realistic audio files
 - The generated videos sometimes contain blurry frames but overall they are pretty good

## Future plans

 - Make voice cloning more clear
 - Improve efficiency of lip syncing
 - Replace Wav2lip for a tool which can manipulate face objects
 - Make 3D model of the person and manipulate it with Action Units (Potentially 3DDFA)

## 3DDFA
 - The idea is to create a 3D model from a video, so an avatar can be created.
 - It gives a relatively good 3D model, but Wav2lip do not need a 3D model for lip syncing, it is not necessary for our goals.

## Challenges

 - Finding out good resolution, orientation and length of raw videos
 - Wav2lip runs out of GPU for no reason at longer videos than 1 minute, without a good error message
 - Setuping environments for projects (dependency conflicts between packages)
 - Making pipeline for the project integrating technologies
 - Making literature search about suitable technologies
 - Finding the best hyperparameters for lip syncing
