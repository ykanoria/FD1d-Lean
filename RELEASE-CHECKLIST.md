# Release Checklist

1. Review `formalization.yaml`, especially authorship, source provenance,
   Apache-2.0 licensing, AI disclosure, scope, and the resource-accounting
   limitation.
2. Confirm the manuscript source checksums:

   ```sh
   sha256sum --check manuscript/SHA256SUMS
   ```

3. Run metadata and repository checks:

   ```sh
   git diff --check
   ruby scripts/validate-formalization.rb
   check-jsonschema \
     --schemafile https://raw.githubusercontent.com/mathlib-initiative/formalization.yaml/99c678e569c7c4c0772db297c5ddd5e4c9b6322e/schema/formalization.schema.json \
     formalization.yaml
   ```

4. Run the complete Lean build:

   ```sh
   lake exe cache get
   lake build
   ```

5. On Linux, run the pinned Comparator and NanoDa replay:

   ```sh
   ./scripts/verify-comparator.sh
   ```

6. Confirm that no generated build output, credential, symlink, submodule, or
   unrelated development artifact is tracked.
7. Commit every source, manifest, metadata, configuration, and documentation
   file.
8. Push the commit to a public GitHub repository and wait for all CI jobs.
9. Record the full immutable commit:

   ```sh
   git rev-parse HEAD
   ```

10. Before starting the Palomar agent protocol, show the owner the repository,
    commit, `comparator.json`, and proposed authorization relationship. The
    owner must explicitly approve an actual submission and later decide
    whether to register the resulting review.

Submit only the full 40-character public commit SHA. Branches, tags, and
uncommitted work are not immutable Palomar inputs.
