# utl-Python-import-standalone-sas-format-catalogs-and-export-to-R-dataframes
Python import standalone sas format catalogs and export to R dataframes

    Python import standalone sas format catalogs and export to R dataframes

    Win 7 64bit SAS 9.4 64bit

    I have not tried 32bit SAS format catalogs.

    Python reading  stanalone sas catalogs without sas and exporting to R dataframes

    * R does not support SAS format catalogs?;

    gitthub
    https://tinyurl.com/y6sacr7n
    https://github.com/rogerjdeangelis/utl-Python-import-standalone-sas-format-catalogs-and-export-to-R-dataframes

    Python packages;
    https://github.com/Roche/pyreadstat
    https://github.com/ofajardo/pyreadr#what-objects-can-be-read

    also see
    https://tinyurl.com/y4czlt2r
    https://github.com/rogerjdeangelis/utl-python-import-sas-catalogs-and-export-to-R-dataframe

    pyreadstat
    https://ofajardo.github.io/pyreadstat_documentation/_build/html/index.html

    macros
    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories


    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    ********************
    SAS FORMAT CATALOG *
    ********************;

    *  d:/sd1/pyfmt.sas7bcat;

    options valivarname=upcase;

    libname sd1 "d:/sd1";

    options fmtsearch=(sd1.pyfmt);

    proc format lib=sd1.pyfmt;
      value $sex
       "M"="Male"
       "F"="Female"
    ;quit;

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    ********
    PYTHON *
    ********

    {'$SEX': {'M': 'Male', 'F': 'Female'}}

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2 entries, 0 to 1
    Data columns (total 2 columns):
    START    2 non-null object
    $SEX     2 non-null object
    dtypes: object(2)
    memory usage: 112.0+ bytes

    ***
    R *
    ***
      START    $SEX
    0     F  Female
    1     M    Male


    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    * if utlfkil this does not delete the file manually delete the file;
    & write_rds sets 'read only?';

    %utlfkil(d:/rds/pycat.Rds);

    %utl_submit_py64_37('
    import pyreadr;
    import pandas as pd;
    import pyreadstat;
    df, meta = pyreadstat.read_sas7bcat("d:/sd1/pyfmt.sas7bcat");
    print(meta.value_labels);
    want=pd.DataFrame.from_dict(meta.value_labels);
    want.index.name = "START";
    want.reset_index(inplace=True);
    want.info();
    print(want);
    pyreadr.write_rds("d:/rds/pycat.Rds", want);
    ');

    *****
    LOG *
    *****

    {'$SEX': {'M': 'Male', 'F': 'Female'}}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2 entries, 0 to 1
    Data columns (total 2 columns):
    START    2 non-null object
    $SEX     2 non-null object
    dtypes: object(2)
    memory usage: 112.0+ bytes
      START    $SEX
    0     F  Female
    1     M    Male


    %utl_submit_r64('
    library(haven);
    pytable <- readRDS("d:/rds/pycat.Rds");
    pytable;
    ');

    *****
    LOG *
    *****

    > library(haven);
    > pytable <- readRDS("d:/rds/pycat.Rds");
    > pytable;

      START   $SEX
    1     F Female
    2     M   Male
    >


