# Plan for issue #72

## Evidence used

The reproduction on commit `2f4e82f` used macOS 26.6.2 arm64, Python
3.12.4, passlib 1.7.4, bcrypt 4.3.0, and pytest 9.1.1. The baseline was
375 passed and 53 xfailed.

The issue's test uses the stored hash `"not_a_valid_bcrypt_hash"`. With
`--runxfail`, it raises `passlib.exc.UnknownHashError: hash could not be
identified` instead of returning `False`. The same behavior occurs for an
empty hash and an unsupported md5-crypt hash. A truncated bcrypt hash raises
`ValueError: salt too small`, so catching only `UnknownHashError` would leave
another malformed-hash path uncaught. A valid hash still returns `True` for
the correct password and `False` for the wrong password.

The login route calls `verify_password` and turns unexpected exceptions into
HTTP 500. The reproduction therefore observes HTTP 500 for malformed stored
hashes, while a normal wrong password correctly returns HTTP 401.

## Diagnosis

`core/security.py:37` calls `pwd_context.verify(...)` without handling the
exceptions passlib raises while parsing or validating malformed stored hashes.
The authentication helper's contract is to return a boolean, so those
malformed-hash cases should fail closed as `False` rather than escape to the
login route.

## Scope

In scope:

- Update `verify_password` in `core/security.py` to return `False` for the
  observed passlib malformed-hash exceptions: `UnknownHashError` and the
  `ValueError` raised for the truncated bcrypt input.
- Remove the `strict=True` xfail marker from
  `tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format`.
- Add focused regression cases for the unparseable, empty, and truncated
  bcrypt hashes while preserving the existing valid-hash controls.

Out of scope:

- Changes to `api/routes/auth.py`; it should receive the helper's boolean
  result and keep its existing 401 behavior.
- Changes to passlib, bcrypt configuration, password hashing, or database
  migrations.
- The unrelated `vector-db`/NumPy 2 startup failure from setup.

## Files and areas

- `core/security.py`: exception handling in `verify_password`.
- `tests/unit/test_security.py`: remove the strict xfail and add regression
  inputs for malformed hashes.

## Approach

1. Import the passlib exception namespace needed to identify
   `UnknownHashError`.
2. Wrap only the `pwd_context.verify` call in `verify_password` and return
   `False` for `passlib.exc.UnknownHashError` and the `ValueError` observed
   for malformed bcrypt data. Keep successful verification and normal wrong
   passwords unchanged.
3. Remove the strict xfail marker so the original issue test becomes a real
   regression test.
4. Add focused assertions for the empty and truncated-bcrypt boundaries,
   alongside the existing malformed literal and valid-hash controls.

## Test plan

Run the focused security tests without `xfail` masking:

```bash
.venv/bin/pytest \
  tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format \
  -q --tb=long
```

Expected after the change: the test passes instead of reporting an
`UnknownHashError` or an `XPASS(strict)`.

Run the full security unit-test module:

```bash
.venv/bin/pytest tests/unit/test_security.py -q
```

Expected after the change: malformed, empty, and truncated hashes return
`False`; a valid hash with the correct password returns `True`; a valid hash
with the wrong password returns `False`; and the module passes without new
failures.

Re-run the login boundary manually or with the existing route tests. Expected
after the change: each malformed stored hash produces the existing invalid-
credentials HTTP 401 response rather than the generic HTTP 500 response.

## Risks and unknowns

- The issue title names `UnknownHashError`, but the reproduction also finds a
  `ValueError` for truncated bcrypt data. The plan includes that exception
  because it is another observed malformed-hash input that violates the same
  boolean helper contract; this choice should be called out for maintainer
  review.
- Catching `ValueError` is intentionally limited to the `pwd_context.verify`
  call. No broader route-level exception handling is added.
- The reproduction did not change the database or test the vector database
  service; neither is needed for this fix.
