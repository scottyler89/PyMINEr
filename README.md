PyMINEr is freely available by the GPL V2 license
=======

PyMINEr can be used in one of three ways

-------------------------------------------------
Method1:
    it can be used to run the MINE analysis and feed the output directly into
    PyMINEr for network analysis
    
or

Method2:
    it can be used with pre-generated MINE output

or

Method3:
    do not perform network analysis, only return the mine analysis
    for a one group input.
    *This still take advantage of the parallelization utilized in*
    *the full analysis, and greatly reduces the memory used      *
    *relative to using the 'allPairs' option in MINE             *
-------------------------------------------------
Usage for Method1:

python3 PyMINEr_1.3.py -infile <raw_data_file.txt> -dividing_col <int> -MineDir <path> [options]
Options:
    -infile <data file>         This file contains all raw data formatted in the format below:
                                IDs     Group1_rep1     Group1_rep2 ... Group2_rep1     Group2_rep2 ...
                                Var1    12.3            13.2        ... 7.8             5.9         ...
                                Var2    6.8             7.1         ... 5.3             6.7         ...
                                ...
    -t < int >=2 >              number of the number of available threads to be used for MINE analysis
                                *default is 2*
    -mem <int>                  available memory in gigs per thread being used *default is 24*
    -MineDir <path>             path to MINE.jar
    -MIC_cutoff <decimal float> all relationships below this cutoff will be thrown out and cannot be recovered
    -microarray                 this option will make PyMINEr treat the input as post-normalization microarray
                                data, and will search for "!series_matrix_table_begin" for the beginning of
                                the table
    -disease <str>              this variable specifies what the name of the disease or treatment is for the
                                second group (past the -dividing_col) *default is "disease"*
    -clean                      cleans up temporary files created by running mine (there are quite a few, but good
                                to have when troubleshooting) *off by default*
    -control <str>              an alternative name for the control *default is "control"*
    -log_fold                   this changes the values for the fold changes to the natural log of the ratio
                                between group1 controls and group2 diseased/treated: Ln(mean(diseased)/mean(treated))
                                *only applicable if graphs are being made*
    -no_graphs                  this option specifies that no graphs should be created, only summary files
    -out_relationship_mat_only  only returns a 2D matrix of interactions for both control and diseased/treated
    -rainbow                    this option specifies that for graphs creaed including expression as a variable
                                the points should be rainbow color-coded with blue as decreased expression, and
                                red as increased expression in the second group relative to the first
                                *on by default*
    -no_rainbow                 instead of the rainbow option, all points will be a semi-transparent black
    -dpi                        dpi for figures.  Default is 360
    -log_expression             changes axis title of figures to include Log2 - should be used if input data is log2 transformed
    -mannwhit                   will additionally perform mann whitney comparisions
                                for all variables between groups and return the uncorrected
                                p-value as an additional column in the expression summary file
    -express_only               performs only the expression analysis
    -no_express                 minimizes the amount of expression analysis performed

-------------------------------------------------
Usage for Method2:
python3 PyMINEr_1.3.py -infile <raw_data_file.txt> -dividing_col <int> -in_mine1 <mine_output_control> -in_mine2 <mine_output_diseased-treated> [options]
Options:
    -infile <data file>         This file contains all raw data formatted in the format below:
                                IDs     Group1_rep1     Group1_rep2 ... Group2_rep1     Group2_rep2 ...
                                Var1    12.3            13.2        ... 7.8             5.9         ...
                                Var2    6.8             7.1         ... 5.3             6.7         ...
                                ...
    -dividing_col <int>         Column which separates control and treated data (index starts at 0)
                                (Thus number should also == the number of replicates in group1)
    -in_mine1 <path>            Mine output file for control.csv
    -in_mine2 <path>            Mine output file for diseased or treated.csv
    -microarray                 this option will make PyMINEr treat the input as post-normalization microarray
                                data, and will search for "!series_matrix_table_begin" for the beginning of
                                the table
    -disease <str>              this variable specifies what the name of the disease or treatment is for the
                                second group (past the -dividing_col) *default is "disease"*
    -control <str>              an alternative name for the control *default is "control"*
    -log_fold                   this changes the values for the fold changes to the natural log of the ratio
                                between group1 controls and group2 diseased/treated: Ln(mean(diseased)/mean(treated))
                                *only applicable if graphs are being made*
    -no_graphs                  this option specifies that no graphs should be created, only summary files
    -out_relationship_mat_only  only returns a 2D matrix of interactions for both control and diseased/treated
    -rainbow                    this option specifies that for graphs creaed including expression as a variable
                                the points should be rainbow color-coded with blue as decreased expression, and
                                red as increased expression in the second group relative to the first
                                *on by default*
    -no_rainbow                 instead of the rainbow option, all points will be a semi-transparent black
    -dpi                        dpi for figures.  Default is 360
    -log_expression             changes axis title of figures to include Log2 - should be used if input data is log2 transformed
    -mannwhit                   will additionally perform mann whitney comparisions
                                for all variables between groups and return the uncorrected
                                p-value as an additional column in the expression summary file
    -express_only               performs only the expression analysis
    -no_express                 minimizes the amount of expression analysis performed
-------------------------------------------------
*note - if mine1 and mine2 files are provided, PyMINEr will not run a new MINE analysis on the input data*
-------------------------------------------------
Usage for Method3:
python3 PyMINEr_1.3.py -infile <raw_data_file.txt> -MineDir <path> [options]

    -t < int >=2 >              number of the number of available threads to be used for MINE analysis
    -mem <int>                  available memory in gigs per thread being used *default is 24*
    -MineDir <path>             path to MINE.jar (will try ~/MINE.jar by default)
    -MIC_cutoff <decimal float> all relationships below this cutoff will be thrown out and cannot be recovered
    -microarray                 this option will make PyMINEr treat the input as post-normalization microarray
                                data, and will search for "!series_matrix_table_begin" for the beginning of
                                the table
    -no_express                 minimizes the amount of expression analysis performed
    -single_network             The -single_network option indicates that there is only one group
                                and the network comparision should not be done, but the MINE analysis to
                                discover relationships should still be performed
-------------------------------------------------

major dependencies are:
MINE.jar    downloadable at exploredata.net
python3     downloadable at python.org
numpy       downloadable at http://www.scipy.org/scipylib/download.html
matplotlib  required only for graphing
            downloadable at http://matplotlib.org/downloads.html

*note - these must all be installed in the python3 library directory,*
*not another version of python's library directory*


Use the -h or --help option for usage instructions
