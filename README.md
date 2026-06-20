# exoas-profiles — Exocortex profiles AssetSpace

**The mount manifests for [Exocortex](https://github.com/kitelev/exocortex).** This repository is an **AssetSpace** — a git-backed package of vault assets you mount into an Exocortex vault. It holds the `exo__Profile` assets (the `$$`-profiles): each one is a **manifest / bill-of-materials** naming which AssetSpaces to mount, so applying a profile turns a vault into a specific working set (personal, work, reading, …).

> **Where this fits.** In the [Exo-as-SDK topology](https://github.com/kitelev/exocortex/blob/main/docs/explanation/assetspace-sdk-topology.md): `exo` = the platform, an AssetSpace = a library/jar, and a **Profile = a manifest** (like a `package.json` / BOM). A profile lists *leaf* AssetSpaces in `exo__Profile_includes`; the engine then resolves transitive dependencies through the [`exoas-registry`](https://github.com/kitelev/exoas-registry) `dependsOn` DAG and mounts the full effective set. Sensitive AssetSpaces are physically absent from disk unless a mounted profile includes them — that's the privacy boundary between, say, work and personal data.

---

## What's in here

A single namespace folder, `profiles/`, with one `exo__Profile` asset per profile (defined in [`exoas-exo`](https://github.com/kitelev/exoas-exo)).

### Profile shape

```yaml
---
exo__Instance_class:
  - "[[exo__Profile]]"
exo__Asset_label: "$$kitelev-my"
exo__Profile_description: "Andrey's personal knowledge — life, notes, reading."
exo__Profile_includes:
  - "[[exoas-my]]"
  - "[[exoas-exocmd]]"
---
```

| Property | Purpose |
| --- | --- |
| `exo__Profile_includes` | The leaf AssetSpaces this profile mounts (transitive deps resolved via the registry). |
| `exo__Profile_description` | Human description of what the profile is for. |

### Profiles in this repo

| Profile | What it mounts |
| --- | --- |
| `$$core` | Minimal floor — only `exoas-exo` (the TS-floor); nothing personal. |
| `$$kitelev-my` | Andrey's personal knowledge (life, notes, reading). |
| `$$kitelev-tbank` | Andrey's T-Bank work space. |
| `$$kitelev-exodev` | The Exocortex-development / infrastructure space. |
| `$$kalashnikova-my`, `$$levina-tbank`, `$$mudriy-tbank` | Peer-tester profiles. |

> The TS-floor (`exoas-exo`) is always mounted regardless of profile and is never unmounted; `exoas-exocmd` / `exoas-w3c-aggregated` come along via `includes`/`dependsOn`. Every profile above is a live asset on disk in `profiles/`.

---

## Using this AssetSpace

Profiles are the front door to a working vault. After bootstrapping, you **apply** one:

- **CLI:** `npx @kitelev/exocortex-cli profile-apply --vault ~/vault $$kitelev-my` — mounts the effective set (and unmounts everything not in it, TS-floor excepted). Private AssetSpaces may need a `--token`.
- **Plugin:** command palette → **Exocortex: Apply profile** → pick a profile. The engine resolves its dependencies through the registry and mounts/unmounts to match.

Once mounted, profile assets appear under `assetspaces/kitelev/exoas-profiles/profiles/`. The active profile is tracked per-device (it does not sync across devices).

## Conventions (for contributors)

- **UID-canon.** Every profile is a UUID-named file (`<exo__Asset_uid>.md`); the `$$name` lives in `exo__Asset_label`.
- **`includes` = leaves only.** List the leaf AssetSpaces you want; let `dependsOn` (in the registry) pull in the rest. Don't enumerate the whole transitive closure by hand.
- **Floor is implicit.** Don't rely on a profile to mount `exoas-exo` — it is the always-on TS-floor; stripping it would leave the vault with no class definitions.
- **CI.** Pushes run the shared [AssetSpace SHACL gate](https://github.com/kitelev/exoas-ci) (`validate schema --shapes-mode`).

## See also

- [exoas-registry](https://github.com/kitelev/exoas-registry) — the AssetSpace catalogue + `dependsOn` DAG these profiles resolve against.
- [exoas-exo](https://github.com/kitelev/exoas-exo) — defines the `exo__Profile` class these assets instantiate.
- [Exocortex engine repo](https://github.com/kitelev/exocortex) — Profile docs: [`docs/explanation/profile.md`](https://github.com/kitelev/exocortex/blob/main/docs/explanation/profile.md).
- [Exo-as-SDK topology](https://github.com/kitelev/exocortex/blob/main/docs/explanation/assetspace-sdk-topology.md).

## License

MIT — see [`LICENSE`](./LICENSE).
