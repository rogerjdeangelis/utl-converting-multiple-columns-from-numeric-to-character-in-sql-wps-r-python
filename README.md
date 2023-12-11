# utl-converting-multiple-columns-from-numeric-to-character-in-sql-wps-r-python
Converting multiple columns from numeric to character in wps r python
    %let pgm=utl-converting-multiple-columns-from-numeric-to-character-in-sql-wps-r-python;

    Converting multiple columns from numeric to character in wps r python;

    SOLUTIONS

        1 wps sql
        2 wps r sql
        3 wps python sql
        4 wps r non sql


    github
    https://tinyurl.com/bdf5jvds
    https://github.com/rogerjdeangelis/utl-converting-multiple-columns-from-numeric-to-character-in-sql-wps-r-python

    stackoverflow r
    https://tinyurl.com/5uh5z8uj
    https://stackoverflow.com/questions/77628631/converting-multiple-columns-from-numeric-to-character-in-r

    /**************************************************************************************************************************/
    /*                    |                                                     |                                             */
    /*                    |                                                     |                                             */
    /*       INPUT        |      SQL PROCESS (SELF DOCUMENTING)                 |            OUTPUT                           */
    /*                    |                                                     |                                             */
    /*  ACT1_A    ACT1_B  |                                                     |     ACT1_A     ACT1_B                       */
    /*                    |                                                     |                                             */
    /*     5         6    |     when act1_b between 1.0 and 4.0 then "leisure"  |     work       work                         */
    /*     2         3    |     when act1_b between 5.0 and 7.0 then "work"     |     leisure    leisure                      */
    /*     1         7    |     else "home"                                     |     leisure    work                         */
    /*     3         2    |     end as act1_a                                   |     leisure    leisure                      */
    /*     8         5    |                                                     |     home       work                         */
    /*     0         1    |     when act1_b between 1.0 and 4.0 then "leisure"  |     home       leisure                      */
    /*     4         8    |     when act1_b between 5.0 and 7.0 then "work"     |     leisure    home                         */
    /*     6         9    |     else "home"                                     |     work       home                         */
    /*     7         4    |     end as act1_a                                   |     work       leisure                      */
    /*     9         0    |                                                     |     home       home                         */
    /*                    |                                                     |                                             */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     input act1_a act1_b ;
    cards4;
    5 6
    2 3
    1 7
    3 2
    8 5
    0 1
    4 8
    6 9
    7 4
    9 0
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.HAVE total obs=10 10MAY2022:20:30:18                                                                               */
    /*                                                                                                                        */
    /*  Obs    ACT1_A    ACT1_B                                                                                               */
    /*                                                                                                                        */
    /*    1       5         6                                                                                                 */
    /*    2       2         3                                                                                                 */
    /*    3       1         7                                                                                                 */
    /*    4       3         2                                                                                                 */
    /*    5       8         5                                                                                                 */
    /*    6       0         1                                                                                                 */
    /*    7       4         8                                                                                                 */
    /*    8       6         9                                                                                                 */
    /*    9       7         4                                                                                                 */
    /*   10       9         0                                                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*                                  _
    / | __      ___ __  ___   ___  __ _| |
    | | \ \ /\ / / `_ \/ __| / __|/ _` | |
    | |  \ V  V /| |_) \__ \ \__ \ (_| | |
    |_|   \_/\_/ | .__/|___/ |___/\__, |_|
                 |_|                 |_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    %utl_submit_wps64x('
    proc sql;
      options validvarname=any;
      libname sd1 "d:/sd1";
      create
        table sd1.want as
      select
         case
           when act1_b between 1.0 and 4.0 then "leisure"
           when act1_b between 5.0 and 7.0 then "work"
           else "home"
         end as act1_a
        ,case
           when act1_b between 1.0 and 4.0 then "leisure"
           when act1_b between 5.0 and 7.0 then "work"
           else "home"
         end as act1_b
      from
         sd1.have
    ;quit;
    proc print data=sd1.want;
    run;quit;
    ');

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* SD1.WANT total obs=10                                                                                                  */
    /*                                                                                                                        */
    /*  Obs    ACT1_A     ACT1_B                                                                                              */
    /*                                                                                                                        */
    /*    1    work       work                                                                                                */
    /*    2    leisure    leisure                                                                                             */
    /*    3    leisure    work                                                                                                */
    /*    4    leisure    leisure                                                                                             */
    /*    5    home       work                                                                                                */
    /*    6    home       leisure                                                                                             */
    /*    7    leisure    home                                                                                                */
    /*    8    work       home                                                                                                */
    /*    9    work       leisure                                                                                             */
    /*   10    home       home                                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*___                                          _
    |___ \  __      ___ __  ___   _ __   ___  __ _| |
      __) | \ \ /\ / / `_ \/ __| | `__| / __|/ _` | |
     / __/   \ V  V /| |_) \__ \ | |    \__ \ (_| | |
    |_____|   \_/\_/ | .__/|___/ |_|    |___/\__, |_|
                     |_|                        |_|
    */
    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    options validvarname=any;
    libname sd1 "d:/sd1";

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    /*----                                                                   ----*/
    /*----  note the posted R solution uses 1 because but we use 1.0         ----*/
    /*----  This is because the posed R solution uses int type. we use float ----*/
    /*----                                                                   ----*/
    proc r;
    export data=sd1.have r=have;
    submit;
    library(sqldf);
    want<-sqldf("
      select
         case
           when  act1_a between 1.0 and 4.0 then  \"leisure\"
           when  act1_a between 5.0 and 7.0 then  \"work\"
           else \"home\"
         end act1_a
        ,case
           when  act1_b between 1.0 and 4.0 then  \"leisure\"
           when  act1_b between 5.0 and 7.0 then  \"work\"
           else \"home\"
         end act1_b
      from
         have
    ");
    want;
    endsubmit;
    import data=sd1.want r=want;
    proc print data=sd1.want;
    run;quit;
    ');

    proc print data=sd1.want;
    run;quit;


    /*____                                    _   _                             _
    |___ /  __      ___ __  ___   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  \ \ /\ / / `_ \/ __| | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) |  \ V  V /| |_) \__ \ | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/    \_/\_/ | .__/|___/ | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
                     |_|         |_|    |___/                                |_|
    */

    %utlfkil(d:/xpt/want.xpt);

    proc datasets lib=work nolist mt=view mt=data nodetails;delete want; run;quit;
    proc datasets lib=sd1 nolist mt=view mt=data nodetails;delete want; run;quit;

    %utl_submit_wps64x("
    options validvarname=any lrecl=32756;
    libname sd1 'd:/sd1';
    proc python;
    export data=sd1.have python=have;
    submit;
    import pyreadstat;
    from os import path;
    import pandas as pd;
    import numpy as np;
    from pandasql import sqldf;
    mysql = lambda q: sqldf(q, globals());
    from pandasql import PandaSQL;
    pdsql = PandaSQL(persist=True);
    want = pdsql('''
      select
         case
           when  act1_a between 1.0 and 4.0 then  `leisure`
           when  act1_a between 5.0 and 7.0 then  `work`
           else `home`
         end act1_a
        ,case
           when  act1_b between 1.0 and 4.0 then  `leisure`
           when  act1_b between 5.0 and 7.0 then  `work`
           else `home`
         end act1_b
      from
         have
    ''');
    print(want);
    pyreadstat.write_xport(want, 'd:/xpt/want.xpt',table_name='want',file_format_version=5
    ,column_labels=['ACT1_A' ,'ACT_B']);
    endsubmit;
    run;quit;
    ");

    libname xpt xport "d:/xpt/want.xpt";

    proc contents data=xpt._all_;
    run;quit;

    data want_py_long_names;
      %utl_rens(xpt.want) ;
      set want;
    run;quit;

    proc print;
    run;quit;

    libname xpt clear;


    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  The WPS Python System                                                                                                 */
    /*                                                                                                                        */
    /*      act1_a   act1_b                                                                                                   */
    /*  0     work     work                                                                                                   */
    /*  1  leisure  leisure                                                                                                   */
    /*  2  leisure     work                                                                                                   */
    /*  3  leisure  leisure                                                                                                   */
    /*  4     home     work                                                                                                   */
    /*  5     home  leisure                                                                                                   */
    /*  6  leisure     home                                                                                                   */
    /*  7     work     home                                                                                                   */
    /*  8     work  leisure                                                                                                   */
    /*  9     home     home                                                                                                   */
    /*                                                                                                                        */
    /*  The WPS System                                                                                                        */
    /*                                                                                                                        */
    /*  WANT_PY_LONG_NAMES total obs=10 10MAY2022:20:40:08                                                                    */
    /*                                                                                                                        */
    /*  Obs    ACT1_A     ACT_B                                                                                               */
    /*                                                                                                                        */
    /*    1    work       work                                                                                                */
    /*    2    leisure    leisure                                                                                             */
    /*    3    leisure    work                                                                                                */
    /*    4    leisure    leisure                                                                                             */
    /*    5    home       work                                                                                                */
    /*    6    home       leisure                                                                                             */
    /*    7    leisure    home                                                                                                */
    /*    8    work       home                                                                                                */
    /*    9    work       leisure                                                                                             */
    /*   10    home       home                                                                                                */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*  _                                          _   _
    | || |     __ _ _ __  ___   _ __   _ __   __ _| |_(_)_   _____
    | || |_   / _` | `_ \/ __| | `__| | `_ \ / _` | __| \ \ / / _ \
    |__   _| | (_| | |_) \__ \ | |    | | | | (_| | |_| |\ V /  __/
       |_|    \__, | .__/|___/ |_|    |_| |_|\__,_|\__|_| \_/ \___|
                 |_|_|
    */

    proc datasets lib=sd1 nolist nodetails;delete want; run;quit;

    options validvarname=any;
    libname sd1 "d:/sd1";

    %utl_submit_wps64x('
    libname sd1 "d:/sd1";
    /*----                                                                   ----*/
    /*----  note the posted R solution uses 1 because but we use 1.0         ----*/
    /*----  This is because the posed R solution uses int type. we use float ----*/
    /*----                                                                   ----*/
    proc r;
    export data=sd1.have r=have;
    submit;
    library(tidyverse);
    want<-have |>
      mutate(across(
        starts_with("act1"),
        \(x) case_when(
          x %in% 1:4 ~ "leisure",
          x %in% 5:7 ~ "work",
          x %in% c(8, 9, 0) ~ "home"
        )
      ));
    want;
    endsubmit;
    import data=sd1.want r=want;
    proc print data=sd1.want;
    run;quit;
    ');

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
