# Review of the six updated Rust CBOMs

20 October 2026. All six v2 documents contain the announced corrections.
The comparison found no additional content changes beyond those corrections,
the nonce provenance update and new document serial numbers. The frozen
reference v1 and the previously received CBOMs remain unchanged.

## Corrections verified

| Item | Result |
| --- | --- |
| Argon2 constituents | BLAKE2b-256 and BLAKE2b-512 are algorithm components in the original and four applicable variants. Each has an occurrence at the `hash_password` call, marked as part of Argon2id-19456-2-1. |
| Relationships and provider | Argon2id has `dependsOn` edges to both constituents; `blake2 0.10.6` provides them. All document references resolve. |
| Initialization-only control | Neither BLAKE2 constituent is asserted in this case. Argon2 setup is not counted as the password-hashing operation. |
| TLS builder | All six application occurrences explicitly say `configuration, not a connection`. |
| MD5 | `rcbom:param:output_bytes=16` is present wherever the algorithm is applicable. |
| BLAKE3 | `rcbom:param:mode=unkeyed` is present wherever applicable. The previously confirmed 256-bit output interpretation remains unchanged. |
| Argon2 | `rcbom:param:version=19` and `rcbom:param:output_bytes=32` match the fixed source expectations for setup and the operation. The component name remains Argon2id-19456-2-1. |

All **31 applicable parameter dimensions** now match; five dimensions do not
apply because the corresponding calls were removed. Component properties are
treated as aggregate claims, not additional per-call occurrences.

## Nonce provenance

The AES-128-GCM and AES-256-GCM occurrences at
`rustls-0.23.40/src/crypto/aws_lc_rs/tls12.rs:288` now include `computed` in
their nonce context and aggregate provenance property, while retaining
`parameter`. These are the two changed dependency occurrences in every case.

The pinned rustls source constructs the 12-byte nonce by copying `dec_salt`
into its first four bytes and the record payload's first eight bytes into the
remainder before `open_in_place`. Its SHA-256 is
`9e3a9220b68ce2a340d2289d376107bf4b75bc73319d1562c2800c5c01476d28`.
The additional provenance description agrees with that source. This is a
source-level check, not a claim that this program establishes a TLS connection
or executes a handshake.

## Regression and comparison method

The existing application controls, explicit operation counts, configured-only
identifiers and **24 package-usage checks** remain unchanged. The preparation
call `SaltString::encode_b64` remains outside the crypto-operation occurrence
projection, as in v1. Internal dependency operations remain folded into their
application origins; the new constituents do not imply separate coverage of
every internal source site.

The comparison matches components by unique `bom-ref` and dependency entries
by unique `ref`. It ignores only ordering of component, dependency, property
and edge-target lists, plus the new document serial number. It preserves
multiplicity and rejects duplicate references. The allowed field changes are
explicit; unrelated alterations are not absorbed by a general normalization.

All six documents pass the pinned CycloneDX 1.7 root schema with the separately
captured cryptography registry schema from 15 October. 

**72 checks passed in each of two isolated VPS runs**, including negative
controls for incorrect parameters, positions, provenance, builder
classification, missing constituents, provider and dependency edges,
unrelated changes, duplicate records and unsafe archive entries. All five
canonical report artifacts match byte for byte between the two runs.

## Inputs and evidence

Received archive: `variants-v2.zip`, 187,232 bytes, six JSON files.
SHA-256:
`d46b0287bb71506897c267916819d6e59ca103fa1fcb424086d323480d6dc7b8`.
The ZIP contains no new execution log; the unchanged build and command are
the supplier's statement. This review reads the documents and pinned sources;
it does not execute the generator, Rust code or archive scripts.

The author subsequently confirmed closure of these original-case review points.
The generator CBOMs are not redistributed here; the report and derived result
tables describe the received documents.
