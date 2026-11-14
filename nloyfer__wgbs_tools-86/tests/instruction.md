# Issue

**Title:** Fix for issue reading beta_to_450k reference

## Description

Hi, I was using wgbstools for some analysis and had some problems using the beta_to_450k functionality, which I've debugged on my end. I'm making a pull request in case any one else would find this useful

- environment:
 running python 3.11.8 in a conda environment on commit ```e7bf6c6fced4f12f615b12adc97a37d495d37799```, with reference genome hg38 

- The issue
Trying to run ``` wgbstools beta_to_450k --genome hg38 <FILE>.beta``` produced the following error:

<details>
<summary>error running beta_to_450k</summary>

```text
multiprocessing.pool.RemoteTraceback: 
"""
Traceback (most recent call last):
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 125, in worker
    result = (True, func(*args, **kwds))
                    ^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 51, in starmapstar
    return list(itertools.starmap(args[0], args[1]))
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbs_tools/src/python/beta_to_450k.py", line 18, in single_beta
    values = beta2vec(load_beta_data(beta_path)[indices - 1], min_cov=cov_thresh)
                      ~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^
IndexError: arrays used as indices must be of integer (or boolean) type
"""

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "BIN_PATH/bin/wgbstools", line 97, in <module>
    main()
  File "BIN_PATH/bin/wgbstools", line 64, in main
    importlib.import_module(args.command).main()
  File "/LOCATION/wgbs_tools/src/python/beta_to_450k.py", line 138, in main
    betas2csv(args)
  File "LOCATION/wgbs_tools/src/python/beta_to_450k.py", line 98, in betas2csv
    arr = p.starmap(single_beta, params)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 375, in starmap
    return self._map_async(func, iterable, starmapstar, chunksize).get()
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 774, in get
    raise self._value
IndexError: arrays used as indices must be of integer (or boolean) type
```
</details>

This is fixed by casting the indices to type int

The error then changed to
<details>
<summary>Second error</summary>

```text
LOCATION/wgbs_tools/src/python/beta_to_450k.py:95: RuntimeWarning: invalid value encountered in cast
  indices = np.array(df['cpg']).astype(int)
multiprocessing.pool.RemoteTraceback: 
"""
Traceback (most recent call last):
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 125, in worker
    result = (True, func(*args, **kwds))
                    ^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 51, in starmapstar
    return list(itertools.starmap(args[0], args[1]))
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbs_tools/src/python/beta_to_450k.py", line 18, in single_beta
    values = beta2vec(load_beta_data(beta_path)[indices - 1], min_cov=cov_thresh)
                      ~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^
IndexError: index 9223372036854775807 is out of bounds for axis 0 with size 29401795
"""

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "BIN_PATH/bin/wgbstools", line 97, in <module>
    main()
  File "BIN_PATH/bin/wgbstools", line 64, in main
    importlib.import_module(args.command).main()
  File "LOCATION/wgbs_tools/src/python/beta_to_450k.py", line 138, in main
    betas2csv(args)
  File "LOCATION/wgbs_tools/src/python/beta_to_450k.py", line 98, in betas2csv
    arr = p.starmap(single_beta, params)
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 375, in starmap
    return self._map_async(func, iterable, starmapstar, chunksize).get()
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "LOCATION/wgbstools/lib/python3.11/multiprocessing/pool.py", line 774, in get
    raise self._value
IndexError: index 9223372036854775807 is out of bounds for axis 0 with size 29401795
```
</details>

Note, that 9223372036854775807 was the sys.maxsize on my system, so presumably this error is the result of casting null or inf values to int. Without the int typecast, however, the first error reoccurs

With these changes though, beta_to_450k functioned as expected, and the results of downstream analysis also came out as expected

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e7bf6c6fced4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.