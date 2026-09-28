# Unit 3 — Plan and Build

## Posted upstream

**GitHub username**

sseid4

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5880154629

I reproduced issue #72 on macOS with Python 3.12.4, passlib 1.7.4, and the
repository at commit `2f4e82f`. The malformed hash in the existing security
test raises `UnknownHashError`, and malformed hashes also turn the login
endpoint's expected 401 into a 500. The valid-hash controls still return True
for the correct password and False for the wrong password.

I plan to make `verify_password` fail closed for both exception types observed
in the reproduction: `UnknownHashError` for unparseable and empty hashes, and
`ValueError` for the truncated bcrypt hash. The issue title names
`UnknownHashError`, but catching only that exception would leave the truncated
bcrypt input returning 500, so I plan to catch both within the helper. I will
then remove the strict xfail and add focused regression cases in
`tests/unit/test_security.py` for those inputs while preserving valid-hash
behavior. The unrelated vector-db setup failure is outside this plan. If the
maintainers want this issue limited to `UnknownHashError`, should I narrow the
change and leave the truncated-bcrypt case for a separate issue?

## Your branch

**Branch**

fix/72-malformed-password-hash

**Branch link**

https://github.com/sseid4/pathreview-ai301-fa26-s3/tree/fix/72-malformed-password-hash

**Evidence**

Before the change:

```bash
.venv/bin/pytest \
  tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format \
  --runxfail -q --tb=long
