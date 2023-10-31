# Enformer variant score

This repo help computing the enformer variant score described in the [original paper](https://www.nature.com/articles/s41592-021-01252-x).

The notebook here is a copy of the example one provided with the paper in google colab

You can use the yml file to generate a compatible conda env

Refer to the [original repository](https://github.com/google-deepmind/deepmind-research/tree/master/enformer) for more information and licensing.

## Predict variants

You can use the `predict_variants.py` script to predict variants from a VCF file or a list of variants IDs.

### Usage

You need cuda and cudnn libraries installed on your system. In our HPC you can load them using `module load cuda/11.5.1 cudnn/8.3.3.40-11.5-cuda-11.5.1`. A conda environment is already available, you have to activate it using `conda activate enformer_20231026`.

In most cases, you just need to provide you input using `--vcf` and/or `--variants` and then set the output folder using `--output`. The script will create a folder with the name you provided and will save the results there.

Input VCF can be uncompressed or gzipped. A list of variants can be provided as comma-separated list or as a file with one variant per line. The format of the variants must be CHR_POS_REF_ALT(_ID). If you use only CHR_POS_REF_ALT this will be use as variant ID. If you want to add the chr prefix to the chromosome names present in the input variatns, use the `--add_chr_prefix` argument.

```bash
usage: enformer_predict.py [-h] [--vcf VCF] [--variants VARIANTS] [--output OUTPUT]
	--ref_genome REF_GENOME [--tfhub_url TFHUB_URL]
    [--transform_pkl_path TRANSFORM_PKL_PATH] [--plot_features PLOT_FEATURES]
	[--add_chr_prefix]

optional arguments:
  -h, --help            show this help message and exit
  --vcf VCF             VCF.gz file with variants to predict
  --variants VARIANTS   List of variants id to predict in the foramt CHR_POS_REF_ALT(_ID). Can be a
                        comma-separated list or a file with one variant per line.
  --output OUTPUT       Output folder
  --ref_genome REF_GENOME
                        Reference genome fasta file. A fai index must be present for this file.
  --tfhub_url TFHUB_URL
  --transform_pkl_path TRANSFORM_PKL_PATH
  --plot_features PLOT_FEATURES
                        Comma separated list of feature indexes to use in output. Use all to plot for
                        all features.
  --add_chr_prefix      add chr prefix to chromosome names when reading input variants
```

By default, the script searches the model pickle object in the folder model in the same folder of the script. You can change this behaviour using the `--transform_pkl_path` argument.

## Plot features

You can generate plots of REF and ALT-REF predictions for each input variants by setting `--plot_features` providing the index of the features you want to plot. The list of available features with their indexes are provided in model/targets_human.txt. All features will be plotted together so requesting an excessive number of features will result in a very crowded plot.

Plots will be saved in the plots folder under the main output folder with the name `variant_id.png`.

## Citation

If you use this code, please cite the original paper:

Effective gene expression prediction from sequence by integrating long-range interactions

Žiga Avsec, Vikram Agarwal, Daniel Visentin, Joseph R. Ledsam, Agnieszka Grabska-Barwinska, Kyle R. Taylor, Yannis Assael, John Jumper, Pushmeet Kohli, David R. Kelley