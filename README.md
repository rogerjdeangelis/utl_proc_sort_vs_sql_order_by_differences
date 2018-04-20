# utl_proc_sort_vs_sql_order_by_differences
Proc sort vs sql order by differences. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Proc sort vs sql order by disfferences

    github
    https://github.com/rogerjdeangelis/utl_proc_sort_vs_sql_order_by_differences

    Same results WPS and SAS

    see
    https://listserv.uga.edu/cgi-bin/wa?A2=SAS-L;ac131b2f.1804c

    Differences

        1. SQL You can sort by a function, even if the variable is not
               the selecet clause.

                 order by put(age,age_deciles.)

        2. SORT  You can change attributes
                 by name;
                 format age z3.;
                 informat _all_;

    INPUT
    =====

       sahelp.class

       proc format;
           value $sex2des
           'M' = "Male"
           "F" = "Female"
       ;run;quit;


    PROCESS with OUTPUT
    ===================

    * SQL has some advantages over sort and can eliminate a view or datastep.;
    * You can order by functions and formats.;

    proc sql;
      select
            *
      from
            sashelp.class
      order
            by put(sex,$sex2des.)
    ;quit;

    /*
    NAME      SEX       AGE    HEIGHT    WEIGHT
    -------------------------------------------
    Judy      F          14      64.3        90
    Jane      F          12      59.8      84.5
    Joyce     F          11      51.3      50.5
    Barbara   F          13      65.3        98
    */

    * Setting  labels and formats with proc sort, transpose and means

    Before Sort

     Variables in Creation Order

    #    Variable    Type    Len

    1    NAME        Char      8
    2    SEX         Char      1
    3    AGE         Num       8
    4    HEIGHT      Num       8
    5    WEIGHT      Num       8


    proc sort data=sashelp.class  out=class;
      label name="New label";
      by name;
      format age z3.;
      informat _all_;
    run;

    After Sort

     Variable    Type    Len    Format    Label

     AGE         Num       8    Z3.
     HEIGHT      Num       8
     NAME        Char      8              New label
     SEX         Char      1

    proc transpose data=sashelp.class out=clsxpo;
      label name="Transpose label";
      format name $char19.;
      informat _all_;
      by name;
      var sex;
    run;

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

       sahelp.class

       proc format;
           value $sex2des
           'M' = "Male"
           "F" = "Female"
       ;run;quit;


    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    * WPS:
    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    libname hlp sas7bdat "C:\Progra~1\SASHome\SASFoundation\9.4\core\sashelp";
    proc format;
        value $sex2des
        "M" = "Male"
        "F" = "Female"
    ;run;quit;
    proc sql;
      select
            *
      from
            hlp.class
      order
            by put(sex,$sex2des.)
    ;quit;

    proc sort data=hlp.class  out=class;
      label name="New label";
      by name;
      format age z3.;
      informat _all_;
    run;

    proc contents data=class;
    run;quit;

    proc transpose data=hlp.class out=clsxpo;
      label name="Transpose label";
      format name $char19.;
      informat _all_;
      by name;
      var sex;
    run;
    ');

    *SAS

     see above


