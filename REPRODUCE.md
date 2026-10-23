# Checking and reproducing the Rust cases

## Read only verification

Use Python 3.11 or newer, without third-party packages:

```sh
python3 -B verify_publication.py .
```

The verifier checks every manifest entry, archive identity and the published
label source hashes and positions. It reads crate members without extracting
or executing them. Independently check source semantics against the labels;
hash verification alone cannot establish that a label is correct.

## Compiler reproduction

Use a disposable Linux environment with no credentials or shared personal
folders, bounded CPU, memory and disk, and restricted networking. Crate builds
can execute build scripts and procedural macros. Obtain the toolchain and all
locked dependencies in a separate acquisition phase; then disable networking.

The pinned configuration is nightly-2026-09-25, rustc 1.100.0-nightly
(f7575a9da 2026-09-24), x86_64-unknown-linux-gnu, release profile and default
features. The MIR flags are `-Zalways-encode-mir -Zmir-opt-level=0
-Zspan-free-formats`. The original Cargo.lock is in cases/original/source.

```sh
RUSTFLAGS='-Zalways-encode-mir -Zmir-opt-level=0 -Zspan-free-formats' \
  cargo +nightly-2026-09-25 check --frozen --workspace --profile release \
  --target x86_64-unknown-linux-gnu --manifest-path Cargo.toml
```

For round two, unpack the unmodified jwt-cli 6.1.1 or hkdf 0.12.4 source archive
in the disposable environment and copy the corresponding supplied lock into
that crate's root as Cargo.lock. Use `--bin jwt` for jwt-cli and `--lib` for hkdf
instead of `--workspace`. No library harness is used. The bundled archives cover
reviewed source bindings, not a complete offline build cache; Cargo.lock fixes
the additional build dependencies that must be acquired separately.

This command reproduces compiler acceptance of a case, not Alessandro's private
generator. His received CBOMs are not included, so the closed comparison tables
are published observations, not a claim that this repository reruns his driver.
