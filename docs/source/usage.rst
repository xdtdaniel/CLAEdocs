Usage
=====

.. _installation:

Installation
------------

To use CLAE, please make sure you have Python 3.6-3.9. It will **not** work on any version that are older than 3.6.

Please first install the following dependencies:

.. code-block:: console

   (.venv) $ pip install pandas biopython tqdm --user



You also need to install these third party tools.

`BLAT: https://genome.ucsc.edu/FAQ/FAQblat.html <https://genome.ucsc.edu/FAQ/FAQblat.html>`_
`BLAST: https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download<https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download>`_
`Blasr: https://github.com/PacificBiosciences/blasr<https://github.com/PacificBiosciences/blasr>`_
`Sparc: https://github.com/yechengxi/Sparc<https://github.com/yechengxi/Sparc>`_

After that, you can clone our repository.

.. code-block:: console

   (.venv) $ git clone https://github.com/xdtdaniel/SequenceExtractionAndConsensusFinding.git
   (.venv) $ cd SequenceExtractionAndConsensusFinding/mp_script/

Quickstart
----------------

In our git repository, we have the sample data for you to verify that your setup is correct.

You can download the sample data here:

`https://github.com/xdtdaniel/SequenceExtractionAndConsensusFinding/tree/main/sample_data <https://github.com/xdtdaniel/SequenceExtractionAndConsensusFinding/tree/main/sample_data>`_

You will see three input files here:

- RCArun.fasta (RCA reads in fasta format)
- Anchors_SMRTbell.fasta (The anchor sequences in fasta format)
- Target_Lambda_NEB.fasta (The target sequences in fasta format)

Please put the sample data into the folder **mp_script**.

To run the first step and generate the BLAST output, please run

.. code-block:: console

   (.venv) $ python blast_generator.py --dbseqin Anchors_SMRTbell.fasta --query RCArun.fasta --out RCABlastn_Sample

You will then see the output file **RCABlastn_Sample.csv**.

From there, we will proceed to our second step of generating subreads and consensus results.

For the Quickstart, we will use Sparc algorithm for both reference based consensus generation and non-reference based consensus generation.

Reference Based
^^^^^^^^^^^^^^^^

.. code-block:: console

   (.venv) $ python clae.py --blast RCABlastn_Sample.csv --seq RCArun.fasta --ref --refseq Target_Lambda_NEB.fasta --algo s --merge

For the output, you will find a folder like **RCArun.fasta_20211216-141253_files** (time may vary).

Here we assume that we are using a 40-core machine.

.. code-block:: console

   RCArun.fasta_20211216-141253_files/
   ├── blastn
   │   ├── blastn_0.csv
   │   ├── ...
   │   ├── blastn_39.csv
   ├── consensus.log
   ├── results
   │   ├── no_ref
   │   │   └── fasta
   │   └── ref
   │       ├── fasta
   │       │   └── consensus_ref_s_2021-12-16_RCArun.fasta
   │       ├── Result_sparc_0_40_RCArun.fasta.csv
   │       ├── Result_sparc_0.csv
   │       ├── ...
   │       └── Result_sparc_39.csv
   └── temp_df
      ├── df_w_seqs_no_blat_0.csv
      ├── ...
      ├── df_w_seqs_no_blat_39.csv
      ├── lseqs_df_0.csv
      ├── ...
      ├── lseqs_df_39.csv
      ├── Q_trimming_0.csv
      ├── ...
      └── Q_trimming_39.csv

The consensus results in fasta format can be found at RCArun.fasta_20211216-141253_files/results/ref/fasta/consensus_ref_s_2021-12-16_RCArun.fasta.
The temporary files are remained for debug purposes.

Non-Reference Based
^^^^^^^^^^^^^^^^

.. code-block:: console

   (.venv) $ python clae.py --blast RCABlastn_Sample.csv --seq RCArun.fasta --algo s --merge

For the output, you will find a folder like **RCArun.fasta_20211216-141253_files** (time may vary).

Here we assume that we are using a 40-core machine.

.. code-block:: console

   RCArun.fasta_20211216-141253_files/
   ├── blastn
   │   ├── blastn_0.csv
   │   ├── ...
   │   ├── blastn_39.csv
   ├── consensus.log
   ├── results
   │   ├── no_ref
   │   │   ├── fasta
   │   │   │   └── consensus_no_ref_s_2021-12-16_RCArun.fasta
   │   │   ├── Result_sparc_0_40_RCArun.fasta.csv
   │   │   ├── Result_sparc_0.csv
   │   │   ├── ...
   │   │   └── Result_sparc_39.csv
   │   └── ref
   │       └── fasta
   └── temp_df
      ├── df_w_seqs_no_blat_0.csv
      ├── ...
      ├── df_w_seqs_no_blat_39.csv
      ├── lseqs_df_0.csv
      ├── ...
      ├── lseqs_df_39.csv
      ├── Q_trimming_0.csv
      ├── ...
      └── Q_trimming_39.csv

The consensus results in fasta format can be found at RCArun.fasta_20211216-141253_files/results/no_ref/fasta/consensus_ref_s_2021-12-16_RCArun.fasta.
The temporary files are remained for debug purposes.


