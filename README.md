<div align="center">    
 
# Spiking-Fer: Spiking Neural Network for Facial Expression Recognition With Event Cameras

<!-- [![Paper](http://img.shields.io/badge/paper-arxiv.1001.2234-B31B1B.svg)](https://www.nature.com/articles/nature14539)
[![Conference](http://img.shields.io/badge/NeurIPS-2019-4b44ce.svg)](https://papers.nips.cc/book/advances-in-neural-information-processing-systems-31-2018)
[![Conference](http://img.shields.io/badge/ICLR-2019-4b44ce.svg)](https://papers.nips.cc/book/advances-in-neural-information-processing-systems-31-2018)
[![Conference](http://img.shields.io/badge/AnyConference-year-4b44ce.svg)](https://papers.nips.cc/book/advances-in-neural-information-processing-systems-31-2018)   -->

[![Paper](http://img.shields.io/badge/paper-arxiv.1001.2234-B31B1B.svg)](https://arxiv.org/abs/2304.10211)

<!--  
Conference   
-->   
</div>

## 🎊🎊🎊 Accepted as regular paper at CBMI 2023 🎊🎊🎊
 
## Description   
**Official Implementation of the paper:** "Spiking-Fer: Spiking Neural Network for Facial Expression Recognition With Event Cameras".

## Reproduction of the video-to-events converted datasets
For legal reason, we are not allowed to publicly provide modifications of the converted FER datasets. Unfortunately, researchers interested in working on the event-based versions of CK+, CASIA, MMI, or ADFES must **(1)** obtain the access to these datasets ; and **(2)** perform the steps described in this section.

### Access rights to the FER datasets
The original video datasets are available (upon request) on their respective website.
- **MMI**: https://mmifacedb.eu/
- **CASIA**: https://www.oulu.fi/en/university/faculties-and-units/faculty-information-technology-and-electrical-engineering/center-machine-vision-and-signal-analysis.
- **CK+**: http://www.jeffcohn.net/Resources/.
- **ADFES**: https://aice.uva.nl/research-tools/adfes-stimulus-set/adfes-stimulus-set.html?cb.

### Standardization of frames
We follow the same steps as described in previous papers such as https://www.sciencedirect.com/science/article/pii/S0925231222006610 . For a given frame (from a video sequence of one dataset), the steps of this standardization process are:
1. Detection of 68 facial landmarks
2. **Correction of head pose variations:** rotation of the frame so that the landmarks match the same position as those of the first frame of the video.
3. **Crop using the facial proportion:** the facial landmarks are used to measure the lengths of facial attributes (nose, eyes, etc). We use these lengths to only keep the face in the produced frame.
   - The crop is centered around the facial landmark that corresponds to the nose
   - **Height of the crop:**
     - Measure $nose$, the length of nose (using the landmarks)
     - The height of the crop is $2 \times \frac{2}{3} nose$
   - **Width of the crop:**
     - Measure $eyes$, the distance between the eyes (using the landmarks).
     - The width of the crop is $2 \times \frac{2}{3} eyes$
4. Resize the cropped frame to the dimension $(200 \times 200)$.

The resulting video sequence should be a folder that contains all the successive frames.

### Vid2Event Conversion
We employ the `v2e` project to simulate a DVS camera and converting a given standardized video. First, install the software (https://github.com/SensorsINI/v2e) or try the google colab version (https://colab.research.google.com/drive/1czx-GJnx-UkhFVBbfoACLVZs8cYlcr_M?usp=sharing).

For a given folder representing a video sequence, run the following command line:

```bash
v2e.py -i {{FOLDER_NAME}} -o {{OUTPUT_PARENT_FOLDER}} --overwrite --skip_video_output --unique_output_folder false --dvs_h5 {{OUTPUT_FILENAME}}.h5 --dvs_aedat2 None --dvs_text None --no_preview --dvs_exposure duration .033 --input_frame_rate 30 --input_slowmotion_factor 1 --slomo_model input/SuperSloMo39.ckpt --auto_timestamp_resolution true --pos_thres 0.2 --neg_thres 0.2 --sigma_thres 0.02 --cutoff_hz 0 --leak_rate_hz 0 --shot_noise_rate_hz 0 --output_width=200 --output_height=200
```

Repeat the whole process for all video sequences in the dataset, and place them in the corresponding folder of this project (e.g. `./data/ADFESDVS/`). For each dataset, a `folds.csv` file is available to reproduce the same folds as in our paper.


## How to launch an experiment
First, install dependencies **(for python 3.8.10**)
```bash
# clone project   
git clone https://github.com/Barchid/spiking-fer

# install project   
cd spiking-fer 
pip install -r requirements.txt
 ```   
The datasets must be placed in `data/`
Next, you can run a training experiment:
 ```bash
python train.py --dataset="CKPlusDVS" --mode="snn" --fold_number=0 --edas="flip,background_activity,crop,reverse,mirror,event_drop"
```

The best checkpoint file will be saved in `experiments/`.

## Citation   
```
@article{barchid2023spiking,
  title={Spiking-Fer: Spiking Neural Network for Facial Expression Recognition With Event Cameras},
  author={Barchid, Sami and Allaert, Benjamin and Aissaoui, Amel and Mennesson, Jos{\'e} and Dj{\'e}raba, Chaabane},
  journal={arXiv preprint arXiv:2304.10211},
  year={2023}
}
```   
