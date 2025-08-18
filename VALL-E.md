---
tags:
  - llm
  - speech
  - tts
Authors: Wang et al.
Year: 2023
Publish: arxiv
---
Links: [code](https://github.com/microsoft/unilm) [demo](https://aka.ms/valle)
annotation-target::[[VALL-E.pdf|Neural Codec Language Models are Zero-Shot Text to Speech Synthesizers]]

## Summary

VALLE-E is a LLM-based TTS model that regards speech synthesis as a conditional codec language modelling task with neural codec speech codes. It's trained on a much larger dataset (60k hours English multi-speaker data) compared with other SOTA approaches (less than 600 hours).
## Main Ideas
### Problem Formulation
$D=\{x_i, y_i\}$: dataset
$x=\{x_0, x_1, ..., x_L\}$: phoneme sequence
$y$: audio sample

$C$: 2 dimensional acoustic code matrix. shape [t, 8] (8 codebooks)
$T$: utterance length / time stamps
$c_{t,:}$: eight codes for frame t. shape [1, 8]
$c_{:,j}$: code sequence from the $j$-th codebook. shape [t, 8-j]

$Encodec(y)=C^{T*8}$ 
$Decodec(C)\approx\hat{y}$
### AR Codec Language Modelling 
- AR language model generates the tokens from the 1st quantizer. 
- AR model consists of 
	- phoneme embedding $W_x$
	- acoustic embedding $W_a$
	- [[Sinuous Position Embedding]] 
	- transformer decoder
	- prediction layer
	- note that output projection layer and $W_a$ share parameters ([[#^uy9c4n91dt|weight tying]])
		- #speech_concept #ml_concept)
- Training time
	- input (causal): $\{x_0, x_1, ... x_L, <EOS>, c_{0,1}, c_{1,1}, c_{2,1}, ..., c_{t,1}, <EOS>\}$ 
	- At training time there's no audio prompt
- inference time
	- input: phoneme sequence of enrolled recording + phoneme sequence + acoustic prompt
	- acoustic tokens of enrolled recording as prefix of the decoded sequence (because we added the phoneme sequence of the enrolled recording in the input. Basically it works as a prompt and the decoding for this part is fake. )
### NAR Codec Language Modelling
- NAR language mode generates the tokens from 2nd-8th quantizers. 
- Main architecture is similar to AR, except contains 8 separate acoustic embedding layers.
- In each training step, training stage $i\in[2, 8]$ will be randomly sampled. The model is trained to maximize the acoustic tokens from the $i$-th quantizer codebook. The acoustic tokens from stage 1 to stage $i$-1 will be embedded and summed up as model input. 
- input to predict $j$-th codebook is the concatenation of $(e_x, e_{\hat{c}}, e_{c:,<1})$ where
  $e_x$: phoneme token embeddings
  $e_{\hat{c}}$: acoustic token embeddings of enrolled recording
  $e_{c:,<1}$: sum of the acoustic token embddings of previous codebooks
- weight lying
- current stage $i$ is injected into the network with [[Adaptive Layer Normalization]]
	- $AdaLN(h, i)=a_iLayerNorm(h)+b$
	- $h$ is the intermediate activations, $a_i$ and $b_i$ are obtained from a linear projection of the stage embedding. 
### Inference
- AR uses sampling-based decoding approach
- NAR uses greedy decoding
- VALL-E: acoustic prompt is not semantically related to text
- VALL-E-continual: acoustic prompt is semantically related to text

### Metrics
- Automatic metrics
	- speaker similarity: SOTA speaker verification model [[WavLM-TDNN]]
	- synthesis robustness: HuBERT-Large model as ASR model to calculate WER (to detect deletion, insertion, etc.)
- Human evaluation
	- speaker similarity: Similarity MOS - range 1 to 5 with 0.5 increments
	- naturalness: Comparative MOS - range from -3 to 3 with intervals of 1

### Results
- VALL-E is better than baseline models. However the result didn't report p-value. 
- Ablation Study
	- both phoneme and acoustic prompts are important to NAR model
	- the acoustic prompt for AR contributes a lot to speaker similarity

## Experiment Setup
- Dataset
	- LibriLight - 60k hours unlabelled English audiobook - 7000 speakers - average sample length 60 seconds
	- Labelled with DNN-HMM ASR model trained on 960 hours LibriSpeech
- Model
	- Transformer
		- 12 layers
		- 16 attention heads
		- embed_dim 1024
		- FFN_dim 4096
		- dropout 0.1
	- [[YourTTS]] (baseline)
		- trained on a combined dataset of VCTK, LibriTTS, and TTS-Portuguese
		- used their released ckpt
- Randomly crop the waveform to a random length between 10 sec and 20 sec

## TODO
- [ ] RVQ: EnCodec
- [ ] VQTTS
- [ ] HuBERT
- [ ] VQVAE
- [ ] WaveNet
- [ ] vq-wav2vec
## Annotations


>%%
>```annotation-json
>{"created":"2025-08-01T02:07:00.382Z","text":"Why the first using codec tokens as intermediate representations? Sounds like Tjandra et al. [2019] also uses VQVAE codes as intermediate representations?","updated":"2025-08-01T02:07:00.382Z","document":{"title":"VALL-E.pdf","link":[{"href":"urn:x-pdf:8f1f3669b82ae94609774b914638453f"},{"href":"vault:/PDFs/VALL-E.pdf"}],"documentFingerprint":"8f1f3669b82ae94609774b914638453f"},"uri":"vault:/PDFs/VALL-E.pdf","target":[{"source":"vault:/PDFs/VALL-E.pdf","selector":[{"type":"TextPositionSelector","start":12487,"end":12563},{"type":"TextQuoteSelector","exact":", VALL-E is the first touse audio codec codes as intermediate representation","prefix":"h 60K hours of data. Furthermore","suffix":"s, and emerge in-context learnin"}]}]}
>```
>%%
>*%%PREFIX%%h 60K hours of data. Furthermore%%HIGHLIGHT%% ==, VALL-E is the first touse audio codec codes as intermediate representation== %%POSTFIX%%s, and emerge in-context learnin*
>%%LINK%%[[#^lufvezx51zi|show annotation]]
>%%COMMENT%%
>Why the first using codec tokens as intermediate representations? Sounds like Tjandra et al. [2019] also uses VQVAE codes as intermediate representations?
>%%TAGS%%
>
^lufvezx51zi


>%%
>```annotation-json
>{"created":"2025-08-04T18:28:49.593Z","text":"Why? Need to read HuBert paper.","updated":"2025-08-04T18:28:49.593Z","document":{"title":"VALL-E.pdf","link":[{"href":"urn:x-pdf:8f1f3669b82ae94609774b914638453f"},{"href":"vault:/PDFs/VALL-E.pdf"}],"documentFingerprint":"8f1f3669b82ae94609774b914638453f"},"uri":"vault:/PDFs/VALL-E.pdf","target":[{"source":"vault:/PDFs/VALL-E.pdf","selector":[{"type":"TextPositionSelector","start":14672,"end":14840},{"type":"TextQuoteSelector","exact":"1) It contains abundant speaker information andacoustic information, which could maintain speaker identity in reconstruction compared to HuBERTcodes [Hsu et al., 2021].","prefix":"shows the following advantages: ","suffix":" 2) There is an off-the-shelf co"}]}]}
>```
>%%
>*%%PREFIX%%shows the following advantages:%%HIGHLIGHT%% ==1) It contains abundant speaker information andacoustic information, which could maintain speaker identity in reconstruction compared to HuBERTcodes [Hsu et al., 2021].== %%POSTFIX%%2) There is an off-the-shelf co*
>%%LINK%%[[#^hcxntto7eu5|show annotation]]
>%%COMMENT%%
>Why? Need to read HuBert paper.
>%%TAGS%%
>#TODO
^hcxntto7eu5


>%%
>```annotation-json
>{"created":"2025-08-04T20:16:43.014Z","text":"I don't understand what is  ̃c:,1","updated":"2025-08-04T20:16:43.014Z","document":{"title":"VALL-E.pdf","link":[{"href":"urn:x-pdf:8f1f3669b82ae94609774b914638453f"},{"href":"vault:/PDFs/VALL-E.pdf"}],"documentFingerprint":"8f1f3669b82ae94609774b914638453f"},"uri":"vault:/PDFs/VALL-E.pdf","target":[{"source":"vault:/PDFs/VALL-E.pdf","selector":[{"type":"TextPositionSelector","start":19020,"end":19092},{"type":"TextQuoteSelector","exact":"Only c:,1 is predicted while the prefix ̃c:,1 is given during inference.","prefix":"t a specific token in training. ","suffix":"For the discrete tokens from the"}]}]}
>```
>%%
>*%%PREFIX%%t a specific token in training.%%HIGHLIGHT%% ==Only c:,1 is predicted while the prefix ̃c:,1 is given during inference.== %%POSTFIX%%For the discrete tokens from the*
>%%LINK%%[[#^1c7eh749ljh|show annotation]]
>%%COMMENT%%
>I don't understand what is  ̃c:,1
>%%TAGS%%
>
^1c7eh749ljh


>%%
>```annotation-json
>{"created":"2025-08-05T18:20:16.357Z","text":"Weight tying. This is to force the input and output spaces to be  aligned.","updated":"2025-08-05T18:20:16.357Z","document":{"title":"VALL-E.pdf","link":[{"href":"urn:x-pdf:8f1f3669b82ae94609774b914638453f"},{"href":"vault:/PDFs/VALL-E.pdf"}],"documentFingerprint":"8f1f3669b82ae94609774b914638453f"},"uri":"vault:/PDFs/VALL-E.pdf","target":[{"source":"vault:/PDFs/VALL-E.pdf","selector":[{"type":"TextPositionSelector","start":21724,"end":21827},{"type":"TextQuoteSelector","exact":"We share the parameters of the output projectionlayer with the parameters of the acoustic embedding Wa.","prefix":"xt token in the first codebook. ","suffix":"In the AR model, we do not expli"}]}]}
>```
>%%
>*%%PREFIX%%xt token in the first codebook.%%HIGHLIGHT%% ==We share the parameters of the output projectionlayer with the parameters of the acoustic embedding Wa.== %%POSTFIX%%In the AR model, we do not expli*
>%%LINK%%[[#^uy9c4n91dt|show annotation]]
>%%COMMENT%%
>Weight tying. This is to force the input and output spaces to be  aligned.
>%%TAGS%%
>
^uy9c4n91dt
