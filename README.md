PyMINEr can be used in one of three ways

-------------------------------------------------
Method1:
    PyMINEr will run the MINE analysis and feed the output directly in
    for network analysis
    
or

Method2:
    PyMINEr can be used with pre-generated MINE outputs
    * this assumes MINE outputs have already been filtered *
    * to an appropriate MIC cutoff                         *

or

Method3:
    do not perform network analysis, only return the mine analysis
    for a one group input.
-------------------------------------------------
Usage for Method1:

python3 PyMINEr_[version].py -infile <raw_data_file.txt>
                             -dividing_col <int>
                             -MineDir <path> [options]
Options:
    -infile <data file>         This file contains all raw data formatted in the format below:
                                IDs     Group1_rep1     Group1_rep2 ... Group2_rep1     Group2_rep2 ...
                                Var1    12.3            13.2        ... 7.8             5.9         ...
                                Var2    6.8             7.1         ... 5.3             6.7         ...
                                ...
    -t < int >=2 >              Number of the number of available threads to be used for MINE analysis
                                *default is 2*
    -mem <int>                  Available memory in gigs per thread being used *default is 24*
    -MineDir <path>             Path to MINE.jar
    -MIC_cutoff <decimal float> All relationships below this cutoff will be thrown out
                                and cannot be recovered
                                *note "-MIC_cutoff1" and "-MIC_cutoff2" can also be
                                used instead to specify different
                                *cutoffs for the two groups
                                *this will be especially needed when the two groups have
                                different n, and different False Postive Rate (FPR)
    -microarray                 This option will make PyMINEr treat the input as
                                post-normalization microarray data, and will search
                                for "!series_matrix_table_begin" for the beginning of
                                the table
    -disease <str>              This variable specifies what the name of the disease
                                or treatment is for the second group
                                (past the -dividing_col) *default is "disease"*
    -control <str>              An alternative name for the control *default is "control"*
    -MINE_only                  This will prevent performing the network comparisions
                                and only return final variable-variable interaction MINE
                                output for each group
    -clean                      Cleans up temporary files created by running mine
                                (there are quite a few, but good to have when troubleshooting)
                                *off by default*
    -log_fold                   This changes the values for the fold changes to the
                                natural log of the ratio between group1 controls and
                                group2 diseased/treated: Ln(mean(diseased)/mean(treated))
                                *only applicable if graphs are being made*
    -no_graphs                  This option specifies that no graphs should be created,
                                only summary files
    -out_relationship_mat_only  Only returns a 2D matrix of interactions for both
                                control and diseased/treated
    -rainbow                    This option specifies that for graphs creaed including
                                expression as a variable the points should be rainbow
                                color-coded with blue as decreased expression, and
                                red as increased expression in the second group relative
                                to the first
                                *on by default*
    -no_rainbow                 Instead of the rainbow option, all points will be a
                                semi-transparent black
    -dpi                        Dpi for figures.  Default is 360
    -log_expression             Changes axis title of figures to include Log2;
                                this should be used if input data is log2 transformed
    -mannwhit                   Will additionally perform mann whitney comparisions
                                for all variables between groups and return
                                the uncorrected p-value as an additional column in the
                                expression summary file
    -express_only               Performs only the expression analysis
    -no_express                 Minimizes the amount of expression analysis performed

    ### cluster computing options ###
    # Clusters which support load balancing thread passing (such as openSSI)
    # should not need these options.
    # Cluster computing requiring submission to queue via "qsub" are currenly
    # the only supported cluster computing format.
    #
    # Note that these options will create a new master_cluster_sub.sh file
    # which will need to be run by command line with "sh master_cluster_sub.sh"
    # This is required because many clusters do not allow additional submission
    # spawning from compute nodes
    
    -cluster                    This option tells PyMINEr that the MINE relationship
                                detection will be done using cluster computing
    -sub_prefix                 This is a path to a file that may contain the header
                                for qsub submissions.  This file may contain email options,
                                memory options, parallele environment options,
                                package imports, etc..
    -tpsub                      This abbreviation stands for "threads per submission"
                                which will indicate how many threads should be given to each
                                *This in conjunction will the -t option will let PyMINEr
                                *know how many submissions to make via qsub

-------------------------------------------------
Usage for Method2:

python3 PyMINEr_[version].py -infile <raw_data_file.txt>
                             -dividing_col <int>
                             -in_mine1 <mine_output_control>
                             -in_mine2 <mine_output_diseased-treated>
                             [options]

Options:
    -infile <data file>         This file contains all raw data formatted in the format below:
                                IDs     Group1_rep1     Group1_rep2 ... Group2_rep1     Group2_rep2 ...
                                Var1    12.3            13.2        ... 7.8             5.9         ...
                                Var2    6.8             7.1         ... 5.3             6.7         ...
                                ...
                                *Note that formatting may be different for microarray option*
    -dividing_col <int>         Column which separates control and treated data
                                (index starts at 0)
                                (Thus number should also == the number of replicates in group1)
    -in_mine1 <path>            Mine output file for control.csv
    -in_mine2 <path>            Mine output file for diseased or treated.csv
    -microarray                 this option will make PyMINEr treat the input as
                                post-normalization microarray data, and will search for
                                "!series_matrix_table_begin" for the beginning of
                                the table
    -disease <str>              This variable specifies what the name of the disease
                                or treatment is for the second group
                                (past the -dividing_col) *default is "disease"*
    -control <str>              An alternative name for the control *default is "control"*
    -log_fold                   This changes the values for the fold changes to the
                                natural log of the ratio between group1 controls and
                                group2 diseased/treated: Ln(mean(diseased)/mean(treated))
                                *only applicable if graphs are being made*
    -no_graphs                  This option specifies that no graphs should be created,
                                only summary files
    -out_relationship_mat_only  Only returns a 2D matrix of interactions for both
                                control and diseased/treated
    -rainbow                    This option specifies that for graphs creaed including
                                expression as a variable the points should be rainbow
                                color-coded with blue as decreased expression, and red
                                as increased expression in the second group relative
                                to the first group
                                *on by default*
    -no_rainbow                 Instead of the rainbow option, all points will be a
                                semi-transparent black
    -dpi                        Dpi for figures.  Default is 360
    -log_expression             Changes axis title of figures to include Log2 - should
                                be used if input data is log2 transformed
    -mannwhit                   Will additionally perform mann whitney comparisions
                                for all variables between groups and return the
                                uncorrected p-value as an additional column in the
                                expression summary file
    -express_only               Performs only the expression analysis
    -no_express                 Minimizes the amount of expression analysis performed

-------------------------------------------------
*note - if mine1 and mine2 files are provided, PyMINEr will not run a new MINE
analysis on the input data*
-------------------------------------------------

Usage for Method3:

python3 PyMINEr_[version].py -infile <raw_data_file.txt>
                             -MineDir <path>
                             -single_network
                             [options]

    -t < int >=2 >              Number of the number of available threads to be used
                                for MINE analysis
    -mem <int>                  Available memory in gigs per thread being used
                                *default is 24*
    -MineDir <path>             Path to MINE.jar (will try ~/MINE.jar by default)
    -MIC_cutoff <decimal float> All relationships below this cutoff will be thrown
                                out and cannot be recovered
    -microarray                 This option will make PyMINEr treat the input as
                                post-normalization microarray data, and will search
                                for "!series_matrix_table_begin" for the beginning of
                                the table
    -no_express                 Minimizes the amount of expression analysis performed
    -single_network             The -single_network option indicates that there is
                                only one group and the network comparision should not
                                be done, but the MINE analysis to discover relationships
                                should still be performed

    ### cluster computing options ###
    # Clusters which support load balancing thread passing (such as openSSI)
    # should not need these options.
    # Cluster computing requiring submission to queue via "qsub" are currenly
    # the only supported cluster computing format.
    #
    # Note that these options will create a new master_cluster_sub.sh file
    # which will need to be run by command line with "sh master_cluster_sub.sh"
    # This is required because many clusters do not allow additional submission
    # spawning from compute nodes
    
    -cluster                    This option tells PyMINEr that the MINE relationship
                                detection will be done using cluster computing
    -sub_prefix                 This is a path to a file that may contain the header
                                for qsub submissions.  This file may contain email options,
                                memory options, parallele environment options,
                                package imports, etc..
    -tpsub                      This abbreviation stands for "threads per submission"
                                which will indicate how many threads should be given to each
                                *This in conjunction will the -t option will let PyMINEr
                                *know how many submissions to make via qsub

-------------------------------------------------

major dependencies are:
MINE.jar    downloadable at exploredata.net
python3     downloadable at python.org
numpy       downloadable at scipy.org/scipylib/download.html
matplotlib  required only for graphing
            downloadable at matplotlib.org/downloads.html

*note - these must all be installed in the python3 library directory,*
*not another version of python's library directory*
