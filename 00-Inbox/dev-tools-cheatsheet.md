# 🛠️ Dev Tools Cheat Sheet

> Activate everything: `exec zsh` (or open a new terminal). Set font to **JetBrainsMono Nerd Font**.

---

## ⌨️ The keybindings you'll use constantly
| Key | Does |
|---|---|
| `Ctrl-R` | **atuin** — full-screen fuzzy search of your shell history |
| `Ctrl-T` | **fzf** — fuzzy-pick file(s), pastes path onto the command line |
| `Alt-C` | **fzf** — fuzzy-pick a directory and `cd` into it |
| `<cmd> **<Tab>` | **fzf** completion, e.g. `vim **<Tab>`, `ssh **<Tab>`, `kill -9 **<Tab>` |
| `Tab` (in fzf) | multi-select (pick several files at once) |

---

## 📂 Navigation & files

### zoxide — smarter cd
It silently learns every dir you `cd` into. Then:
```bash
z brain          # jump to ~/Documents/SecondBrain (best match)
z desk note      # match multiple keywords in the path
zi               # interactive picker (fuzzy) of your visited dirs
```

### eza — better ls (aliased)
```bash
ls               # icons + dirs first
ll               # long view: perms, size, git status, icons
la               # include hidden files
lt               # tree, 2 levels deep
eza -l --sort=modified --reverse    # newest last
```

### bat — better cat (aliased to plain mode)
```bash
cat file.js                  # plain (your alias), no pager
bat file.js                  # FULL: line numbers + syntax + git gutter
bat -r 20:40 file.js         # only lines 20-40
bat --list-themes            # browse color themes
```

### fd — better find
```bash
fd report                    # find files/dirs matching "report"
fd -e md                     # all .md files
fd -e md notes/              # ...under notes/
fd -H secret                 # include hidden files
fd -t d src                  # directories only
fd -e log -x rm              # find .log files and delete each
```

---

## 🌳 Git

### lazygit — visual git (alias: `lg`)
Run `lg` inside any repo. Inside it:
| Key | Action |
|---|---|
| `?` | **help — shows every key (start here!)** |
| `␣` (space) | stage / unstage the selected file or hunk |
| `a` | stage all |
| `c` | commit (write message) |
| `P` | push · `p` pull |
| `Tab` / arrows | move between panels |
| `enter` | drill into a file's hunks |
| `q` | quit |

### delta — automatic pretty diffs
Just use git normally — output is now side-by-side + syntax-highlighted:
```bash
git diff
git show HEAD
git log -p
```
Inside the diff pager: `n` / `N` jump between changed files.

### gh-dash — PR & issue dashboard
```bash
gh dash          # TUI of your PRs/issues. Tab=switch sections, ?=help
```

---

## 🔧 Utilities

### tldr — practical cheat sheets (instead of man)
```bash
tldr tar         # the 5 tar commands you actually use
tldr git rebase
```

### jq / yq — JSON & YAML surgery
```bash
echo '{"name":"shiv","age":25}' | jq '.name'
cat data.json | jq '.items[] | select(.active) | .id'
jq -r '.[].email' users.json        # -r = raw (no quotes)

yq '.services.web.image' docker-compose.yml
yq -o=json config.yaml               # convert YAML → JSON
yq -i '.version = "2.0"' config.yaml # edit in place
```

### xh — friendly HTTP client (better curl)
```bash
xh httpbin.org/get                       # GET
xh POST httpbin.org/post name=shiv age:=25   # = string, := raw JSON/number
xh GET api.site.com/me Authorization:"Bearer TOKEN"
xh -d example.com/file.zip               # download
```

### glow — render markdown in the terminal
```bash
glow README.md
glow -p notes.md          # in a pager
glow .                    # browse all markdown in the folder
```

### btop — system monitor (aliased: `top`)
```bash
top              # or: btop.  Mouse works. m=menu, q=quit, esc=back
```

### dust — visual disk usage
```bash
dust             # what's eating space here
dust ~/Downloads
dust -d 1        # only 1 level deep
```

### hyperfine — benchmark commands
```bash
hyperfine 'npm run build'
hyperfine 'sort file.txt' 'sort -S 1G file.txt'   # compare two, with stats
hyperfine --warmup 3 'your-command'
```

### gum — pretty interactive prompts for shell scripts
```bash
gum choose apple banana cherry        # arrow-key menu, prints choice
NAME=$(gum input --placeholder "your name")
gum confirm "Deploy to prod?" && echo "deploying..."
gum spin --title "Working..." -- sleep 3
```

---

## 🚀 Bigger tools

### mise — unified version manager (NOT yet activated)
You still use nvm/pyenv/rbenv. To try mise in a project without committing fully:
```bash
mise use node@22         # pins node 22 for this dir (.mise.toml)
mise use python@3.12
mise install             # install everything a project pins
```
To make it your default, uncomment the `mise activate` line in `~/.zshrc` and remove the nvm/pyenv/rbenv lines.

### OrbStack — faster Docker (installed, Docker Desktop untouched)
Open the **OrbStack** app, quit Docker Desktop — your `docker` commands now run on OrbStack's faster engine. Bonus:
```bash
orb create ubuntu        # spin up a lightweight Linux VM
orb                      # open a shell in it
```

### Ghostty — fast terminal (pre-configured)
Open the **Ghostty** app. `Cmd-T` new tab. Config lives at `~/.config/ghostty/config` — edit and reload with `Cmd-Shift-,`.
