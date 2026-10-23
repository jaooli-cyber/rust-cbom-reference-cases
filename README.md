# Rust CBOM reference cases

Source-based reference labels and controlled cases for checking what Rust CBOMs
say about cryptographic operations, parameters and locations. Prepared by
José Ángel Olivares, [BOM Fidelity / Global Trade Origin](https://bomfidelity.globaltradeorigin.com/),
in collaboration with Alessandro Perez's generator evaluation.

| Case | Material | Review status |
| --- | --- | --- |
| cbom-real-crates-app | Original and five source variants, labels and closed correction findings | Original-case review points closed with the author |
| jwt-cli 6.1.1 | Frozen binary source reference and CBOM representation | Generator comparison awaiting author clarification |
| hkdf 0.12.4 | Frozen public-API library reference and CBOM representation | Generator comparison awaiting author clarification |

Start with [methodology](METHODOLOGY.md), [reproduction](REPRODUCE.md) and the
[original-case report](reviews/original/REPORT.md). Files under cases/original
are a declared publication projection of the original frozen labels; round-two
label and CBOM files retain their frozen bytes. [Publication scope](PUBLICATION.md)
describes the edition and checking limits.

```sh
python3 -B verify_publication.py .
```

Cite the v1.0.0 release using [CITATION.cff](CITATION.cff). This is a reference
dataset and review, not the BOM Fidelity product or a general Rust scanner.
Third-party crate archives are unmodified and retain their bundled licences.
Our review material and standalone verifier are Apache-2.0.

Related external check: [CycloneDX Java and Dependency-Track](https://github.com/jaooli-cyber/cyclonedx-java-consumer-review).
