# dbsnp MAF extraction using NCBI entrez

This script extracts Minor Allele Frequency (MAF) values for SNP IDs from dbSNP using the NCBI Entrez E-utilities API. It is designed for SNP analysis, particularly for patient sample studies.

## Prerequisites

Before running the script, ensure you have the following Python libraries installed:

- `requests`
- `xml.etree.ElementTree` (part of Python's standard library)
- `pandas`

You can install the required libraries using the following command:

```bash
pip install requests pandas

