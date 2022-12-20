# BBC World Service Accesibility PoC

Background:
BBC World Service Programming is excellent, but often without accessible transcripts due to the manual manner in which they are generated.
This proof of concept seeks to remedy the situation.

Objectives: 
- Stream BBC world Service into audio files (WAV), chunked based on programme schedule
- Use pyannote-audio to annotate into sub-chunks based on speaker (use a speaker hash for speaker names so that this can become reference data)
- Use OpenAI Whisper to transcribe audio
- Re-assemble into VTTs by programme with reference data for speaker names for playback on a website alongside original audio

Proposal: Highlight method to BBC for improving accesibilty of BBC world service programming.

Extended Proposal: build-up a speaker identification list (including regular broadcasters and guests referencing their wikipedia entries), with which to tag the content.

## Research

# Pyannote plays and Whisper rhymes [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Majdoddin/nlp/blob/main/Pyannote_plays_and_Whisper_rhymes_v_2_0.ipynb)

### Whisper's transcription plus Pyannote's Diarization 

Andrej Karpathy [suggested](https://twitter.com/karpathy/status/1574476200801538048?s=20&t=s5IMMXOYjBI6-91dib6w8g) training a classifier on top of  OpenAI [`openai/whisper`](https://github.com/openai/whisper) model features to identify the speaker, so we can visualize the speaker in the transcript. But, as [pointed out](https://twitter.com/tarantulae/status/1574493613362388992?s=20&t=s5IMMXOYjBI6-91dib6w8g) by Christian Perone, it seems that features from whisper wouldn't be that great for speaker recognition as its main objective is basically to ignore speaker differences.

In the following, I use [**`pyannote-audio`**](https://github.com/pyannote/pyannote-audio), a speaker diarization toolkit by Hervé Bredin, to identify the speakers, and then match it with the transcriptions of Whispr. I try it on a part of an [interview](https://youtu.be/NSp2fEQ6wyA) with Freeman Dyson. Check the result [**here**](https://majdoddin.github.io/dyson.html).

To make it easier to match the transcriptions to diarizations by speaker change, I applied the Sarah Kaiser's [suggestion](https://github.com/openai/whisper/discussions/264#discussioncomment-3825375) to run `pyannote.audio` first and  then just run `whisper` on the split-by-speaker chunks. 


(For sake of performance , I also tried attaching the audio segements into a single audio file with a silent -or beep- spacer as a seperator, and run whisper on it see it on [colab](https://colab.research.google.com/drive/1HuvcY4tkTHPDzcwyVH77LCh_m8tP-Qet?usp=sharing). It [works](https://majdoddin.github.io/lexicap.html) on some audio, and fails on some -e.g. Dyson's Interview. The problem is, whisper does not reliably make a timestap on a spacer. See the discussions [#139](https://github.com/openai/whisper/discussions/139) and [#29](https://github.com/openai/whisper/discussions/29))

## Disclaimers - I have no affiliation to the BBC or BBC world service - this is purely an academic piece of research, using a real-world application to prove value.
