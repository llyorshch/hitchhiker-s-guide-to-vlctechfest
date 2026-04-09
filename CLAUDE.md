# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About

Informal presentation for VLCTechFest — a local tech conference in Valencia, Spain. The presentation is written in Markdown and rendered with [presenterm](https://github.com/mfontanini/presenterm).

## Running the presentation

```bash
presenterm slides.md
```

Hot reload is active by default during editing (auto-jumps to modified slide). Use `--present` / `-p` to disable it for actual presenting.

## File structure

- `slides.md` — the presentation source
- `images/` — images embedded in slides (paths are relative to the presentation file)

---

## presenterm presentation format

### Front matter

```yaml
---
title: "My **presentation**"
sub_title: optional subtitle
author: My Name
# or: authors: [Me, You]
theme:
  name: dark          # built-in theme name
  # path: ./my.yaml   # or a custom theme file
  # override: ...     # or inline overrides
options:
  implicit_slide_ends: true   # setext headings end the previous slide automatically
  end_slide_shorthand: true   # use --- as slide separator
  incremental_lists: true     # all lists pause between items globally
---
```

### Slide structure

Slides are separated by:
```html
<!-- end_slide -->
```

A **setext heading** (underlined with `===`) becomes a styled slide title:
```markdown
My Slide Title
==============
```

Regular `#` headings are normal markdown headings styled per theme (h1–h6).

### Comment commands

These HTML comments control layout and behaviour:

| Command | Effect |
|---|---|
| `<!-- end_slide -->` | End current slide |
| `<!-- pause -->` | Pause for incremental reveal |
| `<!-- jump_to_middle -->` | Jump to vertical center of slide |
| `<!-- alignment: center -->` | Text alignment for rest of slide (`left`/`center`/`right`) |
| `<!-- font_size: 2 -->` | Font size 1–7 (kitty terminal only) |
| `<!-- new_line -->` | Explicit blank line |
| `<!-- new_lines: N -->` | N explicit blank lines |
| `<!-- incremental_lists: true -->` | Incremental list items until end of slide |
| `<!-- list_item_newlines: 2 -->` | Spacing between list items for rest of slide |
| `<!-- no_footer -->` | Hide footer on this slide |
| `<!-- skip_slide -->` | Exclude slide from presentation |
| `<!-- include: file.md -->` | Inline another markdown file |
| `<!-- speaker_note: text -->` | Speaker note (only shown in listener instance) |
| `<!-- // comment -->` or `<!-- comment: text -->` | User comment, ignored during rendering |

### Column layouts

```markdown
<!-- column_layout: [2, 1] -->

<!-- column: 0 -->
Content for left column (2/3 width)

<!-- column: 1 -->
Content for right column (1/3 width)

<!-- reset_layout -->
Full-width content below columns
```

Numbers in `column_layout` are proportional units (e.g. `[1, 3, 1]` for centered content).

### Colored text

```markdown
<span style="color: #ff0000; background-color: palette:foo">red text</span>
<span class="my_class">themed text</span>
```

---

## Code blocks

### Syntax highlighting

````markdown
```rust +line_numbers
fn main() { println!("hello"); }
```
````

Selective highlighting: `` ```rust {1,3,5-7} ``

Dynamic highlighting (advances with keypresses): `` ```rust {1,3|5-7} ``

### Code execution

Requires `-x` flag or `snippet.exec.enable: true` in config.

````markdown
```bash +exec
echo hello world
```
````

Other execution modifiers:
- `+exec_replace` — auto-executes and replaces the block with output (needs `-X`)
- `+auto_exec` — auto-executes like `+exec` without `ctrl+e`
- `+pty` — run inside a pseudo-terminal (for `top`, `htop`, etc.)
- `+validate` — validates during dev without making it executable
- `+image` — renders execution output as an image

### Hiding lines from display (but still executed)

Prefix with `# ` in Rust, `/// ` in Python/bash/go/etc.:
```rust
# fn main() {        // hidden
println!("shown");
# }                  // hidden
```

### Including external files

````markdown
```file +line_numbers
path: snippet.rs
language: rust
start_line: 5   # optional
end_line: 10    # optional
```
````

### Rendered diagrams

````markdown
```mermaid +render
graph LR; A --> B
```
````

Supports `mermaid`, `latex`, `d2` via `+render`. Use `options.auto_render_languages` to skip `+render` globally.

---

## Images

```markdown
![](images/photo.png)
![image:width:50%](images/photo.png)
```

- Paths are relative to the presentation file.
- Requires a terminal supporting iterm2, kitty, or sixel protocols (kitty, ghostty, WezTerm, foot, iterm2).
- Remote images are not supported.
- tmux requires `allow-passthrough` enabled.

---

## Themes

Built-in themes: `dark`, `light`, `gruvbox-dark`, `terminal-dark`, `terminal-light`, `catppuccin-latte/frappe/macchiato/mocha`, `tokyonight-moon/day/night/storm`.

```bash
presenterm --theme dark slides.md
presenterm --list-themes   # preview all themes
```

Custom themes go in `~/.config/presenterm/themes/*.yaml`. A theme can `extends: dark` to inherit and override.

Key theme YAML structure:
```yaml
extends: dark
default:
  margin:
    percent: 8
  colors:
    foreground: "e6e6e6"
    background: "040312"
slide_title:
  separator: true
  font_size: 2
footer:
  style: template
  left: "{author}"
  center: "_@handle_"
  right: "{current_slide} / {total_slides}"
```

---

## Navigation keybindings

| Key | Action |
|---|---|
| `→` / `l` / `j` / Space | Next |
| `←` / `h` / `k` | Previous |
| `n` / `p` | Next/previous fast (skips pauses) |
| `gg` / `G` | First / last slide |
| `<number>G` | Jump to slide N |
| `ctrl+p` | Slide index modal |
| `?` | Key bindings modal |
| `ctrl+e` | Execute code block |
| `ctrl+r` | Reload presentation |
| `q` / `ctrl+c` | Exit |

---

## Speaker notes

```bash
presenterm slides.md --publish-speaker-notes   # presenter screen
presenterm slides.md --listen-speaker-notes    # notes screen
```

Define notes in slides:
```markdown
<!-- speaker_note: Remember to mention the demo here -->
```

---

## Exporting

```bash
presenterm --export-pdf slides.md    # requires weasyprint
presenterm --export-html slides.md   # no extra dependencies
```
