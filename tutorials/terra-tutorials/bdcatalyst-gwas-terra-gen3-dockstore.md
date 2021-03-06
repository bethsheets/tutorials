---
description: 'Powered by Dockstore, Terra, and Gen3'
---

# Genome Wide Association Study with TOPMed Data Tutorial

This page is an open access preview of the Terra tutorial workspace that you can find at this [link](https://terra.biodatacatalyst.nhlbi.nih.gov/#workspaces/biodata-catalyst/BioData%20Catalyst%20GWAS%20blood%20pressure%20trait).

## GWAS Tutorial Using Blood Pressure Traits

This template workspace was created to offer example tools for conducting a single variant, mixed-models GWAS focusing on a blood pressure trait from start to finish using the [NHLBI BioData Catalyst](https://biodatacatalyst.nhlbi.nih.gov/) ecosystem. We have created a set of documents [to get you started in the BioData Catalyst system](https://support.terra.bio/hc/en-us/sections/360008068731-BDC-category). If you're ready to conduct an analysis, proceed with this dashboard:

### Data Model

This template was set up to work with the NHLBI BioData Catalyst Gen3 data model. In this dashboard, you will learn how to import data from the Gen3 platform into this Terra template and conduct an association test using this particular data model.

#### TOPMed Data

Currently, BioData Catalyst's Gen3 hosts the [TOPMed](https://www.nhlbi.nih.gov/science/trans-omics-precision-medicine-topmed-program) program, which is controlled access. If you do not already have access to a TOPMed project through dbGAP, this template workspace may not yet be helpful to you. To apply for access to TOPMed, submit an application within [dbGAP](https://www.nhlbiwgs.org/topmed-data-access-scientific-community).

If you already have access to a TOPMEd project and have been onboarded to the BioData Catalyst platform, you should be able to access your data through BioData Catalyst powered by Gen3 and use your data with this template workspace. We focused this template on analyzing a blood pressure trait, but not all TOPMed projects may contain blood pressure data. You will need to carefully consider how to update this analysis for the dataset you bring and how this may affect the scientific accuracy of the question you are asking.

#### A note about TOPMed metadata

Some types of metadata will always be present: GUID, Case ID, Project Name, Number of Samples, Study, Gender, Age at Index, Race, Ethnicity, Number of Aliquots, SNP Array Files, Unaligned Read Files, Aligned Read Files, Germline Variation Files.

Other metadata depend on the analysis plan submitted when applying for TOPMed access. Examples include BMI, Years Smoked, years smoked greater than 89, hypertension, hypertension medications, diastolic blood pressure, systolic blood pressure, etc.

The TOPMed Data Coordinating Center \(DCC\) is currently harmonizing select phenotypes across TOPMed, which will also be deposited into the TOPMed accessions. The progress of phenotype metadata harmonization can [be tracked here](https://topmedphenotypes.org/). The data in the Gen3 graph model in this tutorial are harmonized phenotypes. You can find the unharmonized phenotypic and environmental data in the "Reference File" node of the Gen3 graph. Documentation about how to interact with unharmonized data in Terra is coming soon.

### Outline of this template:

_**Part 1: Navigate the BioData Catalyst environment**_ Learn how to search and export data from Gen3 and workflows from Dockstore into a Terra workspace. Each cloud-based platform interoperates with one another for fast and secure research. The template we have created here can be cloned for you to walk through as suggested, or you can use the basics you learn here to perform your own analysis.

_**Part 2: Explore TOPMed data in an interactive Jupyter notebook**_ In this Terra workspace, you can find a series of interactive notebooks to explore TOPMed data.

First, you will use the notebook **1-unarchive-vcf-tar-file-to-workspace** to extract the contents of tar bundles to your workspace for use in the GWAS. These tar bundles were generated by dbGAP and contain TOPMed multi-sample VCFs per consent code and chromosome.

Next, the **2-GWAS-preliminary-analysis** notebook will lead you through a series of steps to explore the phenotypic and genotypic data. It will call the functions in the companion **terra-data-util** notebook to consolidate your clinical data from Gen3 into a single data table that can be imported into the Jupyter notebook. Then you will examine phenotypic distributions and genetic relatedness using the [HAIL genomic data analysis tool](https://hail.is/).

_**Part 3: Perform mixed-model association tests using workflows**_ Next, perform mixed models genetic association tests \(run as a series of batch workflows using GCP Compute engine\). For details on the four workflows and what they do, scroll down to **Perform mixed-model association test workflows**. The workflows are publicly available in [Dockstore](https://dockstore.org/) in this [collection](https://dockstore.org/organizations/bdcatalyst/collections/GWAS).

Mixed models require two steps within the [GENESIS](https://bioconductor.org/packages/release/bioc/html/GENESIS.html) package in [R](https://www.r-project.org/about.html):  
1\) Fitting a null model assuming that each genetic variant has no effect on phenotype and 2\) Testing each genetic variant for association with the outcome, using the fitted null model.

## Part 1: Navigate the NHLBI BioData Catalyst multi-platform ecosystem

### 1a\) Link your Terra account to external services

Before you're able to access genomic data from Gen3 in the Terra data table, you need to link your Terra account to external services. Link your profile [by following these instructions](https://support.terra.bio/hc/en-us/articles/360038086332?flash_digest=2f492682b688b21da27c701af68656ac095d5803).

### 1b\) Create an Authorization Domain to protect your controlled-access data

Because this workspace was created to be used with controlled access data, it should be registered under an Authorization Domain that limits its access to only researchers with the appropriate approvals. Learn how to set up an Authorization Domain [here](https://support.terra.bio/hc/en-us/articles/360039415171) before proceeding.

### 1c\) Export a TOPMed project with blood pressure data from Gen3

1. Start by learning about Gen3's graph-structured data model for NHLBI's BioData Catalyst using this [orientation document](https://support.terra.bio/hc/en-us/articles/360038087312).
2. Once you better understand the graph, log into [Gen3](https://gen3.biodatacatalyst.nhlbi.nih.gov/) through the NIH portal using your eRA Commons username and password. 
3. Navigate to the [Gen3 Explorer](https://gen3.biodatacatalyst.nhlbi.nih.gov/explorer) view to see what datasets you currently have and do not have access to. On the left-hand side, you can use the faceted search tool to narrow your results to specific projects. 
4. First, under "Files" and "Access", select "Data with Access" to filter through projects that you currently have access to.
5. Next, under "Filters", you can select phenotypic or environmental data to narrow your results. Here, select the "Diagnosis" tab and under "BP Diastolic" move the left hand side of the sliding bar from 0 to 35.  This will make your search range 35 - 163. This will only show the TOPMed projects that contain that trait data. 
6. In all of TOPMed, there are 23 studies with diastolic blood pressure data. You may see anywhere from 0 to 23, depending on what projects you have applied for and received access to. 
7. Next, click on the "Subject" tab. If you have access to a TOPMed project with blood pressure data, this will list all of the project names. Select only a single project to use in this template. 
8. Once selected, click the button "Export all to Terra", wait until the Terra window appears, and add your data to your copy of this template workspace. 

### 1d\) Select a workspace for your data

Once the new Terra window appears, you are given a few options for where to place your data.

1\) "Start with a template" This feature allows you to import data directly into a template workspace that has everything set up for you to do an analysis but does not contain any data. Once you select a workspace, you will need to enter:

* Workspace name: Enter a name that is meaningful for your records.
* Billing Project: Select the billing projects available to you. If you are a new user, you can use the [$300 of free credits offered](https://support.terra.bio/hc/en-us/articles/360027940952-Free-credits-FAQs).
* Authorization Domain: Assign the authorization domain that you generated above to protect your data. This is important for working with controlled access. data. 

2\) "Start with an existing workspace" If you have already created a workspace, you can import your data directly to this workspace.

3\) "Start a new workspace" This will create an empty workspace. You can individually copy notebooks and workflows from other workspaces, import workflows from Dockstore, or start fresh.

## Part 2: Explore TOPMed data in Jupyter Notebooks

### 2a\)  Extract multi-sample VCFs to your workspace

Gen3 uploaded tar compressed bundles, as they are provided by dbGAP, into cloud buckets owned by BioData Catalyst. To make these tar files actionable and ready for use in analyses, users will need to unarchive these tar bundlers to their workspace.

First, open the _**1-unarchive-vcf-tar-file-to-workspace**_ notebook and follow the steps to select which tar bundle\(s\) to extract to your workspace for use in the GWAS. Please understand that this step may be time consuming since TOPMed multi-sample VCF files are several hundred gigabytes in size.

### 2b\) Prepare your phenotypic and genotypic data for input into association test workflows

Now that you can interact with the Gen3 structured data more easily, you will use an interactive notebook to explore your phenotypic and environmental data and performs several analyses to prepare the data for use in batch association workflows.

1. [Learn how to customize your interactive analysis compute](https://support.terra.bio/hc/en-us/articles/360038125912) to work with the data you imported. 
2. Open the **2-GWAS-preliminary-analysis** notebook and set your runtime configuration. We have given a suggested configuration within the notebook.
3. From within this **2-GWAS-preliminary-analysis** notebook you can call functions from the companion **terra\_data\_table\_util** notebook to reformat multiple data tables into a single data table that can be loaded as a dataframe in the notebook.
4. Subset the dataframe to include only your traits of interest and remove any individuals that lack data for these traits.
5. Visualize phenotype and environmental variable distributions in a series of plots.
6. Import the multi-sample VCF from the "Reference File" data table using DRS. You can learn more about GA4GH's Data Repository Service [here](https://support.terra.bio/hc/en-us/articles/360039330211).
7. Filter your VCF to only common variants to increase statistical power. Genetic analyses in this notebook utilize the [Hail software](https://hail.is/). Hail is a framework for distributed computing with a focus on genetics. Particularly relevant for whole genome sequence \([WGS](https://en.wikipedia.org/wiki/Whole_genome_sequencing)\) analysis, Hail allows for efficient, nearly boundless computing \(in terms of variant and sample size\).    
8. Perform a principal component analysis \([PCA](https://en.wikipedia.org/wiki/Principal_component_analysis)\) to assess population stratification. Genetic stratification can strongly affect association tests and should be accounted for.
9. Generate a genetic relatedness matrix \([GRM](https://hail.is/docs/0.2/methods/genetics.html?highlight=pc_rel#hail.methods.genetic_relatedness_matrix)\) to account for closely related individuals in your association testing workflows.
10. Generate a new "sample\_set" data table that holds the derived files we created in the steps above using the [FireCloud Service Selector \(FISS\) package](https://github.com/broadinstitute/fiss).  The files in this data table will be used in the workflows we run in Part 3.     

#### Time and cost estimate

You can adjust the runtime configuration to fit your computational needs in the Jupyter notebook. We recommend selecting the default environment and selecting the custom profile to use and configure the spark cluster for parallel processing. Using the profile suggested profile within the Jupyter notebook and a project with around 1000 samples, running this notebook on this dataset takes about 90 minutes and $20/hr to compute.

When working in a notebook with computing times over 30 minutes, learn more about Terra's [auto-pause feature](https://support.terra.bio/hc/en-us/articles/360029761352) and [how to adjust auto-pause](https://support.terra.bio/hc/en-us/articles/360027020412) for your needs. Please carefully consider how adjusting auto-pause can remove protections that help you from accidentally accumulating cloud costs that you did not need.

## Part 3: Perform mixed-model association tests using workflows

In Part 2, we explored the data we imported from Gen3 and performed a few important steps for preparing our data for association testing. We generated a new "sample\_set" data table that holds the files we created in the interactive notebook. These files will be used in our batch workflows that will perform the association tests. Below, we describe the four workflows in this workspace and their cost estimates for running on the sample set we create in this tutorial.

The workflows used in this template were imported from [Dockstore](https://www.dockstore.org) and their parameters were configured to work with Terra's data model. If you're interested in searching other Docker-based workflows, [learn more about how they can easily be launched in Terra](https://support.terra.bio/hc/en-us/articles/360038137292).

### Notes on how attributes are set in workflows

We have set the input and output attributes for each workflow in this template. Before running the first workflow, you can look through the inputs and outputs of each workflow and see that outputs from the first workflow feed into the second workflow, and so on.

In the 2-GWAS-preliminary-analysis notebook, we created a Sample Set data table that holds a row called "systolicbp" which contains the input files for the following workflows. You can check this data table out in the Data tab of this workspace. When you open a workflow, make sure that "Sample Set" is set and the "systolicbp" \(or whatever you named your run\) is selected before running a workflow.

**1-vcfToGds**

This workflow converts genotype files from Variant Call Format \([VCF](https://en.wikipedia.org/wiki/Variant_Call_Format)\) to Genomic Data Structure \([GDS](https://si.biostat.washington.edu/sites/default/files/modules/GDS_intro.pdf)\), the input format required by the R package GENESIS.

**Time and cost estimates**

| Sample Set Name | Sample Size | \# Variants | Time | Cost $ |
| :--- | :--- | :--- | :--- | :--- |
| systolicbp | 1,052 samples | 6,429,788 | 15m | $1.01 |

Inputs:

* VCF  genotype file \(or chunks of VCF files\)

Outputs:

* GDS genotype file

**2-genesis\_GWAS**

This workflow creates a null model from phenotype data with the GENESIS biostatistical package. This null model can then be used for association testing. This workflow also runs single variant and aggregate test for genetic data. Implements Single-variant, Burden, SKAT, SKAT-O and, SMMAT tests for Continuous or Dichotomous outcomes. All tests account for familiar relatedness through kinship matrixes. Underlying functions adapted from: Conomos MP and Thornton T \(2016\). GENESIS: GENetic EStimation and Inference in Structured samples \(GENESIS\): Statistical methods for analyzing genetic data from samples with population structure and/or relatedness. R package version 2.3.4.

**Time and cost estimates**

| Sample Set Name | Sample Size | Time | Cost |
| :--- | :--- | :--- | :--- |
| systolicbp | 1,052 samples | 26m | $0.94 |

Inputs:

* GDS genotype file
* Genetic Relatedness Matrix
* Trait outcome name
* Trait outcome type
* CSV file of covariate traits
* Sample ID list

Outputs:

* A null model as an RData file
* Compressed CSV file\(s\) containing raw results
* CSV file containing all associations
* CSV file containing top associations
* PNG file of Quantile-Quantile and Manhattan plots

## Cost Examples

Below are reported costs from using 1,000 and 10,000 samples to conduct a GWAS using the BioData Catalyst GWAS Blood Pressure Trait template workspace. The costs were derived from single variant tests that used Freeze 5b VCF files that were filtered for common variants \(MAF &lt;0.05\) for input into workflows. The way these steps scale will vary with the number of variants, individuals, and parameters chosen. TOPMed Freeze 5b VCF files contain 582 million variants and Freeze 8 increases to ~1.2 billion. For GWAS analyses with Freeze 8 data, computational resources and costs are expected to be significantly higher.

| Analysis Step | Cost \(n=1,000; Freeze5b\) | Cost \(n=10,000; Freeze 5b\) |
| :--- | :--- | :--- |
| GWAS Preliminary Analysis Notebook | $29.34 \($19.56/hr for 1.5 hours\) | $336 \($56/hr for 6 hours\) |
| vcfTogds workflow | $1.01 | $5.01 |
| genesis\_GWAS workflow | $0.94 | $6.67 |
| TOTAL | $32.29 | $347.68 |

These costs were derived from running these analyses in Terra in June 2020.

## Optional: Bring your own data

Both the notebook and workflow can be adapted to other genetic datasets. The steps for adapting these tools to another dataset are outlined below:

_**Update the data tables**_ Learn more about uploading data to Terra [here](https://support.terra.bio/hc/en-us/articles/360025758392-Managing-data-with-the-data-table-). You can use functions available from the terra\_data\_table\_util companion notebook to consolidate new data tables you generate.

_**Update the notebook**_ Accommodating other datasets may require modifying many parts of this notebook. Inherently, the notebook is an interactive analysis where decisions are made as you go. It is not recommended that the notebook be applied to another dataset without careful thought.

_**Run an additional workflow**_ You can search [Dockstore](https://www.dockstore.org) for available workflows and export them to Terra following [this method](https://docs.dockstore.org/en/develop/launch-with/terra-launch-with.html).

#### Helpful resources to master this tutorial

* If you are new to BioData Catalyst powered by Terra, we have created an [onboarding syllabus](https://bdcatalyst.gitbook.io/biodata-catalyst-documentation/analyze-data/terra/onboarding-syllabus-using-gen3-terra-dockstore-and-pic-sure) that includes several introductory webinars.
* [Controlling cloud costs](https://support.terra.bio/hc/en-us/articles/360029748111)
* [Intro to Jupyter notebooks in Terra](https://app.terra.bio/#workspaces/help-gatk/Jupyter%20Notebooks%20101)
* [Intro to Hail using a Terra workspace](https://app.terra.bio/#workspaces/help-gatk/Hail-Notebook-Tutorials)
* [GWAS tutorial using open data from the 1000 Genomes Project](https://app.terra.bio/#workspaces/broad-t2d-dev/Running_GWAS_in_Terra)

#### Authors, contact information, and funding

This template was created for the [NHLBI's BioData Catalyst](https://biodatacatalyst.nhlbi.nih.gov/) project in collaboration with the [Computational Genomics Platform](https://cgpgenomics.ucsc.edu/) at [UCSC Genomics Institute](https://ucscgenomics.soe.ucsc.edu/) and the [Data Sciences Platform](https://www.broadinstitute.org/data-sciences-platform) at [The Broad Institute](https://www.broadinstitute.org/). The association analysis tools were contributed by the [Manning Lab](https://manning-lab.github.io/).

Contributing authors include:

* [Beth Sheets](mailto:esheets@ucsc.edu) \(UC Santa Cruz Genomics Institute\)
* Michael Baumann \(Broad Institute, Data Sciences Platform\)
* Brian Hannafious \(UC Santa Cruz Genomics Institute\)
* [Tim Majarian](mailto:tmajaria@broadinsitute.org) \(Manning Lab\)
* Alisa Manning \(Manning Lab\)
* Ash O'Farrell \(UC Santa Cruz Genomics Institute\)

#### Workspace change log

| Date | Change | Author |
| :--- | :--- | :--- |
| December 9, 2020 | Update notebooks, workflows, and workspace markdown | Ash |
| June 26, 2020 | terra\_data\_table\_util updates | Beth |
| Feb 26, 2020 | Added notebook to copy/extract VCF | Beth |
| Jan 31, 2020 | Replaced text with new Broad documentation | Beth |
| Jan 30, 2020 | Template updates | Beth |
| Jan 3, 2020 | Updates from BDC F2F | Beth |
| December 3, 2019 | Gen3 updates | Beth |
| November 22, 2019 | Updates from Alisa | Beth |
| October 22, 2019 | User experience edits from Beri | Beth |

