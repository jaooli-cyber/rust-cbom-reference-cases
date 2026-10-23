# Rust reference methodology

Review pinned sources and resolved dependencies before consulting generator
output. Preserve the initial labels; post-exchange explanations and corrections
belong to separate review records. Match an occurrence using source identity,
file, one-based Unicode line and column, algorithm and origin identifiers.
Zero-based offsets are columns minus one, not UTF-8 byte offsets.

Separate operation, initialization, configuration and compiled presence. Reachability
is static and conditional, not evidence that a particular run used an algorithm.
Record fixed parameters separately from caller-dependent or unresolved values.
Do not turn an unresolved parameter into a negative or infer a library's generic
algorithm from an example instantiation.

Dependency internals may be folded into an application operation by a generator.
The original reference's 41 dependency sites are therefore not 41 independent
omissions. Origin mappings explain granularity; missing parameters and relations
are separate questions. Native and typed utility boundaries retain their stated
scope and are not claims of a complete audit of machine code or every dependency.

For the original binary, main is the root. For the second-round library, public
functions, methods and statics are roots with caller arguments left open. Tests,
examples and benches are excluded. hkdf is itself the HKDF implementation, so a
generator reporting calls into crypto dependencies can represent its HMAC calls
without naming HKDF at every local extract or expand implementation site.

The original five controls remove MD5, replace application SHA-256 with SHA-512,
change PBKDF2 rounds, move call sites and retain initialization without application
operations. Do not overwrite the original source with a control variant.
