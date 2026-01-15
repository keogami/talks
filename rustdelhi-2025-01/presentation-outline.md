# Title

Oxydizing our beloved metro
by Tushar Saxena \[keogami\]

- introduce myself
- pivot to dmrc
- show the state of dmrc app

# Issue

A jumble of screenshots to show:

- bland ui, political branding
- no fuzzy search
- funky map
- no offline support
- no dark theme

# Missing features

- location-to-location planning
- crowd indicator
- multi-point planning

# Challenge

- Govt website, very hard to lobby
- a lot of complex work needed to lay out the groundwork
- pretty hard for a single person

# Plan

- Create a rust crate for dmrc
- Solve routing
- Make it fast
- Make it cross platform, including the web

# Goals

- Pre compute all the journeys for the entire network
- Embed all the data needed into the binary itself
  - Connect to location-to-location planning
  - Runtime cost
- Make it compatible with wasm32-* targets
- Add bells and whistles like fuzzy search

# Raptor

- Short introduction to the algo (raptor-rs)
- Decide on data structure
- Run it for every combination
- Hit the 25min wall
- Show a solution with os threads, introduce lock contention
- Step back, tease `.par_iter()` with 3min runtime

# Introduce Rayon

- Explain the crate
- Explain how it avoids lock contention

# Embedding

- Introduce build.rs and `include_bytes!` macro and the elf data section
- Think about serialization and program startup implications
- What if we could completely avoid deserialization (diss microsoft for .doc memory dumping)

# Introduce rkyv

- Explain the crate
- Explain how it achieves zero-parsing and zero-copy "deserialization"
- Tease the blog entry for the construction
- Show how much space our structure consumes in the memory

# Introduce dmrc-rs

- Run an example program
- Bring back wasm goal
- Emphasize that its just memory access
- Flex rkyv's cross-platform compatibility
- Run the same example with wasmer

# Conclusion and future

- Proof of concept works
- But the API is not ergonomic
- Will be open for contributions soon
- Link to linkedin and blog
