# utl-get-the-top-and-bottom-n-rows-by-group-usng-a-hash
Get the top and bottom n rows by group usng a hash
    %let pgm=utl-get-the-top-and-bottom-n-rows-by-group-usng-a-hash;

    %stop_submission;

    Get the top and bottom n rows by group usng a hash;

    Hash by

      Paul Dorfman
      sashole@bellsouth.net

            1 paul hash
            2 related repos

    github
    https://tinyurl.com/mp77v3ty
    https://github.com/rogerjdeangelis/utl-get-the-top-and-bottom-n-rows-by-group-usng-a-hash

    sas communities
    https://tinyurl.com/5d4dhc38
    https://communities.sas.com/t5/New-SAS-User/Select-top-N-from-each-group-in-proc-sql/m-p/852237#M37440


    /**************************************************************************************************************************/
    /*         INPUT           |             PROCESS                           |                 OUTPUT                       */
    /*         =====           |             =======                           |                 ======                       */
    /*                         |                                               |                                              */
    /*  WORK.HAVE total obs=19 |                                               | GVKEY  FY    TA_1    TA_2    TA_3            */
    /*                         |                                               |                                              */
    /*   GVKEY   FY     TA     | GVKEY    FY     TA_1     TA_2     TA_3        |  1311 1995 3311.66 1599.23  656.23           */
    /*                         |                                               |  1700 1996 4182.83 2068.55 1727.07           */
    /*    1311  1995 3311.66*    1311    1995  3311.66  1599.23   656.23       | 84578 1995 3700.93 1312.85 1131.18           */
    /*    1311  1995 1599.23*                                                  |                                              */
    /*    1311  1995  564.76*                                                  |                                              */
    /*    1311  1995  560.05   |                                               |                                              */
    /*    1311  1995  656.23   |                                               |                                              */
    /*                         |                                               |                                              */
    /*    1700  1996  577.84*    1700    1996  4182.83  2068.55  1727.07       |                                              */
    /*    1700  1996 1696.43*                                                  |                                              */
    /*    1700  1996 1081.96*                                                  |                                              */
    /*    1700  1996 1727.07   |                                               |                                              */
    /*    1700  1996 4182.83   |                                               |                                              */
    /*    1700  1996 2068.55   |                                               |                                              */
    /*    1700  1996  841.20   |                                               |                                              */
    /*    1700  1996     .     |                                               |                                              */
    /*                         |                                               |                                              */
    /*   84578  1995 1312.85*    84578   1995  3700.93  1312.85  1131.18       |                                              */
    /*   84578  1995 1131.18*                                                  |                                              */
    /*   84578  1995 3700.93*                                                  |                                              */
    /*   84578  1995 1099.79   |                                               |                                              */
    /*   84578  1995  848.66   |                                               |                                              */
    /*   84578  1995 1067.57   |                                               |                                              */
    /*                         |                                               |                                              */
    /*   data have;            |   data want (drop = ta) ;                     |                                              */
    /*  input gvkey fy TA ;    |     if _n_ = 1 then do ;                      |                                              */
    /*  cards4;                |       dcl hash h (ordered:"D") ;              |                                              */
    /*  1311 1995 3311.662     |       h.definekey ("TA") ;                    |                                              */
    /*  1311 1995 1599.234     |       h.definedone () ;                       |                                              */
    /*  1311 1995 564.76       |       dcl hiter hi ("h") ;                    |                                              */
    /*  1311 1995 560.047      |     end ;                                     |                                              */
    /*  1311 1995 656.226      |     do until (last.fy) ;                      |                                              */
    /*  1700 1996 577.841      |       set have ;                              |                                              */
    /*  1700 1996 1696.431     |       by gvkey fy ;                           |                                              */
    /*  1700 1996 1081.963     |       h.ref() ;                               |                                              */
    /*  1700 1996 1727.069     |     end ;                                     |                                              */
    /*  1700 1996 4182.832     |     array TA_ [3] ;                           |                                              */
    /*  1700 1996 2068.554     |     do _n_ = 1 to dim (ta_)                   |                                              */
    /*  1700 1996 841.204      |       while (hi.next() = 0) ;                 |                                              */
    /*  1700 1996 .            |       ta_[_n_] = ta ;                         |                                              */
    /*  84578 1995 1312.852    |     end ;                                     |                                              */
    /*  84578 1995 1131.176    |     _n_ = hi.first() ;                        |                                              */
    /*  84578 1995 3700.925    |     _n_ = hi.prev() ;                         |                                              */
    /*  84578 1995 1099.786    |     h.clear() ;                               |                                              */
    /*  84578 1995 848.657     |   run ; quit ;                                |                                              */
    /*  84578 1995 1067.566    |                                               |                                              */
    /*  ;;;;                   |                                               |                                              */
    /*  run;quit;              |                                               |                                              */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

     data have;
    input gvkey fy TA ;
    cards4;
    1311 1995 3311.662
    1311 1995 1599.234
    1311 1995 564.76
    1311 1995 560.047
    1311 1995 656.226
    1700 1996 577.841
    1700 1996 1696.431
    1700 1996 1081.963
    1700 1996 1727.069
    1700 1996 4182.832
    1700 1996 2068.554
    1700 1996 841.204
    1700 1996 .
    84578 1995 1312.852
    84578 1995 1131.176
    84578 1995 3700.925
    84578 1995 1099.786
    84578 1995 848.657
    84578 1995 1067.566
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*  GVKEY     FY        TA                                                                                                */
    /*                                                                                                                        */
    /*   1311    1995    3311.66                                                                                              */
    /*   1311    1995    1599.23                                                                                              */
    /*   1311    1995     564.76                                                                                              */
    /*   1311    1995     560.05                                                                                              */
    /*   1311    1995     656.23                                                                                              */
    /*   1700    1996     577.84                                                                                              */
    /*   1700    1996    1696.43                                                                                              */
    /*   1700    1996    1081.96                                                                                              */
    /*   1700    1996    1727.07                                                                                              */
    /*   1700    1996    4182.83                                                                                              */
    /*   1700    1996    2068.55                                                                                              */
    /*   1700    1996     841.20                                                                                              */
    /*   1700    1996        .                                                                                                */
    /*  84578    1995    1312.85                                                                                              */
    /*  84578    1995    1131.18                                                                                              */
    /*  84578    1995    3700.93                                                                                              */
    /*  84578    1995    1099.79                                                                                              */
    /*  84578    1995     848.66                                                                                              */
    /*  84578    1995    1067.57                                                                                              */
    /**************************************************************************************************************************/

     /*                    _   _               _
    / |  _ __   __ _ _   _| | | |__   __ _ ___| |__
    | | | `_ \ / _` | | | | | | `_ \ / _` / __| `_ \
    | | | |_) | (_| | |_| | | | | | | (_| \__ \ | | |
    |_| | .__/ \__,_|\__,_|_| |_| |_|\__,_|___/_| |_|
        |_|
    */

    data want (drop = ta) ;
      if _n_ = 1 then do ;
        dcl hash h (ordered:"D") ;
        h.definekey ("TA") ;
        h.definedone () ;
        dcl hiter hi ("h") ;
      end ;
      do until (last.fy) ;
        set have ;
        by gvkey fy ;
        h.ref() ;
      end ;
      array TA_ [3] ;
      do _n_ = 1 to dim (ta_)
        while (hi.next() = 0) ;
        ta_[_n_] = ta ;
      end ;
      _n_ = hi.first() ;
      _n_ = hi.prev() ;
      h.clear() ;
    run ; quit ;


    /**************************************************************************************************************************/
    /*   GVKEY     FY       TA_1       TA_2       TA_3                                                                        */
    /*                                                                                                                        */
    /*    1311    1995    3311.66    1599.23     656.23                                                                       */
    /*    1700    1996    4182.83    2068.55    1727.07                                                                       */
    /*   84578    1995    3700.93    1312.85    1131.18                                                                       */
    /**************************************************************************************************************************/

    /*___
    |___ \   _ __ ___ _ __   ___  ___
      __) | | `__/ _ \ `_ \ / _ \/ __|
     / __/  | | |  __/ |_) | (_) \__ \
    |_____| |_|  \___| .__/ \___/|___/
                     |_|
    /*                _           _
    | |_ ___  _ __   | |__   ___ | |_
    | __/ _ \| `_ \  | `_ \ / _ \| __|
    | || (_) | |_) | | |_) | (_) | |_
     \__\___/| .__/  |_.__/ \___/ \__|
             |_|
    */

    https://github.com/rogerjdeangelis/utl-Why-does-the-vintage-Dell-I7-E6420-outperform-brand-new-Dell-I5-ultra-thin-Dell-laptop
    https://github.com/rogerjdeangelis/utl-allocating-moderately-large-r-matrices-sizes_16gb_32gb-and_125gb-on-desktop
    https://github.com/rogerjdeangelis/utl-conditionally-stop-a-running-interactive-job-without-ending-the-sas-session
    https://github.com/rogerjdeangelis/utl-create-table-with-the-top-three-values-by-group-proc-summary
    https://github.com/rogerjdeangelis/utl-find-top-and-bottom-ten-persent-of-records-by-group
    https://github.com/rogerjdeangelis/utl-flag-students-who-have-taken-intro-and-advanced-courses-on-the-same-topic-DOW-loop
    https://github.com/rogerjdeangelis/utl-how-to-get-the-top-n-largest-values-with-associated-IDs
    https://github.com/rogerjdeangelis/utl-how-to-stop-interactive-sas-processing-in-the-1980s-display-manager
    https://github.com/rogerjdeangelis/utl-locating-the-top-five-longest-lines-sas-and-r
    https://github.com/rogerjdeangelis/utl-my-85-dollar-very-slow-laptop-as-fast-as-SAS-CAS
    https://github.com/rogerjdeangelis/utl-select-the-top-n-and-bottom-n-by-group-sql-r-python-excel
    https://github.com/rogerjdeangelis/utl-select-the-top-n-most-frequent-ages-and-ecucation-levels
    https://github.com/rogerjdeangelis/utl-select-the-top-ten-rows-from-excel-table-without-importing-to-sas
    https://github.com/rogerjdeangelis/utl-top-four-seasonal-precipitation-totals--european-cities-sql-partitions-in-wps-r-python
    https://github.com/rogerjdeangelis/utl-top-n-values-by-patient-using-proc-univariate_and_means-
    https://github.com/rogerjdeangelis/utl-usefull-windows-and-unix-commands-like-stop-a-running-task
    https://github.com/rogerjdeangelis/utl_create_flag_for_top_5_percent_members_by_total_cost_for_every_group
    https://github.com/rogerjdeangelis/utl_pdf_graphics_top_40_a_sas_ods_graphics_look_at_chicago_public_schools_salaries_by_job
    https://github.com/rogerjdeangelis/utl_select_the_15_top_baseball_hitters_top_n_values_from_a_table
    https://github.com/rogerjdeangelis/utl_sql_output_multitple_tables_for_top_store_sales
    https://github.com/rogerjdeangelis/utl_web_scraping_top_cnn_stories
    https://github.com/rogerjdeangelis/utl-find-top-and-bottom-ten-persent-of-records-by-group
    https://github.com/rogerjdeangelis/utl-select-the-top-n-and-bottom-n-by-group-sql-r-python-excel

    /*               _
    | |__   __ _ ___| |__
    | `_ \ / _` / __| `_ \
    | | | | (_| \__ \ | | |
    |_| |_|\__,_|___/_| |_|

    */

    https://github.com/rogerjdeangelis/distinct-counts-for_3200-variables-and_660-thousand-records-using-HASH-SQL-and-proc-freq
    https://github.com/rogerjdeangelis/utl-append-and-split-tables-into-two-tables-one-with-common-variables-and-one-without-dosubl-hash
    https://github.com/rogerjdeangelis/utl-are-the-files-identical-or-was-the-file-corrupted-durring-transfer-hash
    https://github.com/rogerjdeangelis/utl-average-nap-time-for-three-babies-in-and-unsorted-table-using-a-hash-and-r
    https://github.com/rogerjdeangelis/utl-count-distinct-compound-keys-using-sql-and-hash-algorithms
    https://github.com/rogerjdeangelis/utl-create-a-list-of-male-students-at-achme-high-school-using_a_hash
    https://github.com/rogerjdeangelis/utl-create-a-state-diagram-table-hash-corresp-and-transpose
    https://github.com/rogerjdeangelis/utl-creating-two-tables-sum-of-weight-by-age-and-by-sex-using-a-hash-of-hashes_hoh
    https://github.com/rogerjdeangelis/utl-deduping-six-hundred-million-records-with-one-million-unique-sql-hash
    https://github.com/rogerjdeangelis/utl-deleting-multiple-rows-per-subject-with-condition-hash-and-dow
    https://github.com/rogerjdeangelis/utl-dosubl-persistent-hash-across-datasteps-and-procedures
    https://github.com/rogerjdeangelis/utl-elegant-hash-to-add-missing-weeks-by-customer
    https://github.com/rogerjdeangelis/utl-excluding-patients-that-had-same-condition-pre-and-post-clinical-randomization-hash
    https://github.com/rogerjdeangelis/utl-fast-efficient-hash-to-eliminate-duplicates-in-unsorted-grouped-data
    https://github.com/rogerjdeangelis/utl-fast-join-small_1g-table_with-a-moderate_50gb-tables-hash-sql
    https://github.com/rogerjdeangelis/utl-fast-normalization-and-join-using-vvaluex-arrays-sql-hash-untranspose-macro
    https://github.com/rogerjdeangelis/utl-hash-applying-business-rules-by-observation-when-data-and-rules-are-in-the-same-table
    https://github.com/rogerjdeangelis/utl-hash-filling-in-missing-gender-for-my-patients-appointments
    https://github.com/rogerjdeangelis/utl-hash-of-hashes-left-join-four-tables
    https://github.com/rogerjdeangelis/utl-hash-vs-summary-min-and-max-for-four-variables-by-region-for-l89-million-obs
    https://github.com/rogerjdeangelis/utl-hash_which-columns-have-duplicate-values-across-rows
    https://github.com/rogerjdeangelis/utl-in-memory-hash-output-shared-with-dosubl-hash-subprocess
    https://github.com/rogerjdeangelis/utl-loop-through-one-table-and-find-data-in-next-table--hash-dosubl-arts-transpose
    https://github.com/rogerjdeangelis/utl-make-new-column-with-the-previous-version-of-a-row-wps-hash-lag-and-r-code
    https://github.com/rogerjdeangelis/utl-multitasking-the-hash-for-a-very-fast-distinct-ids
    https://github.com/rogerjdeangelis/utl-no-need-for-sql-or-sort-merge-use-a-elegant-hash-excel-vlookup
    https://github.com/rogerjdeangelis/utl-only-keep-groups-without-duplicated-accounts-hash-sql
    https://github.com/rogerjdeangelis/utl-output-the-student-with-the-highest-grade-hash-defer-open
    https://github.com/rogerjdeangelis/utl-remove-duplicate-words-from-a-sentence-hash-solution
    https://github.com/rogerjdeangelis/utl-replicate-sets-of-rows-across-many-columns-elegant-hash
    https://github.com/rogerjdeangelis/utl-sas-fcmp-hash-stored-programs-python-r-functions-to-find-common-words
    https://github.com/rogerjdeangelis/utl-sharing-hash-storage-with-two-separate-datasteps-in-the-same-SAS-session
    https://github.com/rogerjdeangelis/utl-simple-example-of-a-hash-of-hashes-hoh-to-split_a-table
    https://github.com/rogerjdeangelis/utl-simplest-case-of-a-hash-or-sql-lookup
    https://github.com/rogerjdeangelis/utl-two-table-join-benchmarks-hash-sortmerge-keyindex-and-sasfile
    https://github.com/rogerjdeangelis/utl-two-techniques-for-a-persistent-hash-across-datasteps-and-procedures
    https://github.com/rogerjdeangelis/utl-using-a-hash-to-compute-cumulative-sum-without-sorting
    https://github.com/rogerjdeangelis/utl_benchmarks_hash_merge_of_two_un-sorted_data_sets_with_some_common_variables
    https://github.com/rogerjdeangelis/utl_hash_lookup_with_multiple_keys_nice_simple_example
    https://github.com/rogerjdeangelis/utl_hash_merge_of_two_un-sorted_data_sets_with_some_common_variables
    https://github.com/rogerjdeangelis/utl_hash_persistent
    https://github.com/rogerjdeangelis/utl_how_to_reuse_hash_table_without_reloading_sort_of
    https://github.com/rogerjdeangelis/utl_many_to_many_merge_in_hash_datastep_and_sql
    https://github.com/rogerjdeangelis/utl_nice_example_of_a_hash_of_hashes_by_paul_and_don
    https://github.com/rogerjdeangelis/utl_nice_hash_example_of_rolling_count_of_dates_plus-minus_2_days_of_current_date
    https://github.com/rogerjdeangelis/utl_select_ages_less_than_the_median_age_in_a_second_table_paul_dorfman_hash_solution
    https://github.com/rogerjdeangelis/utl_simple_one_to_many_join_using_SQL_and_datastep_hashes
    https://github.com/rogerjdeangelis/utl_simplified_hash_how_many_of_my_friends_are_in_next_years_math_class
    https://github.com/rogerjdeangelis/utl_using_a_hash_to_transpose_and_reorder_a_table
    https://github.com/rogerjdeangelis/utl_using_md5hash_to_create_checksums_for_programs_and_binary_files

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
