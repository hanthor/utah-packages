# DELTA — wave/netfs-auth-starters (NetFsAuth)

Worktree: /tmp/parity-netfs, branch wave/netfs-auth-starters, base upstream/main aec307a.
Recipe count: 426 → 431 (+5 dirs). pinentry-gnome3 is a subpackage of the
pinentry recipe (%package gnome3), so 6 assigned names = 5 recipe dirs.

## New recipe dirs (untracked additions)
- packages/cifs-utils/ — rawhide 7.9, commit 7d394f53deee72d404af69ff3e3a77d5950b99ba
- packages/nfs-utils/ — rawhide 2.9.2, commit 26e3c738bded2e3718add203bbbe16429fce53b2
- packages/rpcbind/ — rawhide 1.3.1, commit 21e390d3b83324e7439d98ef635be4fce8652524
- packages/autofs/ — rawhide 5.1.9, commit 1e2a140aa8f797d7de0269c9a47f615ad678fca0
- packages/pinentry/ — rawhide 1.3.3, commit 5925b9a70559e697b7fc7b6560689266b264a5ac

## Modified tracked files (union-merge against siblings)
1. config/upstream-sources.json — APPEND 5 entries at end of .packages
   (after pinentry): cifs-utils, pinentry (via bootstrap --merge), then
   nfs-utils, rpcbind, autofs (hand-added, same object shape, no
   fallback_urls — same as 261 existing entries).
   - cifs-utils 7.9 https://download.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-7.9.tar.bz2 sha512 42034f9e… **plus "stage": 3** (ICU rule: cifs-utils BuildRequires libwbclient-devel from samba, an ICU linker at stage 2, so the consumer must build strictly later — same fix as gvfs at stage 3; tests/test_icu_staging.py enforced it)
   - pinentry 1.3.3 https://gnupg.org/ftp/gcrypt/pinentry/pinentry-1.3.3.tar.bz2 sha512 3562d6a8…
   - nfs-utils 2.9.2 https://www.kernel.org/pub/linux/utils/nfs-utils/2.9.2/nfs-utils-2.9.2.tar.xz sha512 3752a284…
   - rpcbind 1.3.1 https://downloads.sourceforge.net/rpcbind/rpcbind-1.3.1.tar.bz2 (https upgrade of spec's http Source0; bytes verified identical) sha512 364e8417…
   - autofs 5.1.9 https://www.kernel.org/pub/linux/daemons/autofs/v5/autofs-5.1.9.tar.gz sha512 f176eecd…
2. .packit.yaml — APPEND 5 blocks (render_packit_config.py --write):
   autofs, cifs-utils, nfs-utils, pinentry, rpcbind (alpha order in file).
3. docs/architecture.md — counts 426 → 431 (render_architecture_counts.py --write).
4. reports/factory-build-backlog.json — regen (factory_build_backlog.py):
   dev-tools already_recipe 2→12, pending 136→126; long-tail
   already_recipe 9→11, pending 170→168. Entries flipped
   pending→already_recipe: autofs, cifs-utils, cifs-utils-info,
   nfs-client-utils, nfs-common-utils, nfs-utils, nfsv3-client-utils,
   nfsv4-client-utils, pinentry, pinentry-gnome3 (+2 long-tail entries).

## Repinned manifests (part of recipe dirs, listed for review)
- packages/nfs-utils/sources: upstream re-rolled tar.xz (same tree, new
  compression bytes); repinned SHA512 to 3752a284… (was 17fffef4…).
- packages/rpcbind/sources: same, repinned to 364e8417… (was 469cddc6…).
- packages/autofs/sources: same, repinned to f176eecd… (was 30a7a47b…).
- Tree-equality proof: Fedora lookaside copies vs upstream downloads
  unpacked byte-identical (diff -r -q clean for all three pairs).
- rpcbind lock uses https:// although spec Source0 is http:// (validate
  forbids http primaries; exact https URL fetched, digest matches).

## Intra-wave BuildRequires
- None of the 5 BuildRequire each other. External factory deps, all
  already landed: cifs-utils → samba (libwbclient-devel); pinentry →
  gcr, libsecret, gtk2 (gtk2/qt/kf6 flavors per spec bconds).
  Non-factory BRs (rpcgen, libsss_autofs, krb5/ldap stacks) resolve from
  the buildroot, not this factory — no blocker.
- samba-winbind NOT touched (Closures lane).

## Validation
- tools/validate.py: exit 0 (431 recipes, 427 rawhide + 4 upstream).
- just check: exit 0.
- just test (pytest): 1273 passed, 2 skipped, 2649 subtests passed.
- pre-commit run --all-files: passed (one hook stripped a trailing blank
  line in unrelated docs/skills/package-build-cache.md; reverted to keep
  this wave scoped).
- Commit 1fac1e5 on branch wave/netfs-auth-starters. DELTA.md left
  untracked by design.
