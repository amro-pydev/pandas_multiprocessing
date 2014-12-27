MP_Pandas
=========

Pandas' group-by/apply with multiprocessing
===========================================

Pandas is a Python library used for data manipulation.  The main Pandas data structure is the data frame which is very similar to the R data frame. One of the main processing paradigms on data frames is the group-by/apply which is essentially map/reduce. The syntax for group-by/apply is:

                    data_frame.groupby(column_list).apply(apply_func, *args, **kwargs)

When the computation to be performed by the apply function is CPU intensive and/or the number of data frame groups is large, a group-by/apply computation can easily become a processing bottleneck. 
The Pandas library is very fast and in general one is better off leveraging its capabilities. The first and best bottleneck removal step is to make sure that your apply function is optimized. Similarly, often structuring the data differently may lead to a reduction in the number of groups generated by the group-by step. If after having implemented these two steps your group-by/apply is still a processing bottleneck, one may want to consider using multiprocessing.

The group-by/apply operation is, in general but not always, embarrassingly parallel. Here we assume that the group-by/apply is embarrassingly parallel.
The basic idea in a multiprocessor group-by/apply is to assign groups resulting from the group-by step to different CPUs. Enabling multiprocessing in a Pandas group-by/apply is interesting if it is generic in the sense that we preserve the syntax and we do not need to rewrite a special apply function code. Our multiprocessing syntax is as follows:

                mp_groupby(data_frame, column_list, apply_func, *args, **kwargs, **mp_args)

The arguments to mp_groupby() are the same as in the Pandas groupby/apply except for the additional mp_arg argument, which contains multiprocessing information such as the number of CPUs to use and load balancing information.
