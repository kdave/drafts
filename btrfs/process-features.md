What should we considered when starting to implement a new feature:

1. use case - how it's used, examples
2. problem description - what is being solved or improved
3. solution - how it's solved, design document, snippets of key code or conceptual implementation

## Small features

Unintrusive to most of other code, isolated to one area, do not need backward compatibility, no on-disk format changes. Can be finished in 1-2 releases.

- preparatory work, cleanups refactoring
- main work
- post cleanups, documentation updates, exposed to users so reports come
- endless fixups, stable backports

## Moderate features

May require updates to generic code and span more than one area inside btrfs, may introduce new incompat bit and on-disk format change. Backward compatibility has to be maintained.

Patches go in in the same order as for small feature, enabling the feature and adding the incompat bit must be done in one change (due to bisectability).

User space side support must be finished before the final release but may be released in btrfs-progs earlier to ease testing. All relevant tools must be able to understand the new feature format (namely btrfs-check).

## Big features

Everything that may touch several areas, may require to rewrite some subsystem or introduce internal API to wrap and hide further changes (to avoid too many merge conflicts). On disk format changes can be expected.

Timeframe for completion is several releases and years to finalize. Incremental changes should be possible but must not interfere with normal code and operation (hidden behind config option DEBUG is possible), no regressions.

Design should be outlined from the beginning and should not change much over time, while the implementation could change eventually if some way turns out to be wrong.

This is the most problematic feature because of the scope and implementation time, things need to be resolved and steered on demand and while things are in progress.

### Refinements

possible Big feature refinements:

- split into smaller features and implement separately (as Moderate features)
- have a lot of design-level review first, make time for that before adding new code
- have a lot of implementation level review, once design is set, there's always more than one way to do it but at least this has more flexibility
- feature shield can be either CONFIG_BTRFS_DEBUG or some other way, like sysfs knob, in the worst case a mount option (not persistent though)
