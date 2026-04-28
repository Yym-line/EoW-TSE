<div align="center">
    <h1>
  EoW-TSE
    </h1>
    <p>
    Official PyTorch code for paper <br>
    <b><em>Enroll-on-Wakeup: A First Comparative Study of Target Speech Extraction
for Seamless Interaction in Real Noisy Human-Machine Dialogue Scenarios</em></b>
    </p>
    <p>
    </p>
</div>


# Dataset

We utilize a dataset of real-world recordings across five distinct acoustic scenarios. These scenarios are designed to reflect the complexities of actual human-machine dialogue, varying in terms of speaker distance (*d*), reverberation time (*RT*60), and signal-to-noise ratio (SNR).

The dataset can be found at: [GitHub-Eow-TSE](https://github.com/Yym-line/EoW-TSE/tree/main/audio)

<table>
  <thead>
    <tr>
      <th rowspan="2">Scenario</th>
      <th rowspan="2">Condition</th>
      <th rowspan="2">#spk/#utt</th>
      <th colspan="2">Average duration (s)</th>
    </tr>
    <tr>
      <th>enrollment</th>
      <th>mixture</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>CloseNoise-10</td>
      <td>d=1, RT=0.4</td>
      <td>45/503</td>
      <td>1.08</td>
      <td>1.78</td>
    </tr>
    <tr>
      <td>FarNoise-10</td>
      <td>d=3, RT=0.4</td>
      <td>45/502</td>
      <td>1.08</td>
      <td>1.78</td>
    </tr>
    <tr>
      <td>FarNoiseReverb-10</td>
      <td>d=3, RT=0.6</td>
      <td>30/278</td>
      <td>1.17</td>
      <td>1.79</td>
    </tr>
    <tr>
      <td>FarNoise-5</td>
      <td>d=3, RT=0.4</td>
      <td>45/503</td>
      <td>1.08</td>
      <td>1.78</td>
    </tr>
    <tr>
      <td>FarNoiseReverb-5</td>
      <td>d=3, RT=0.6</td>
      <td>15/226</td>
      <td>0.97</td>
      <td>1.76</td>
    </tr>
  </tbody>
</table>

**This dataset is used to test the performance of SOTA TSE models within the EoW framework.**

- **d** denotes distance to the microphone (m). We evaluate the impact of distance by comparing close-talk (***d* = 1m, *CloseNoise***) and far-field (***d* = 3m, *FarNoise***) configurations
- **RT** is the reverberation time (**RT60 in seconds**). We assess the impact of increased reverberation by varying ***RT*60 from 0.4s to 0.6s (*FarNoiseReverb*)**.  
- **-10/-5** indicates **SNR levels of 10dB and 5dB** respectively. 
- Both the enrollment segments (**x_wake**) and the noisy mixtures (**x_query**) in these scenarios contain complex TV background noise, non-target human speech from the program, and additional spontaneous interfering talkers in the recording room.  
- The enrollment utilize two specific **Chinese** wake-up commands: **“Hi, Pandora” or “Hello, Cube”**.   

# Architecture

![figeow2](D:\桌面\fczgsrjmgzwfjnmchhssxpcsrnfyxpmc\figeow2.png)


# Evaluation

We test the **WER and DNSMOS** of **discriminative and generative models** (Solospeech, SEF-PNet, LExt and CIE-mDPTNet) in these five scenarios within the EoW framework.

We utilize **enrollment augmentation methods using LLM-based TTS** **to** **explore the performance upper limit** of these models within the EoW framework.

# Results

***Results of TSE models on Libri2Mix 2-Speaker+Noise.***

| Models       | SI-SDR | PESQ | STOI | Params | MACs |
|----------------------------|---------|----------|-------------|-------------|-------------|
| Mixture | -1.96 | 1.08 | 64.73 | -  | -  |
| SEF-PNet | 8.18 | 1.55 | 82.67   | 6.08M  | 15.87G |
| CIE-mDPTNet    | 9.47 | 1.78 | 85.35     | **2.9M** | 12.10G |
| LExt           | 10.47 | 1.88 | **87.26**   | 3.9M       | **8.63G**  |
| Solospeech           | **11.12** | **1.89** | -           | 590.9M    | -          |



***OVRL scores on five scenarios: Original EoW-TSE* *vs. TTS-augmented (CR).***

![fig](D:\桌面\fczgsrjmgzwfjnmchhssxpcsrnfyxpmc\fig.png)



***WERs on five scenarios: Original EoW-TSE vs. TTS augmented (CR).***

![fig1](D:\桌面\fczgsrjmgzwfjnmchhssxpcsrnfyxpmc\fig1.png)



***Results on enrollments synthesized using different TTS models***

<table>
  <thead>
    <tr>
      <th rowspan="2">Conditions</th>
      <th colspan="4">DNSMOS ↑</th>
      <th colspan="4">WER ↓</th>
    </tr>
    <tr>
      <th>Noisy</th>
      <th>xTTS</th>
      <th>IndexTTS2</th>
      <th>CosyVoice3</th>
      <th>Noisy</th>
      <th>xTTS</th>
      <th>IndexTTS2</th>
      <th>CosyVoice3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>CloseNoise-10</td>
      <td>2.029</td>
      <td>2.472</td>
      <td>2.564</td>
      <td><strong>2.609</strong></td>
      <td>14.61</td>
      <td>39.76</td>
      <td><strong>11.23</strong></td>
      <td>40.36</td>
    </tr>
    <tr>
      <td>FarNoise-10</td>
      <td>1.590</td>
      <td><strong>2.464</strong></td>
      <td>2.237</td>
      <td>2.428</td>
      <td>27.74</td>
      <td>40.04</td>
      <td><strong>19.27</strong></td>
      <td>54.88</td>
    </tr>
    <tr>
      <td>FarNoiseReverb-10</td>
      <td>1.262</td>
      <td><strong>2.549</strong></td>
      <td>1.917</td>
      <td>2.114</td>
      <td>19.69</td>
      <td>33.45</td>
      <td><strong>16.10</strong></td>
      <td>59.71</td>
    </tr>
    <tr>
      <td>FarNoise-5</td>
      <td>1.440</td>
      <td>2.274</td>
      <td>2.063</td>
      <td><strong>2.400</strong></td>
      <td>45.13</td>
      <td>46.57</td>
      <td><strong>36.58</strong></td>
      <td>80.62</td>
    </tr>
    <tr>
      <td>FarNoiseReverb-5</td>
      <td>1.251</td>
      <td><strong>2.473</strong></td>
      <td>1.743</td>
      <td>2.107</td>
      <td>70.58</td>
      <td>55.86</td>
      <td><strong>44.47</strong></td>
      <td>102.54</td>
    </tr>
  </tbody>
</table>



***EoW-TSE results with different enrollment augmentation methods on CIE-mDPTNet.***

<table>
  <thead>
    <tr>
      <th rowspan="2">Conditions</th>
      <th colspan="5">DNSMOS ↑</th>
      <th colspan="5">WER ↓</th>
    </tr>
    <tr>
      <th>Noisy</th>
      <th>xTTS (CR)</th>
      <th>IndexTTS2 (CR)</th>
      <th>xTTS(EC)</th>
      <th>IndexTTS2(EC)</th>
      <th>Noisy</th>
      <th>xTTS(CR)</th>
      <th>IndexTTS2(CR)</th>
      <th>xTTS(EC)</th>
      <th>IndexTTS2(EC)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>CloseNoise-10</td>
      <td>1.904</td>
      <td>2.163</td>
      <td>2.119</td>
      <td><strong>2.197</strong></td>
      <td>2.170</td>
      <td><strong>3.10</strong></td>
      <td>4.37</td>
      <td>3.24</td>
      <td>3.91</td>
      <td>3.48</td>
    </tr>
    <tr>
      <td>FarNoise-10</td>
      <td>1.371</td>
      <td>1.511</td>
      <td>1.481</td>
      <td><strong>1.527</strong></td>
      <td>1.500</td>
      <td><strong>4.54</strong></td>
      <td>6.41</td>
      <td>5.38</td>
      <td>6.81</td>
      <td>5.14</td>
    </tr>
    <tr>
      <td>FarNoiseReverb-10</td>
      <td>1.165</td>
      <td>1.202</td>
      <td>1.200</td>
      <td><strong>1.214</strong></td>
      <td>1.195</td>
      <td><strong>3.52</strong></td>
      <td>7.15</td>
      <td>5.06</td>
      <td>7.00</td>
      <td>6.08</td>
    </tr>
    <tr>
      <td>FarNoise-5</td>
      <td>1.286</td>
      <td>1.379</td>
      <td>1.366</td>
      <td><strong>1.396</strong></td>
      <td>1.371</td>
      <td><strong>13.31</strong></td>
      <td>16.01</td>
      <td>29.55</td>
      <td>15.92</td>
      <td>25.61</td>
    </tr>
    <tr>
      <td>FarNoiseReverb-5</td>
      <td>1.173</td>
      <td>1.216</td>
      <td><strong>1.223</strong></td>
      <td>1.217</td>
      <td>1.212</td>
      <td><strong>15.95</strong></td>
      <td>22.89</td>
      <td>31.32</td>
      <td>23.12</td>
      <td>28.36</td>
    </tr>
  </tbody>
</table>



# Checkpoints

We release several checkpoints for reproduction and further research:  

- **SEF-PNet**

- **LExt** 
  
- **CIE-mDPTNet** 

All checkpoints can be found at: [GitHub-Eow-TSE](https://github.com/Yym-line/EoW-TSE/tree/main/checkpoints)
