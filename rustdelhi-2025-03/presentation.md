---
theme:
  name: catppuccin-latte
---

<!-- font_size: 3 -->
<!-- alignment: center -->

<!-- jump_to_middle -->

# Popping Boba: Bringing Go's bubbletea to ratatui

<!-- font_size: 2 -->

<!-- column_layout: [2, 4, 1] -->

<!-- column: 1 -->

<!-- alignment: left -->

Tushar Saxena [keogami]
https://github.com/keogami
https://linkedin.com/in/keogami--

<!-- reset_layout -->

<!-- end_slide -->

<!-- jump_to_middle -->

Have you seen these?
===

<!-- end_slide -->

Claude Code
===

<!-- speaker_note: AI-powered terminal assistant - a TUI that took the dev world by storm -->

<!-- font_size: 2 -->

![](claude-code.png)

<!-- end_slide -->

Charm's Crush
===

<!-- speaker_note: Beautiful TUI from the Charm ecosystem - Go's bubbletea in action -->

<!-- font_size: 2 -->

![](charm-crush.png)

<!-- end_slide -->

Vim / Neovim
===

<!-- speaker_note: The OGs - editors that live in your terminal -->

<!-- font_size: 2 -->

![](vim-neovim.png)

<!-- end_slide -->

Helix
===

<!-- speaker_note: The modern take - written in Rust, btw -->

<!-- font_size: 2 -->

![](helix.png)

<!-- end_slide -->

Bacon
===

<!-- speaker_note: Rust's own background code checker - TUI for cargo diagnostics -->

<!-- font_size: 2 -->

![](bacon.png)

<!-- end_slide -->

ani-cli
===

<!-- speaker_note: Yes, someone made an anime player for the terminal. We live in a wonderful timeline -->

<!-- font_size: 2 -->

![](ani-cli.png)

<!-- end_slide -->

TUIs are _everywhere_
===

<!-- speaker_note: There are way too many to list - here's a taste -->

<!-- font_size: 2 -->

![](tui-scatter.png)

<!-- end_slide -->

<!-- jump_to_middle -->

How deep does the rabbit hole go?
===

<!-- end_slide -->

The TUI Iceberg
===

<!-- speaker_note: We're going to peel layers of abstraction, like going down an iceberg -->

<!-- font_size: 2 -->

![](iceberg-meme.png)

<!-- end_slide -->

<!-- jump_to_middle -->

<!-- font_size: 3 -->

Tip of the Iceberg: TUI Frameworks
===

<!-- end_slide -->

TUI Frameworks
===

<!-- speaker_note: tuirealm and friends - the "React" of terminal UIs -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

Crates like **tuirealm** try to give you the full framework experience:

<!-- pause -->

- Event handling, propagation, bubbling
- High-level components out of the box
- No manual bookkeeping for inputs, scrolling, etc

<!-- pause -->

Sounds great, right?

<!-- reset_layout -->

<!-- end_slide -->

TUI Frameworks: The Catch
===

<!-- speaker_note: They're opinionated and immature - not quite production ready -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

- _Very_ opinionated
- Also not very well developed, frankly
- Limited ecosystem and documentation
- You're locked into their way of doing things

<!-- pause -->

Let's go deeper...

<!-- reset_layout -->

<!-- end_slide -->

<!-- jump_to_middle -->

<!-- font_size: 3 -->

Going Deeper: Ratatui
===

<!-- end_slide -->

Ratatui
===

<!-- speaker_note: The most popular TUI crate - it's what most Rust TUIs are built on -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

The **most popular** TUI crate in Rust

- Agnostic over backends (we will get there)
- Provides you with layout mechanisms
- Widgets, buffers, and a rendering pipeline

<!-- pause -->

But there's a catch...

<!-- reset_layout -->

<!-- end_slide -->

Ratatui: You Do The Work
===

<!-- speaker_note: Ratatui gives you building blocks but you wire everything yourself -->

<!-- font_size: 2 -->

_You_ need to do **all** the bookkeeping

```rust +line_numbers
fn ui(f: &mut Frame, app: &App) {
    let chunks = Layout::default()
        .direction(Direction::Vertical)
        .constraints([
            Constraint::Length(3),
            Constraint::Min(1),
        ])
        .split(f.area());

    let input = Paragraph::new(app.input.as_str())
        .block(Block::default().borders(Borders::ALL).title("Input"));
    f.render_widget(input, chunks[0]);

    // you manage state, focus, events... everything
}
```

<!-- end_slide -->

<!-- jump_to_middle -->

<!-- font_size: 3 -->

Going Deeper: Terminal Wrappers
===

<!-- end_slide -->

Terminal Wrappers
===

<!-- speaker_note: Crossterm and Termion abstract over different terminal implementations -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

**Crossterm**, **Termion** — the backends ratatui sits on

Why do they exist?

<!-- pause -->

"Cross-browser shenanigans all over again"

<!-- pause -->

- Different terminals behave differently
- These crates provide a simple abstraction over terminal primitives:
  - Cursor movement
  - Text styling (bold, colors, etc)
  - Alternate screen buffer

<!-- reset_layout -->

<!-- end_slide -->

Terminal Wrappers: The Limitations
===

<!-- speaker_note: They're write-only and miss advanced terminal features -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

- Still no OSC queries — it's **write-only**
  - Can't ask the terminal "what size are you _really_?"
  - Can't detect terminal capabilities

<!-- pause -->

- No special OSC commands either
  - No inline images? **bummer**
  - No clipboard access
  - No hyperlinks

<!-- pause -->

Let's go even deeper...

<!-- reset_layout -->

<!-- end_slide -->

<!-- jump_to_middle -->

<!-- font_size: 3 -->

Rock Bottom: std::fmt::write!()
===

<!-- end_slide -->

Good ol' write!()
===

<!-- speaker_note: It comes with Rust - no crates needed, just raw escape sequences -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

Comes with Rust. No crates needed.

The terminal is just a stream of bytes. You already know some escape sequences:

```rust
print!("Hello\n");   // newline
print!("Col1\tCol2"); // tab
print!("\x07");        // bell (yes, your terminal can beep)
```

<!-- pause -->

But there's a whole world beyond `\n` and `\t`...

<!-- reset_layout -->

<!-- end_slide -->

Raw Mode
===

<!-- speaker_note: Normally the terminal buffers and processes input - raw mode gives you full control -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

Normally your terminal is in **cooked mode**:

- Input is line-buffered (waits for Enter)
- Special keys are interpreted (Ctrl+C = SIGINT)
- Echo is on (you see what you type)

<!-- pause -->

**Raw mode** turns all of that off:

- Every keypress is delivered immediately
- No signal generation
- No echo
- _You_ are in full control

<!-- pause -->

This is how TUIs actually work under the hood.

<!-- reset_layout -->

<!-- end_slide -->

Advanced Escape Sequences
===

<!-- speaker_note: ANSI escape codes are the real magic - CSI sequences control everything -->

<!-- font_size: 2 -->

```rust +line_numbers
use std::io::Write;

fn main() {
    let mut out = std::io::stdout();

    // Clear the entire screen
    write!(out, "\x1B[2J").unwrap();

    // Move cursor to row 5, column 10
    write!(out, "\x1B[5;10H").unwrap();

    // Bold + Red text
    write!(out, "\x1B[1;31mHello, Terminal!\x1B[0m").unwrap();

    // Enable Kitty keyboard protocol
    write!(out, "\x1B[>1u").unwrap();

    out.flush().unwrap();
}
```

<!-- pause -->

This is what **every** TUI library does under the hood.

<!-- end_slide -->

<!-- jump_to_middle -->

<!-- font_size: 3 -->

So... Why Are We Here?
===

<!-- end_slide -->

PoppingBoba
===

<!-- speaker_note: Introduce PoppingBoba as a Rust implementation of bubbletea's architecture on top of ratatui -->

<!-- font_size: 2 -->

<!-- column_layout: [1, 5, 1] -->

<!-- column: 1 -->

<!-- alignment: center -->

A Rust implementation of Go's **bubbletea** — for **ratatui**

![](bubbletea-example.gif)

<!-- pause -->

The Elm Architecture, in your terminal, in Rust.

<!-- reset_layout -->

<!-- end_slide -->

<!-- jump_to_middle -->

Fin.
===

<!-- end_slide -->

<!-- font_size: 2 -->

Mentorship
===

<!-- speaker_note: Shill the mentorship program -->

<!-- new_lines: 3 -->

<!-- column_layout: [1, 3, 1] -->

<!-- column: 1 -->

<!-- alignment: center -->

Interested in learning Rust with guidance?

<!-- font_size: 3 -->

**https://mentorship.keogami.dev**

<!-- font_size: 2 -->

<!-- new_lines: 3 -->

**Tushar Saxena [keogami]**

https://github.com/keogami

https://linkedin.com/in/keogami--

<!-- reset_layout -->
