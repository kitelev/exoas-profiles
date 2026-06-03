# exoas-profiles

Exocortex AssetSpace для Knowledge Profile и Focus Profile instances.

**Created:** 2026-06-04 per RFC [Phase 6 Bootstrap + Knowledge/Focus split](https://github.com/kitelev/vault-exodev/blob/main/inbox/13da049f-30e6-43d6-a07b-1848d24838f8.md) + sibling onto-RFC [52f2acdd](https://github.com/kitelev/vault-exodev/blob/main/inbox/52f2acdd-b22d-4372-8f94-351ccca7d01c.md).

## Structure

Plain flat directory с UUID-named markdown файлами:

```
.
├── README.md
├── LICENSE
└── <profile-uid>.md   # exo__KnowledgeProfile OR exo__FocusProfile OR dual-class instances
```

## Class membership

Profile assets могут принадлежать:
- `exo__KnowledgeProfile` (hard switch — filesystem materialization, slow change)
- `exo__FocusProfile` (soft switch — RDF query filter, fast change)
- Both (dual-class — legacy migration default)

Plugin filters instances by `Instance_class` для palette commands routing.

## Cloning

Standard Exocortex submodule pattern:

```bash
git submodule add https://github.com/kitelev/exoas-profiles.git assetspaces/profiles
```

OR via Plugin Bootstrap palette (Phase 6.2 — Exocortex: Bootstrap vault).

## Related

- TBox classes — kitelev/exocortex-exo-ontology (`exo__KnowledgeProfile`, `exo__FocusProfile`, properties)
- Plugin — kitelev/exocortex (FocusProfileSwitchManager, palette commands)
- Migration tool — `exocortex migrate shared-identities` (CLI, Phase 6.5e)

## License

MIT
