# NGS Mismatch Analysis in Fission Yeast

## Title and Purpose

This Python program analyses next-generation sequencing (NGS) data from *Schizosaccharomyces pombe* to investigate whether exposure to a genotoxic agent affects the frequency, distribution and types of nucleotide mismatches.

## How to Run the Program

Open the notebook in Google Colab and select **Runtime → Run all**. The setup cell automatically downloads the required data using `wget`, so no manual uploads are needed.

## Inputs and Outputs

The program uses `reference.txt`, containing the reference DNA sequence, and `reads.csv`, containing sequencing reads, read identifiers, sample names and experimental groups.

It returns annotated reads, sample-level statistics and statistical comparison dictionaries. Printed outputs include valid and invalid read counts, mismatch percentages, substitution frequencies, top mismatch positions and Welch's t-test results.

## Code Design

I separated calculations from printing using `print_table()` and `print_sample_summaries()`. Rather than embedding formatting within `summarise_samples()`, these helpers reduced repetition and made calculations easier to understand, test and modify independently.

I used a dictionary for combined mismatch counts rather than a fixed-length array because the sample summary does not contain the reference length. This avoids hard-coding sequence lengths and storing positions without mismatches.

I introduced `get_top_positions()` to share ranking and tie-breaking logic between `summarise_samples()` and `compare_positions()`. Rather than repeating the sorting code, a shared function ensures consistent position selection and reduces the risk of errors during modifications.

## Testing

Three tests verified correctness and robustness:

- Hand-made sample C3 returned `[4, 1, 2]`, confirming that frequency takes priority and earlier positions resolve ties.
- Hand-made all-invalid sample C2 returned `None` and displayed N/A, distinguishing unavailable percentages from genuine zero values.
- `welch_df()` was checked against independently calculated values of `4.0` and `50/17`, verifying degrees of freedom for equal and unequal variances.
