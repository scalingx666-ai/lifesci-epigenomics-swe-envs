# Issue

**Title:** Fix silent error swallowing and resource cleanup bugs from code review

## Description

- Stop catching bare exceptions in the progress bar polling loop
  (was hiding KeyboardInterrupt and unrelated bugs); catch only
  AttributeError, which is what the private _number_left access
  guards against.
- Guard the default chunksize calculation against ZeroDivisionError
  when a pool has no workers.
- Fix _ParallelAsyncResult.get() so the pool is joined for cleanup
  even when the wrapped call raises the worker's exception, while
  still not blocking on a plain TimeoutError.
- Replace isinstance(x, typing.Callable) with callable(x).
- Remove long-dead commented-out imap/istarmap draft code.
- Fix stale variable names in _func_star_many's docstring.
- Document the Windows/macOS "spawn" __name__ == "__main__" guard
  requirement in the README.

Claude-Session: https://claude.ai/code/session_01CCVXqX6vkWNBxHJ85zgAgz

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a6369e84124`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.