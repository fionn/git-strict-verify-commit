# git strict-verify-commit

## Background

Git's native commit and tag signature verification verifies the signature against the keys available in the local keyring or `gpg.ssh.allowedSignersFile`. It does not verify that the signature matches the committer or tagger.

This is discussed in [_verify-commit: scriptable commit signature verification_](https://lore.kernel.org/git/20140613080229.GJ7908@sigill.intra.peff.net/), where the consensus is that this is a policy and Git itself should be unopinionated about it, with the specific counterpoint that the committer might use a "plus address" that doesn't match that of a key identifier.

This allows committer _A_ to impersonate committer _B_ by forging a commit attributed to _B_ and signing it with _A_'s key, so long as the verifying party trusts both keys and doesn't manually inspect the output of `git verify-commit` and compare the key identity with the committer identity, which is common if the commit verification is automated.

## "Strict" Verification

This is a strict version of `git verify-commit`, only for commits signed with SSH keys. If unable to match the committer to a key, we fall back to verifying the signature anyway (i.e. how `git verify-commit` works) but print the error and exit with a non-zero status.

## Installation and Usage

Link `git-strict-verify-commit` to somewhere in `PATH`.

Verify a commit with `git strict-verify-commit [<commit>]`. We are more forgiving than `git verify-commit` as we default to `HEAD` if the argument is omitted.
