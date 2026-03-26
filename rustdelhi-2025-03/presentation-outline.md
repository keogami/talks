# Structure

This presentation is to introduce terminal related functionality to the rust community.

It will go with following flow:
- Show TUIs that are popular with images: claude code, crush by charmbracelet, vim and neovim and helix, rust's bacon, anime player
- Mention lots of tuis that popular as a text scatter image
- Explain to the audience that we will be peeling layers of abstraction like going down an iceberg (show the meme)
- Tip of iceberg: Explain tui frameworks in rust (tuirealm and the other)
  - Get event handling/propagation/bubbling
  - High level components
  - No need for manual book keeping for things like inputs, scrolling, etc
  - _Very_ opinionated
  - Also not very well developed, frankly
- Going deeper: Explain ratatui
  - The most popular tui crate in rust
  - Agnostic over backends (we will get there)
  - Provides you with layout mechanisms
  - But _you_ need to do all the book keeping
- Going deeper: Terminal wrappers
  - Introduce various wrappers: Crossterm, Termion
  - Why they exist? "cross-browser shenanigans all over again"
  - Provide simple abstraction over terminal primitives: cursor, styling, alt screen
  - Still no OSC queries, write-only
  - No special OSC commands either, no images? bummer
- Going deeper: good ol' std::fmt::write!()
  - Comes with rust
  - Explain basics of escape sequences: `\n`, `\t`, `\a`
  - Explain raw mode
  - Explain advanced escape sequences: `\x1B[2J`, `\x1B[?u`
- Finally introduce PoppingBoba as a rust impl of go's bubbletea
- End with shilling mentorship program: https://mentorship.keogami.dev
