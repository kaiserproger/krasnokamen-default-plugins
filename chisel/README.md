# Chisel

Optional datapack-style addon installed as a server package; its custom item
model and texture additionally need a consenting Loader client. Right-click a
block that has stair/slab variants while holding the chisel:

- TOP face -> top slab (one matching slab granted, dropped if the inventory
  is full),
- BOTTOM face -> bottom slab (same grant),
- SIDE face -> stairs facing that side (nothing granted),
- right-click stairs -> slab, copying `half`/`waterlogged` (nothing granted).

The tool is `chisel:chisel` (vanilla `minecraft:stick` carrier, stack 1,
durability 250, 1 wear per use), crafted from an iron ingot over a stick
(shaped, vertical). Covers every vanilla full block with both `_slab` and
`_stairs` variants (58 families, 232 native block-tool rules).

This is a `server_and_client` package whose Loader bundle (item definition,
model and texture) is optional: `chisel/plugin.toml` declares
`required = false`. In a deployment whose bundles are all optional, core
Configuration sends a `krasnokamen:loader/probe` payload before known-pack
negotiation. A client that answers `krasnokamen:loader/hello` negotiates the
manifest for the platforms the bundle declares (`fabric`, `neoforge`, `forge`,
`native`), consents to `register_items` and `load_assets`, and receives the
item definition, model and texture.

A client that does not answer — vanilla, or a Loader build older than the
probe — completes Configuration without any Loader content and is not refused.
Core then omits non-`minecraft:` `item_model` components from the stacks it
sends that client, so `chisel:chisel` shows as its `minecraft:stick` carrier
with its `Chisel` name instead of the vanilla missing-model cube. The
block-tool rules match the authoritative stack, which keeps the model, so that
client gets the same conversions. One required bundle anywhere in the
deployment restores the refusal: every client negotiates and a client without
the Loader is disconnected during Configuration naming the Loader requirement
(core `docs/plugins/CONTRACT.md`).

Install the `chisel` package alongside the other packages in
`[plugins].directory` — it is opt-in and not part of the five-package standard
pack.

Build the component with
`tools/build_standard_pack.py --core-root ../solaris --package chisel`;
rebuild the tracked `chisel/client/chisel-content.zip` bundle (and the hashes
in `chisel/plugin.toml`) after changing `sources/chisel/client-src` with:

```sh
python3 sources/chisel/build_bundle.py
```

The bundle and the index it carries are deterministic (fixed timestamps,
sorted entries): rerunning the script without source changes is a no-op.
`chisel/plugin.wasm` is built, never committed by hand.
