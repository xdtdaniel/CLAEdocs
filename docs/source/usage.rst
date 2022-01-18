Usage
=====

.. _installation:

Installation
------------

To use CLAE, please make sure you have Python 3.6-3.9. It will **not** work on any version that are older than 3.6.

Please first install the following dependencies:

.. code-block:: console

   (.venv) $ pip install pandas biopython tqdm --user


The version below is for reference. You may be able to run it with newer version of those packages.

- python>=3.7
- numpy==1.17.2
- Bio==0.4.1
- pandas==1.2.4
- tqdm==4.60.0


You also need to install these third party tools. You may need Python 3.7+ and Conda environment to perform the installation below.

You can download the source code and compile it yourself. You can also download the pre-compiled executable directly that meets your platform.

`BLAT <https://genome.ucsc.edu/FAQ/FAQblat.html>`_

`BLAST <https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download>`_

`Blasr <https://github.com/PacificBiosciences/blasr>`_

`Sparc <https://github.com/yechengxi/Sparc>`_

`pbdagcon <https://github.com/PacificBiosciences/pbdagcon>`_

ALL of the above are required.

After that, you can clone our repository.

.. code-block:: console

   (.venv) $ git clone https://github.com/xdtdaniel/CLAE
   (.venv) $ cd CLAE/mp_script/

Quickstart
----------------

In our git repository, we have the sample data for you to verify that your setup is correct.

You can download the sample data here:

`https://github.com/xdtdaniel/CLAE/tree/main/sample_data <https://github.com/xdtdaniel/CLAE/tree/main/sample_data>`_

You will see three input files here:

- RCArun.fasta (RCA reads in fasta format)
- Anchors.fasta (The anchor sequences in fasta format)
- Lambda_NEB.fasta (The target sequences in fasta format)

Please put the sample data into the folder **mp_script**.

To run the first step and generate the BLAST output, please run

.. code-block:: console

   (.venv) $ python blast_generator.py --dbseqin Anchors_SMRTbell.fasta --query RCArun.fasta --out RCABlastn_Sample

You will then see the output file **RCABlastn_Sample.csv**.

If you'd like to use your own blast results, you can skip the step above. We also include a sample for blast file `here <https://github.com/xdtdaniel/CLAE/blob/main/sample_data/Blast_result.csv>`_

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
   ├── consensus.log
   └── results
       ├── subreads_0_40_RCArun.fasta.csv
       ├── ref
       │   ├── fasta
       │   │   └── consensus_ref_s_2021-12-16_RCArun.fasta
       │   └── Result_sparc_0_40_RCArun.fasta.csv
       └── no_ref
           └── fasta

The consensus results in fasta format can be found at RCArun.fasta_20211216-141253_files/results/ref/fasta/consensus_ref_s_2021-12-16_RCArun.fasta.
The subread file can be found at RCArun.fasta_20211216-141253_files/results/subreads_0_40_RCArun.fasta.csv.

Non-Reference Based
^^^^^^^^^^^^^^^^

.. code-block:: console

   (.venv) $ python clae.py --blast RCABlastn_Sample.csv --seq RCArun.fasta --algo s --merge

For the output, you will find a folder like **RCArun.fasta_20211216-141253_files** (time may vary).

Here we assume that we are using a 40-core machine.

.. code-block:: console

   RCArun.fasta_20211216-141253_files/
   ├── consensus.log
   └── results
       ├── subreads_0_40_RCArun.fasta.csv
       ├── no_ref
       │   ├── fasta
       │   │   └── consensus_no_ref_s_2021-12-16_RCArun.fasta
       │   └── Result_sparc_0_40_RCArun.fasta.csv
       └── ref
           └── fasta

The consensus results in fasta format can be found at RCArun.fasta_20211216-141253_files/results/no_ref/fasta/consensus_no_ref_s_2021-12-16_RCArun.fasta.
The subread file can be found at RCArun.fasta_20211216-141253_files/results/subreads_0_40_RCArun.fasta.csv.


