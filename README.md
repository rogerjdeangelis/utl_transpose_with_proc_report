# utl_transpose_with_proc_report
proc report can do more complex transposes than proc transpose?

    ```  SAS Forum: making the same format in every dataset  ```
    ```    ```
    ```     Apply I template to mutiple SAS datasetsl  ```
    ```    ```
    ```     Great question!!  ```
    ```    ```
    ```     WORKING CODE  ```
    ```        SAS (WPS does not support DOSUBL - neither does SAS officially?)  ```
    ```    ```
    ```     1. DOSUBL COMPILE TIME  ```
    ```           * get template format;  ```
    ```           If _n_=0  ```
    ```             proc contents data=work.template;  ```
    ```             proc sql;  ```
    ```              select catx(" ",variable,format) into :fmt separated by " "  ```
    ```              from vars order by num;  ```
    ```    ```
    ```     2. MAINLINE (meta contains list of datasets)  ```
    ```    ```
    ```           set meta;  ```
    ```             call symputx('dsn',dsn);  ```
    ```    ```
    ```     3. DOSUBL  ```
    ```          proc datasets lib=work;  ```
    ```                modify &dsn;  ```
    ```              format &fmt;  ```
    ```    ```
    ```     4. MAINLINE ( if error a window pops up asking if you want to continue)  ```
    ```            *you could use Python tkinter for fancy interface;  ```
    ```    ```
    ```        if symget('rc_code') ne '0' then do;  ```
    ```    ```
    ```             putlog // "Failed to FORMAT " dsn " using dataset " template //;  ```
    ```    ```
    ```             window chose irow=5 rows=25  ```
    ```               #5 @12 "Error encountered - Continue or stop and format " response $8. attr=underline;  ```
    ```             display chose;  ```
    ```             if response='stop' then do;  ```
    ```                 put "Stopping datastep because of user request";  ```
    ```                 stop;  ```
    ```             end;  ```
    ```             putlog "Continuing datastep processing even though an error was encountered";  ```
    ```        end;  ```
    ```    end;  ```
    ```    ```
    ```    ```
    ```  see  ```
    ```  https://goo.gl/VxUB3g  ```
    ```  https://communities.sas.com/t5/Base-SAS-Programming/making-the-same-format-in-every-dataset/m-p/390562  ```
    ```    ```
    ```    ```
    ```    ```
    ```  HAVE  (template dataset and three datasets to reformat like the template)  ```
    ```  =========================================================================  ```
    ```    ```
    ```  Template dataset and datasets we want reformat  ```
    ```    ```
    ```  META  ```
    ```    ```
    ```  Up to 40 obs WORK.META total obs=3  ```
    ```    ```
    ```  Obs    TEMPLATE     DSN  ```
    ```    ```
    ```   1     template    hav1st  ```
    ```   2     template    hav2nd  ```
    ```   3     template    hav3rd  ```
    ```    ```
    ```    ```
    ```  DATASETS  ```
    ```    ```
    ```  DSNS      Variable    Type    Len    Format  ```
    ```    ```
    ```  TEMPLATE  NAME        Char      8    $CHAR8.  ```
    ```  TEMPLATE  AGE         Num       8    2.  ```
    ```    ```
    ```  HAV1ST    NAME        Char      8    $32.  ```
    ```  HAV1ST    AGE         Num       8    18.8  ```
    ```    ```
    ```  HAV2ND    NAME        Char      8    $8.  ```
    ```  HAV2ND    AGE         Num       8    8.2  ```
    ```    ```
    ```  HAV3RD    NAME        Char      8    $CHAR12.  ```
    ```  HAV3RD    AGE         Num       8    PERCENT.  ```
    ```    ```
    ```    ```
    ```    ```
    ```    ```
    ```  WANT (set all formats to look like the template)  ```
    ```  =================================================  ```
    ```    ```
    ```  DATASETS  ```
    ```    ```
    ```  DSNS      Variable    Type    Len    Format  ```
    ```    ```
    ```  TEMPLATE  NAME        Char      8    $CHAR8.  ```
    ```  TEMPLATE  AGE         Num       8    2.  ```
    ```    ```
    ```  HAV1ST    NAME        Char      8    $CHAR8.  ```
    ```  HAV1ST    AGE         Num       8    2.  ```
    ```    ```
    ```  HAV2ND    NAME        Char      8    $CHAR8.  ```
    ```  HAV2ND    AGE         Num       8    2.  ```
    ```    ```
    ```  HAV3RD    NAME        Char      8    $CHAR8.  ```
    ```  HAV3RD    AGE         Num       8    2.  ```
    ```    ```
    ```    ```
    ```  *                _              _       _  ```
    ```   _ __ ___   __ _| | _____    __| | __ _| |_ __ _  ```
    ```  | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |  ```
    ```  | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |  ```
    ```  |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  data meta;  ```
    ```    input template$ dsn$;  ```
    ```  cards4;  ```
    ```  template hav1st  ```
    ```  template hav2nd  ```
    ```  template hav3rd  ```
    ```  ;;;;  ```
    ```  run;quit;  ```
    ```    ```
    ```  data template;  ```
    ```    set sashelp.class;  ```
    ```    format name $char8. age 2.;  ```
    ```    keep name age;  ```
    ```  run;quit;  ```
    ```    ```
    ```  data hav1st;  ```
    ```    set sashelp.class;  ```
    ```    format name $32. age 18.8;  ```
    ```    keep name age;  ```
    ```  run;quit;  ```
    ```    ```
    ```  data hav2nd;  ```
    ```    set sashelp.class;  ```
    ```    format name $8. age 8.2;  ```
    ```    keep name age;  ```
    ```  run;quit;  ```
    ```    ```
    ```    ```
    ```  data hav3rd;  ```
    ```    set sashelp.class;  ```
    ```    format name $char12. age percent.;  ```
    ```    keep name age;  ```
    ```  run;quit;  ```
    ```    ```
    ```  *          _       _   _  ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __  ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \  ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |  ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  data _null_;  ```
    ```    ```
    ```     * get template formats;  ```
    ```     if _n_=0 then do;  ```
    ```       %let rc=%sysfunc(dosubl('  ```
    ```         proc contents data=work.template;  ```
    ```         run;quit;  ```
    ```         proc sql;  ```
    ```            select catx(" ",variable,format) into :fmt separated by " "  ```
    ```            from vars order by num;  ```
    ```         ;quit;  ```
    ```       );  ```
    ```      end;  ```
    ```    ```
    ```      * get list of datasets;  ```
    ```      set meta;  ```
    ```         call symputx('dsn',dsn);  ```
    ```      %let dsn=have2nd;  ```
    ```      rc=dosubl('  ```
    ```          proc datasets lib=work;  ```
    ```              modify &dsn;  ```
    ```              format &fmt;  ```
    ```          run;quit;  ```
    ```          %let rc_code=&syserr;  ```
    ```          %let rctxt=&syserrortxt;  ```
    ```        ');  ```
    ```    ```
    ```        if symget('rc_code') ne '0' then do;  ```
    ```    ```
    ```             syserr=symget('rc_code');     * probably do not need to do this;  ```
    ```             syserrortext=symget('rctxt'); * probably do not need to do this;  ```
    ```    ```
    ```             putlog // "Failed to Create Dataset &dsn " //;  ```
    ```    ```
    ```             putlog "Error Code =" syserr //;  ```
    ```             putlog "Error Text =" syserrortext //;  ```
    ```    ```
    ```             window chose irow=5 rows=25  ```
    ```               #5 @12 "Error encountered - Continue or stop and create " response $8. attr=underline;  ```
    ```             display chose;  ```
    ```             if response='stop' then do;  ```
    ```                 put "Stopping datastep because of user request";  ```
    ```                 stop;  ```
    ```             end;  ```
    ```             putlog "Continuing datastep processing even though an error was encountered";  ```
    ```        end;  ```
    ```    end;  ```
    ```    ```
    ```  run;quit;  ```
    ```    ```
    ```    ```
