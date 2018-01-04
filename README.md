# utl_elegant_solution_to_a_fairly_complex_filter_using_dow_loop
SAS/WPS Elegant solution to a fairly complex filter_using_DOW_loop.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS co
    SAS/WPS Elegant solution to a fairly complex filter_using_DOW_loop.

    "If the total no of installments for a particular arrangement
    is Less than or equal to 4 then the output should have all
    the installments.

    If it is greater than four, or a multiple of four, the the values
    should be the next subsequent set of four values."

    github
    https://goo.gl/HjFuEt
    https://github.com/rogerjdeangelis/utl_elegant_solution_to_a_fairly_complex_filter_using_dow_loop

    see
    https://goo.gl/UxYEhB
    https://stackoverflow.com/questions/48089223/retrieving-subset-of-a-group-of-data-in-sql-or-sas

    Richard profile
    https://stackoverflow.com/users/1249962/richard


    INPUT
    =====

     WORK.HAVE total obs=25        |           RULES
                                   |
      ID    SEQUENCE    DUE_DATE   |    ID    SEQUENCE    DUE_DATE
                                   |
       1        1        1-Dec     |     1        1        1-Dec   * put all out because
       1        2        8-Dec     |     1        2        8-Dec   * sequence is <= 4
       1        3        15-Dec    |     1        3        15-Dec
       1        4        22-Dec    |     1        4        22-Dec
                                   |
       2        1        1-Dec     |     2        1        1-Dec   * put all out because
       2        2        8-Dec     |     2        2        8-Dec   * sequence is <= 4

       3        1        5-Dec     |     3        1        5-Dec
       3        2        12-Dec    |     3        2        12-Dec
       3        3        19-Dec    |     3        3        19-Dec

       4        1        6-Nov     |     4        5        4-Dec   * if sequence > 4 or
       4        2        13-Nov    |     4        6        11-Dec  * multiple of 4 output last set
       4        3        20-Nov    |
       4        4        27-Nov    |
       4        5        4-Dec     |
       4        6        11-Dec    |

       5        1        1-Jan     |     5        9        23-Feb  * last set after mutiple of
       5        2        7-Jan     |     5       10        24-Feb  * four
       5        3        13-Jan    |
       5        4        20-Jan    |
       5        5        27-Jan    |
       5        6        3-Feb     |
       5        7        10-Feb    |
       5        8        17-Feb    |
       5        9        23-Feb    |
       5       10        24-Feb    |


    PROCESS
    =======

     data want(label="Rows from each ids last 4-row chunk");
      do _n_ = 0 by 1 until (last.id);
        set have;
        by id sequence;
      end;

      _out_from_n = floor ( _n_ / 4 ) * 4;

      do _n_ = 0 to _n_;
        set have;
        if _n_ >= _out_from_n then OUTPUT;
      end;

      drop _:;
     run;

    OUTPUT
    ======

     WORK.WANT total obs=13

      ID    SEQUENCE    DUE_DATE

       1        1        1-Dec
       1        2        8-Dec
       1        3        15-Dec
       1        4        22-Dec
       2        1        1-Dec
       2        2        8-Dec
       3        1        5-Dec
       3        2        12-Dec
       3        3        19-Dec
       4        5        4-Dec
       4        6        11-Dec
       5        9        23-Feb
       5       10        24-Feb

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname wrk sas7bdat "%sysfunc(pathname(work))";

    data wrk.want(label="Rows from each ids last 4-row chunk");
      do _n_ = 0 by 1 until (last.id);
        set wrk.have;
        by id sequence;
      end;

      _out_from_n = floor ( _n_ / 4 ) * 4;

      do _n_ = 0 to _n_;
        set wrk.have;
        if _n_ >= _out_from_n then OUTPUT;
      end;

      drop _:;
    run;quit;
    ');
