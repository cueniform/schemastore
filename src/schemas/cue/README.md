# CUE schemastore

This is an experimental use of the JSONSchema documents curated by SchemaStore, translating them into the similar-but-different [CUE](https://cuelang.org) configuration language.

CUE's [import feature](https://cuelang.org/docs/concepts/packages/) is extremely well suited to the concept of there being an external, authoritative source of unopinionated (i.e. not /policy/-related) schema definitions.

However, the mechanism by which those definitions are /fetched/ is [still being designed and developed](https://github.com/cue-lang/cue/issues/851). Because of this uncertainty, the only version that this importable CUE module currently makes available is `v0`. **Using this `v0` means that you're aware that, as the CUE ecosystem refines its module referencing/fetching/installing/sharing processes, you might have to update your CUE files that refer to this module. By using `v0`, you accept this potential future burden**! A `v1` will be declared at some point in the future, which will bring stability guarantees that aren't yet possible.
