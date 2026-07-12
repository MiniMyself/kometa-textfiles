# Kometa Text Files — TV Franchises & Universes

Curated, chronological **episode-level** viewing orders for TV show franchises and shared universes, formatted for use with the [Kometa](https://kometa.wiki) [`text_file` builder](https://kometa.wiki/en/latest/files/builders/textfile/text-file/).

Point a Kometa collection at one of the raw `.txt` URLs in this repo and you get a collection containing every episode of every show in that franchise, in the intended watch order.

---

## Requirements

- A working [Kometa](https://kometa.wiki) installation
- A **Show library** in Plex containing the shows in the franchise
- Nothing else — the files are fetched over HTTPS at runtime, no download or local copy needed

---

## Usage

### 1. Reference the collection file from your `config.yml`

In the library you want these collections to appear in:

```yaml
libraries:
  TV Shows:
    collection_files:
      - file: config/TV-Franchises.yml
```

Adjust the path/filename to match wherever you store your collection files.

### 2. Create the collection file

In this example a shared template is used, that is not required.

`TV-Franchises.yml`:

```yaml
templates:
  TV Collection:
    optional:
      - text_file
    text_file: <<text_file>>
    collection_order: custom
    builder_level: episode
    sync_mode: sync
    summary: "Chronological viewing order of all <<collection_name>> TV shows"
```

What the template does:

| Setting | Why |
|---|---|
| `text_file` | Pulls the episode list straight from this repo |
| `collection_order: custom` | Keeps Plex's sort identical to the order in the text file |
| `builder_level: episode` | Builds a collection of **episodes**, not shows |
| `sync_mode: sync` | Removes anything from the collection that is no longer in the text file |
| `summary` | Auto-generates a description using the collection name |

### 3. Add your collections

In the same file:

```yaml
collections:
  Arrowverse:
    template: {name: TV Collection, text_file: https://raw.githubusercontent.com/MiniMyself/kometa-textfiles/refs/heads/main/Arrowverse.txt}
  FBI:
    template: {name: TV Collection, text_file: https://raw.githubusercontent.com/MiniMyself/kometa-textfiles/refs/heads/main/FBI.txt}
```

Adding another franchise is a one-liner: copy a block, change the collection name and the file name in the URL.

---

## File Format

Each file is a plain text list, one item per line, using [Kometa's text file syntax](https://kometa.wiki/en/latest/files/builders/textfile/text-file/):

```text
# Arrowverse — chronological order
tvdb_episode:279121_1_1   # Arrow S1E1 - Pilot
tvdb_episode:279121_1_2   # Arrow S1E2 - Honor Thy Father
```

- Lines starting with `#` are comments and are ignored
- Trailing `# comments` after a value are also ignored — used here to label each episode
- `tvdb_episode:<tvdb_id>_<season>_<episode>` targets a single episode
- `tvdb_season:<tvdb_id>/<season>` expands to every episode of that season at `builder_level: episode`

Blank lines are fine, and the order of the lines **is** the collection order.

---

## Notes & Caveats

- **Only what you own shows up.** Episodes you don't have in Plex are simply skipped.
- Crossover-heavy franchises (like the Arrowverse) have no single "official" order — these lists reflect a sensible, widely-used chronological/crossover-aware order. If you disagree with a placement, see below.
- Files are fetched live from GitHub each run, so updates here reach your server on your next Kometa run.

---

## Contributing

Corrections, missing episodes, ordering fixes and brand-new franchises are all welcome.

1. Open an issue describing the change (or the new franchise), **or**
2. Fork, edit/add the `.txt` file, and open a pull request

When adding a new franchise, please:

- Name the file after the franchise
- Use TVDb IDs (`tvdb_episode:`)
- Add a trailing comment on each line with the show, season/episode and title, so the list stays reviewable
- Keep one item per line, in viewing order

---

## Credits

- [Kometa](https://github.com/Kometa-Team/Kometa) — the tool that makes this possible
- Ordering references sourced from community-maintained watch orders and franchise wikis — the specific source used for each list is credited in a comment at the top of the corresponding .txt file

## License

Released under the [MIT License](LICENSE). These are lists of identifiers — no media, artwork, or copyrighted content is included.
