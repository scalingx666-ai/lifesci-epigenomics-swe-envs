# Issue

**Title:** fromstring in one-hot encoding issue

## Description

## Issue Description
The library fails when processing DNA sequences due to a breaking change in NumPy where `np.fromstring()` in binary mode has been removed.

## Error Details
```
ValueError: The binary mode of fromstring is removed, use frombuffer instead
```

**Stack Trace:**
```
File "/home/jeff/iv3/repos/tf-bind-transformer/tf_bind_transformer/data_bigwig2.py", line 100, in __getitem__
  seq = self.fasta(chr_name, begin, end)
File "/home/jeff/iv3/repos/enformer-pytorch/enformer_pytorch/data.py", line 157, in __call__
  one_hot = str_to_one_hot(seq)
File "/home/jeff/iv3/repos/enformer-pytorch/enformer_pytorch/data.py", line 67, in str_to_one_hot
  seq_chrs = torch_fromstring(seq_strs)
File "/home/jeff/iv3/repos/enformer-pytorch/enformer_pytorch/data.py", line 58, in torch_fromstring
  np_seq_chrs = list(map(lambda t: np.fromstring(t, dtype = np.uint8), seq_strs))
File "/home/jeff/iv3/repos/enformer-pytorch/enformer_pytorch/data.py", line 58, in <lambda>
  np_seq_chrs = list(map(lambda t: np.fromstring(t, dtype = np.uint8), seq_strs))
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a5974d2821c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.