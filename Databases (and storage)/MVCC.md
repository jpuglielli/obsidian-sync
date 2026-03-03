Multi-version concurrency control

MVCC at a high level:
1. **Writes create new tuple versions** rather than overwriting in place, tagged with the creating transaction's ID
2. **Each transaction's snapshot** determines which tuple versions are visible to it — based on transaction IDs and commit status
3. **Reads never block writes, writes never block reads** — because they're operating on different physical versions of the row
4. **Dead tuples accumulate** and VACUUM cleans them up