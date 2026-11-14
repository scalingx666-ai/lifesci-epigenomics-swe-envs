# Issue

**Title:** Add Continuous Integration script to reproduce error `assert q.is_cuda and k.is_cuda and v.is_cuda`

## Description

Dear DNABERT 2 maintainer,

With this Pull Request I'd like to prove that the documentation is incomplete, by reproducing the error

```
assert q.is_cuda and k.is_cuda and v.is_cuda
```

This error is reported too in #11, #20, #34, #96 and #112.

I made the error reproducible with the script in `.github/workflow/check_build.yaml`, where I copy-paste the install process only. I simply followed the install procedure in the README, using the recommended Python 3.8, yet without the conda setup:

```text
- uses: actions/setup-python@v2
  with:
    python-version: 3.8

- name: install required packages
  run: python3 -m pip install -r requirements.txt

- name: run example code shown in README
  run: python3 .github/workflows/example_huggingface_v4_28.py
```

The script it runs is copied from the README too:

```python
import torch
from transformers import AutoTokenizer, AutoModel

tokenizer = AutoTokenizer.from_pretrained("zhihan1996/DNABERT-2-117M", trust_remote_code=True)
model = AutoModel.from_pretrained("zhihan1996/DNABERT-2-117M", trust_remote_code=True)

dna = "ACGTAGCATCGGATCTATCTATCGACACTTGGTTATCGATCTACGAGCATCTCGTTAGC"
inputs = tokenizer(dna, return_tensors = 'pt')["input_ids"]
hidden_states = model(inputs)[0] # [1, sequence_length, 768]

# embedding with mean pooling
embedding_mean = torch.mean(hidden_states[0], dim=0)
print(embedding_mean.shape) # expect to be 768

# embedding with max pooling
embedding_max = torch.max(hidden_states[0], dim=0)[0]
print(embedding_max.shape) # expect to be 768
```

The error log is shown in the next message below.

I think that a user should be able to install a Python module and run example code with success by reading the documentation, hence I think the documentation is incomplete or the error message given is not helpful enough (#112). 

I hope you appreciate the effort I put in to make DNABERT 2 even better for everyone. Maybe using CI scripts is new to you, I'd be happy to discuss their pros and cons and I'd be happy to maintain such scripts for my users :-)

Cheers, Richel Bilderbeek

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0e55cc4f25f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.