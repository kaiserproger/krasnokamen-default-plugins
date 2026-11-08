# Chisel

Optional datapack-style addon for ordinary Loader clients. Right-click a
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

The client bundle (item definition, model and texture) is optional:
`required = false` in `plugin.toml`. Plain vanilla Java clients may join;
they do not download the Loader bundle and do not get its custom presentation.
Native Solaris and Fabric/NeoForge/Forge Solaris Loader clients can negotiate
the bundle. The core harness checks plain Java admission and chat with
`chisel --client java --windowed`; `chisel --client native --windowed` checks
the block-tool conversions, wear and slab grants.

Install the `chisel` package in `[plugins].directory` — it is opt-in and not
part of the five-package standard pack. Another installed package with a
required client bundle can still require Solaris Loader for the server.

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
