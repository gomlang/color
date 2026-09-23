# palette

`ecosystem::color::palette` provides an immutable `Palette` of `Srgb` colors.
`Palette::new(colors)` accepts 1 through 65,536 entries and copies the input.
`colors()` returns a separate copy, while `get(index)` returns `None` outside
the palette. Entries cannot be changed after construction.

`nearest_index(color)` and `nearest(color)` scan the palette in insertion order.
They minimize squared Euclidean distance over premultiplied **linear-light**
red, green and blue channels plus alpha. Exactly equal distances select the
first entry. Fully transparent colors therefore have the same RGB coordinates
for lookup even if their hidden unassociated RGB bytes differ. Queries require
no allocation, take O(n) time, and return an index or stored color directly.
The distance is a deterministic policy rather than a perceptual color-difference
formula.

`web_safe()` constructs the 216 opaque sRGB colors with channels 0, 51, 102,
153, 204 and 255, ordered by red, then green, then blue. It allocates a fresh
palette on each call and exposes no mutable global state.
