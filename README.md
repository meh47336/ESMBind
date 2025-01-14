# ESMBind

ESMBind ([preprint](https://www.biorxiv.org/content/10.1101/2024.08.09.607368v1)) is a deep learning and physics-based workflow for predicting metal-binding proteins and modeling their 3D structures with bound metal ions. It combines evolutionary scale modeling (ESM) for residue-level metal binding prediction with physics-based protein-metal modeling to generate detailed 3D structures of protein-metal complexes.

## Dependencies

- Python 3.10+
- PyTorch 2.0+
- Numba 0.60.0
- OpenMM 8.1.1
- AmberTools 23.3

We recommend using mamba to manage dependencies.

## Usage

ESMBind consists of two main components:

1. Deep learning model for residue-level metal binding prediction
2. 3D modeling of protein-metal complexes

### 1. Deep Learning Model

#### Inference

To use the pre-trained model for inference:

```bash
python multi_modal_binding/inference.py --config multi_modal_binding/configs/inference.json
```

We have provided pretrained model weights in the `multi_modal_binding/model/trained_weights` directory. The inference script will automatically load these weights for prediction.

After running the inference script, the predictions will be saved in a pickle file in the `multi_modal_binding/results/` directory, which will be used in the 3D modeling step.


#### Training

To train the model from scratch:

```bash
python multi_modal_binding/train.py --config multi_modal_binding/configs/training.json
```

### Example Data

Both the inference and training scripts assume that embeddings from ESM and ESM-IF are precomputed. The `multi_modal_binding/datasets` directory contains a sample dataset that can be used for both training and inference. Labels are not required for inference; any standard FASTA file will work.

We provide two example scripts, `get_esm_embedding.py` and `get_esm_if_embedding.py`, to generate embeddings for the input sequences and structures. Please refer to the official [ESM repository](https://github.com/facebookresearch/esm) for more details.

### 2. 3D Modeling

After obtaining residue-level predictions, use the following to generate 3D structures with placed metal ions:

```bash
cd 3D_modeling
bash run-3d-modeling.sh
```

Please revise the `run-3d-modeling.sh` script to specify the input and output directories before running. This script processes the deep learning predictions, places metal ions, and performs energy minimization to produce the final 3D structures.

## Data

This project uses data from:
- [Protein Data Bank](https://www.rcsb.org/)
- [AlphaFold Protein Structure Database](https://alphafold.ebi.ac.uk/)
- [BioLiP database](https://zhanggroup.org/BioLiP/index.cgi)

## License

This source code is licensed under the CSI approved 3-clause BSD license found in the LICENSE file in the root directory of this source tree.

## Citation
If you find our work useful, please cite our work as:
```
@article {ESMBind,
	author = {Dai, Xin and Henderson, Max and Yoo, Shinjae and Liu, Qun},
	title = {Predict metal-binding proteins and structures through integration of evolutionary-scale and physics-based modeling},
	elocation-id = {2024.08.09.607368},
	year = {2024},
	doi = {10.1101/2024.08.09.607368},
	publisher = {Cold Spring Harbor Laboratory},
	URL = {https://www.biorxiv.org/content/early/2024/08/10/2024.08.09.607368},
	journal = {bioRxiv}
}
```

## Contact

For questions or issues, please open an issue on GitHub or contact Xin Dai (xdai@bnl.gov).
