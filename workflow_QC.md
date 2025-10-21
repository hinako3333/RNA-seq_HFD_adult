# RNA-seq QC Workflow

## Step 1: Activate environment  
```bash
conda activate rnaseq
```

## Step 2: Move to the project root directory  
```bash
cd ~/Downloads/RNA-seq_HFD_adult/sequence
```

## (Optional 1) Set proxy environment variables & Conda proxy config  
```bash
# --- proxy for command line & conda ---
export HTTPS_PROXY="http://proxy.l2.med.tohoku.ac.jp:8080"
export HTTP_PROXY="$HTTPS_PROXY"

# Configure Conda to use the proxy
conda config --set proxy_servers.https "http://proxy.l2.med.tohoku.ac.jp:8080"
conda config --set proxy_servers.http  "http://proxy.l2.med.tohoku.ac.jp:8080"
```

## (Optional 2) Setup a dedicated environment for MultiQC  
```bash
# Ensure conda-forge and strict channel priority for compatibility
conda config --add channels conda-forge
conda config --set channel_priority strict

# Create Python venv and install MultiQC via pip
python3 -m venv ~/venvs/multiqc
source ~/venvs/multiqc/bin/activate
python -m pip install --upgrade pip
pip install "multiqc>=1.25.1"
multiqc fastqc_results -o multiqc_report
deactivate
```

## Step 4: Run FastQC on all gzipped FASTQ files  
```bash
fastqc -t 4 -o fastqc_results raw_data/*.fastq.gz
```
* `-t 4` uses 4 threads.  
* Output is stored in the folder `fastqc_results/`.

## Step 5: Run MultiQC to summarise QC outputs  
```bash
multiqc fastqc_results -o multiqc_report
```
* This creates the folder `multiqc_report/`.  
* Inside you’ll find the HTML report (e.g., `multiqc_report.html` or `multiqc_report_1.html`) and a data folder (e.g., `multiqc_data/` or `multiqc_data_1/`).

## Step 6: Open the MultiQC report (macOS)  
```bash
open multiqc_report/multiqc_report.html
```

## Directory structure (for reference)  
```
project_root/
  raw_data/
    *.fastq.gz
  fastqc_results/
    *.html  *.zip  (FastQC outputs)
  multiqc_report/
    multiqc_report.html     (or multiqc_report_1.html)
    multiqc_data/           (or multiqc_data_1/)
```