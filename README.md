# nf-core/rnaseq (optimized for the execution on the Seven Bridges platform)

## Introduction

The **nf-core/rnaseq** (v 3.12.0) pipeline is the most utilized nf-core workflow and has received a considerable amount of support from the community in terms of development. In the nf-core public apps gallery this is by far the most utilized worfklow which is updated regularly as well.

Related to this, our team decided to optimize this pipeline for the execution on the **Seven Bridges** platform. This document is intended as a guidance to put and run **nf-core/rnaseq** pipeline on the platform (note that this procedure can be used for other pipelines as well).

## Deploying Nextflow applications on Seven Bridges Platform

The nextflow pipelines can be delivered to SevenBridges platform in a two ways:

* Via command line interface (CLI) by using **sbpack** package.
* Via platform general user interface (GUI) - If you want to use this method refer to Seven Bridges documentation[https://docs.sevenbridges.com/docs/add-nextflow-apps-through-the-visual-interface].


### CLI for deploying Nextflow pipelines onto the platform


To put nextflow pipelines on the platform via CLI you need to install ```sbpack``` on your local machine. To do this type the following command:

```pip install sbpack```

If you want to install specific version of sbpack type:

```pip install sbpack==v2024.8.28rc1```

We encourage you to install and use the latest version of ```sbpack```.

If you want to put this pipeline onto platform you should do the following:

  1. ```git clone git@github.com:DarkoCucin/nf-core-rnaseq-3-12-0-sbg.git```- this command will clone repository to your local machine.
  2. ```sbpack_nf --appid APPID --workflow-path WORKFLOW_PATH --sb-schema sb_nextflow_schema.yaml --execution-mode multi-instance --revision-note 'first revision'``` - In the command above, replace the placeholders as follows:
  * ```--appid``` - parameter that specifies the identifier of the app on the platform, in the ```{user or division}/{project}/{app_name}``` format. If you are using [Enterprise](https://docs.sevenbridges.com/docs/about-the-enterprise-feature), the ```{user or division}``` part is name of your Division on the platform; otherwise, specify your platform username. The ```{project}``` part is the project to which you want to push the app and ```{app_id}``` is the ID you want to assign to the app. For example the full app ID can be ```my-division/my-new-project/my-nextflow-app```. If the specified app ID does not exist, it will be created. If it exists, a new [revision (version)](https://docs.sevenbridges.com/docs/app-versions) of the app will be created.
  * ```--worfklow-path``` - parameter which specifies the path where the **Nextflow** app files are located on your local machine.
  * ```--execution-mode``` - execution mode for your application. Possible values are: 

       * ```single-instance``` - the mode that was used in the earliest **Nextflow** implementation on the platform. Using this mode is highly discouraged.

       * ```multi-instance``` - the mode which uses more instances for one task. Implemented in the latest **Nextflow** implementation on the platform.
  * ```--sb-schema``` - parameter which specifies a path to an existing ```sb_nextflow_schema``` file in JSON or YAML format. In this case we provided  ```sb_nextflow_schema.yaml``` file for you. If you want to make ```sb_nextflow_schema```from scratch refer to this [put the link later]()


### Running nf-core/rnaseq on the platform

When you put **nf-core/rnaseq** pipeline onto the platform it is time to run this pipeline. This pipeline has the following required inputs (if ```test``` or ```test_full``` profile option is not used):

1. **Samplesheet file** (```--input```) - This is file which specifies read files needed for running pipeline. If you want to learn more about specifying this file refer to [nf-core/rnaseq documentation](https://nf-co.re/rnaseq/3.12.0/docs/usage/#samplesheet-input).
2. If **Igenomes** (```--genome```) parameter is not specified the following input files are required:
   * **FASTA genome file** (```--fasta```) - Fasta file with specified genome.
   * Annotation file - a gene annotation file in GTF or GFF3 format (`--gtf` or `--gff`)  corresponding to the **Genome FASTA** (`--fasta`) file.
3. **Output directory** (```--outdir```) - Specify the name of output directory where the pipeline results will be stored

As we mentioned above this pipeline is optimized to run on the Seven Bridges platform. In the ```Profiles``` input you can use the following profiles to optimize exectuon:

1. ```sbg_full``` - This configuration is tailored to run pipeline efficiently with the ```test_full``` and similar datasets. The suggested number of parallel instances  is 10.
2. ```sbg_large_number_samples``` - This configuration is made to run pipeline efficiently with the larger number of samples. It is optimized with the dataset which consists of 78 paired-end samples with the average size of 4.5GB per sample. The suggested number of parallel instances is variable but it is advisable to increase this number by one on every 10th sample.
