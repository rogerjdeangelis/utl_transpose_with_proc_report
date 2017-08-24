# utl_transpose_with_proc_report
proc report can do more complex transposes than proc transpose?

    ```  T1007380 SAS-L Reshaping complex data using proc report  ```
    ```    ```
    ```      WORKING CODE (all of it)  ```
    ```    ```
    ```         SAS Only  ```
    ```    ```
    ```         Failed with WPS with this error  ```
    ```         ERROR: Cannot use OUT= option if there is a BY clause  ```
    ```    ```
    ```           proc report data=havsrt  nowd missing  ```
    ```                  out=havrpt(  ```
    ```                      rename=(  ```
    ```                         _C1_= Estimate  ```
    ```                         _C2_=StdErr  ```
    ```                         _C3_= Probt  ```
    ```                         _C4_=Race_Estimate  ```
    ```                         _C5_=Race_StdErr  ```
    ```                         _C6_=Race_Probt  ```
    ```                         _C7_=COL1_Race_Estimate  ```
    ```                         _C8_=COL1_Race_StdErr  ```
    ```                         _C9_=COL1_Race_Probt));  ```
    ```           by _name_;  ```
    ```           cols effect, (estimate stderr probt);  ```
    ```           define effect   / across;  ```
    ```           define estimate / sum ;  ```
    ```           define stderr   / sum ;  ```
    ```           define probt    / sum ;  ```
    ```    ```
    ```    ```
    ```  Good question, nice presentation;  ```
    ```    ```
    ```  see  ```
    ```  https://mail.google.com/mail/u/0/#inbox/15df6067fa4c4cfc  ```
    ```    ```
    ```    ```
    ```  HAVE  ```
    ```  ====  ```
    ```       Up to 40 obs WORK.HAVE total obs=6  ```
    ```    ```
    ```       Obs    _NAME_    EFFECT       ESTIMATE     STDERR      PROBT  ```
    ```    ```
    ```        1      X_1      COL1          0.00029    0.001542    0.84858  ```
    ```        2      X_1      Race         -0.12320    0.041150    0.00281  ```
    ```        3      X_1      COL1*Race    -0.00072    0.002932    0.80644  ```
    ```        4      X_10     COL1          0.03974    0.015330    0.00963  ```
    ```        5      X_10     Race         -0.15440    0.053240    0.00380  ```
    ```        6      X_10     COL1*Race     0.00913    0.025580    0.72127  ```
    ```    ```
    ```    ```
    ```    ```
    ```  WANT  (proc report output dataset)  ```
    ```  ====  ```
    ```  Up to 40 obs from havrpt total obs=2  ```
    ```                                                                                           COL1_      COL1_      COL1_  ```
    ```                                                        RACE_       RACE_      RACE_       RACE_      RACE_      RACE_  ```
    ```  Obs    _NAME_    ESTIMATE     STDERR      PROBT     ESTIMATE     STDERR      PROBT     ESTIMATE     STDERR     PROBT     _BREAK_  ```
    ```    ```
    ```   1      X_1      0.000294    0.001542    0.84858     -.00072    0.002932    0.80644     -0.1232    0.04115    .002812  ```
    ```   2      X_10     0.039740    0.015330    0.00963     0.00913    0.025580    0.72127     -0.1544    0.05324    .003801  ```
    ```    ```
    ```  *                _              _       _  ```
    ```   _ __ ___   __ _| | _____    __| | __ _| |_ __ _  ```
    ```  | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |  ```
    ```  | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |  ```
    ```  |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|  ```
    ```    ```
    ```  ;  ```
    ```    ```
    ```  data have;  ```
    ```  retain _name_;  ```
    ```  informat effect $10.;  ```
    ```  input  ```
    ```  _NAME_$ Effect$ Estimate StdErr Probt;  ```
    ```  cards4;  ```
    ```  X_1 COL1 0.000294 0.001542 0.84858  ```
    ```  X_1 Race -0.1232 0.04115 0.002812  ```
    ```  X_1 COL1*Race -0.00072 0.002932 0.80644  ```
    ```  X_10 COL1 0.03974 0.01533 0.009625  ```
    ```  X_10 Race -0.1544 0.05324 0.003801  ```
    ```  X_10 COL1*Race 0.00913 0.02558 0.721273  ```
    ```  ;;;;  ```
    ```  run;quit;  ```
    ```    ```
    ```  proc sort data=have out=havsrt;  ```
    ```  by _name_;  ```
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
    ```  %utl_submit_wps64('  ```
    ```  libname wrk sas7bdat "%sysfunc(pathname(work))";  ```
    ```  proc report data=wrk.havsrt  nowd missing  ```
    ```         out=havrpt(  ```
    ```             rename=(  ```
    ```                _C1_= Estimate  ```
    ```                _C2_=StdErr  ```
    ```                _C3_= Probt  ```
    ```                _C4_=Race_Estimate  ```
    ```                _C5_=Race_StdErr  ```
    ```                _C6_=Race_Probt  ```
    ```                _C7_=COL1_Race_Estimate  ```
    ```                _C8_=COL1_Race_StdErr  ```
    ```                _C9_=COL1_Race_Probt));  ```
    ```  by _name_;  ```
    ```  cols effect, (estimate stderr probt);  ```
    ```  define effect   / across;  ```
    ```  define estimate / sum ;  ```
    ```  define stderr   / sum ;  ```
    ```  define probt    / sum ;  ```
    ```  run;quit;  ```
    ```  ');  ```
    ```    ```
