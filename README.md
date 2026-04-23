# k9s — The Field Guide

A direct, practical cheatsheet for the [k9s](https://k9scli.io) Kubernetes CLI. Organised for fast lookup, not for reading front-to-back.

**Live version →** [https://YOUR-USERNAME.github.io/k9s-cheatsheet](#-deploy-to-github-pages) (after you deploy it; see below)

---

## Table of contents

1. [Launch & flags](#1-launch--flags)
2. [Universal navigation](#2-universal-navigation)
3. [Jump to resources](#3-jump-to-resources)
4. [Resource actions](#4-resource-actions)
5. [Logs viewer](#5-logs-viewer)
6. [Filter & sort](#6-filter--sort)
7. [Namespaces & contexts](#7-namespaces--contexts)
8. [Port forwarding](#8-port-forwarding)
9. [Cluster tools](#9-cluster-tools)
10. [Config & paths](#10-config--paths)
11. [Everyday workflows](#11-everyday-workflows)
12. [Gotchas & tips](#12-gotchas--tips)
13. [Deploy to GitHub Pages](#-deploy-to-github-pages)

---

## 1. Launch & flags

| Command | What it does |
|---|---|
| `k9s` | Launch using current kubeconfig context. |
| `k9s -n <ns>` | Start in a specific namespace. |
| `k9s -c <ctx>` | Start with a specific context. |
| `k9s --readonly` | Read-only mode — no edits, deletes, or exec. **Use in prod.** |
| `k9s -A` | All namespaces at launch. |
| `k9s --headless` | Hide top header. |
| `k9s --logoless` | Hide the k9s logo. |
| `k9s --crumbsless` | Hide the breadcrumb bar. |
| `k9s -r 3` | Set refresh rate to 3s (default 2). |
| `k9s --kubeconfig <path>` | Use a specific kubeconfig file. |
| `k9s info` | Print version + all config paths. |

---

## 2. Universal navigation

These keys work almost everywhere.

| Key | Action |
|---|---|
| `:` | Enter **command mode**. Type a resource alias, then `Enter`. |
| `/` | Enter **filter mode** on the current view. |
| `?` | Show the help panel for this view. |
| `Ctrl+a` | Show **all resource aliases**. |
| `Esc` | Close menu / cancel filter / go back. |
| `Enter` | Drill into the selected row. |
| `Tab` | Cycle between panes. |
| `↑ ↓` or `j k` | Move selection (vim-style works). |
| `g` / `G` | Jump to top / bottom. |
| `:q`, `:quit`, `Ctrl+c` | Quit k9s. |

---

## 3. Jump to resources

Type `:` then the alias, then `Enter`.

| Alias | Resource |
|---|---|
| `:pod` · `:po` | Pods |
| `:deploy` · `:dp` | Deployments |
| `:svc` | Services |
| `:ing` | Ingresses |
| `:ns` | Namespaces |
| `:node` · `:no` | Nodes |
| `:cm` | ConfigMaps |
| `:sec` | Secrets (press `x` to decode a value) |
| `:pvc` · `:pv` | Persistent volume claims / volumes |
| `:sts` · `:ds` · `:rs` | StatefulSets / DaemonSets / ReplicaSets |
| `:job` · `:cj` | Jobs / CronJobs |
| `:sa` · `:role` · `:rb` · `:crb` | RBAC resources |
| `:events` · `:ev` | Events — watch this when things break |
| `:ctx` | Switch kubeconfig context |
| `:helm` | Helm releases (Enter for revision history) |
| `:pf` | Active port-forwards |
| `:screendump` · `:sd` | Dump cluster state (YAML) to disk |
| `:alias` | Full alias list |

> **Tip:** you can jump straight to a resource — `:pod my-pod-abc123` or narrow by namespace: `:pod production`.

---

## 4. Resource actions

With a row highlighted, press…

| Key | Action |
|---|---|
| `d` | **Describe** — full kubectl-describe output. |
| `y` | View **YAML** from the API. |
| `e` | **Edit** in `$EDITOR`; save to apply. |
| `l` | **Logs** for the selected pod. |
| `p` | **Previous** container logs (last crash). |
| `s` | **Shell** into a pod. |
| `a` | **Attach** to a running container. |
| `f` | Show port-forwards for this resource. |
| `Shift+f` | **Create** a port-forward. |
| `r` | On a deployment: restart rollout. |
| `x` | On secrets: decode value. Elsewhere: context-specific. |
| `t` | On deployments: scale (transition replicas). |
| `Space` | **Mark** row. Repeat to select multiple. |
| `Ctrl+Space` | Clear all marks. |
| `Ctrl+d` | **Delete** with confirmation. Respects marks — bulk delete. |
| `Ctrl+k` | **Kill** — force-delete without confirmation. ⚠️ |

---

## 5. Logs viewer

Keys inside the log pane.

| Key | Action |
|---|---|
| `0` | Tail **all lines** (follow indefinitely from pod start). |
| `1`–`5` | Tail last 1 / 10 / 100 / 1000 / 5000 lines. |
| `f` | Toggle **full screen**. |
| `w` | Toggle line **wrap**. |
| `t` | Toggle **timestamps**. |
| `s` | **Save** buffer to disk. |
| `c` | **Copy** buffer to clipboard. |
| `m` | **Mark** current position. |
| `n` / `Shift+n` | Next / previous mark. |
| `Shift+c` | Clear buffer (display only). |
| `Ctrl+s` | Toggle autoscroll. |
| `/` | Filter log lines (regex supported). |

---

## 6. Filter & sort

| Key | Action |
|---|---|
| `/text` | Filter rows containing "text". |
| `/!text` | **Inverse** filter. |
| `/-l app=web` | Label selector. |
| `/-f status.phase=Running` | Field selector. |
| `/regex` | Regex filter (e.g. `/api-.*-prod`). |
| `Shift+n` | Sort by name. |
| `Shift+a` | Sort by age. |
| `Shift+s` | Sort by status. |
| `Shift+r` | Sort by ready. |
| `Shift+c` | Sort by CPU (metrics-server required). |
| `Shift+m` | Sort by memory. |
| `Shift+i` | Sort by IP. |
| `Shift+o` | Sort pods by node — useful for noisy-neighbour hunting. |
| `0` | Show **all namespaces** in a resource list. |
| `1`–`9` | Jump to favourited namespace. |

---

## 7. Namespaces & contexts

| Key | Action |
|---|---|
| `:ns` | Namespace list. `Enter` to switch. |
| `Ctrl+s` | In namespace view: **favourite** the selected namespace. |
| `u` | In namespace view: **unfavourite**. |
| `:ctx` | Context list. `Enter` to switch. |
| `0` | View **all namespaces** (works in any resource list). |

---

## 8. Port forwarding

| Key | Action |
|---|---|
| `Shift+f` | Create a port-forward on the selected pod or service. |
| `:pf` | List **active** port-forwards. |
| `Ctrl+b` | On a forward: open the URL in your browser. |
| `Ctrl+d` | Delete / stop the selected forward. |

> Port-forwards die with the k9s process. Same lifetime as `kubectl port-forward`.

---

## 9. Cluster tools

| Command | What it does |
|---|---|
| `:pulse` | Cluster **health dashboard**. |
| `:popeye` | Run **Popeye** — cluster sanitizer that flags misconfigs. |
| `:xray <resource>` | **Dependency tree** for a resource (e.g. `:xray deployment`). |
| `:dir <path>` | Browse and apply YAML manifests from a local directory. |
| `:benchmark` · `:be` | Run / inspect benchmarks. |
| `:plugins` | List configured plugins. |
| `:skins` | List available skins. |

---

## 10. Config & paths

Run `k9s info` first — it prints the exact paths on your system.

| File | Purpose |
|---|---|
| `config.yaml` | Global settings: refresh rate, default ns, UI toggles. |
| `aliases.yaml` | Custom resource aliases. |
| `hotkeys.yaml` | Global key bindings. |
| `plugins.yaml` | Per-view commands that shell out to scripts. |
| `skins/*.yaml` | Per-cluster colour themes. |

| OS | Config directory |
|---|---|
| Linux | `~/.config/k9s` |
| macOS | `~/Library/Application Support/k9s` |
| Windows | `%LOCALAPPDATA%\k9s` |

> **Prod-safe skins:** drop a `prod.yaml` skin with red/alarming colours in `skins/`, then reference it by context in `config.yaml`. You'll never confuse prod and dev again.

---

## 11. Everyday workflows

### Tail logs from a specific pod
```
:pod <Enter>         # open pod list
/api-server <Enter>  # filter
l                    # open logs
0                    # tail indefinitely
/error <Enter>       # filter to errors
```

### Exec into a pod
```
:pod <Enter>
↑/↓ to highlight
s                    # shell; pick container if prompted
exit                 # or Ctrl+d to leave
```

### Port-forward a service
```
:svc <Enter>
↑/↓ to highlight
Shift+f              # prompts for ports
8080:80 <Enter>
:pf                  # confirm it's live
```

### Edit a ConfigMap and roll the deployment
```
:cm <Enter>          # select ConfigMap
e                    # edit in $EDITOR, save
:deploy <Enter>      # select consuming deployment
r                    # restart rollout
```

### Find what's failing right now
```
:ev <Enter>          # events
Shift+a              # sort by age (newest first)
/Warning <Enter>     # filter
:pulse               # correlate with cluster health
```

### Decode a secret
```
:sec <Enter>         # select secret
<Enter>              # open keys
x                    # decode the highlighted value in-place
```

### Bulk-delete stuck pods
```
:pod <Enter>
/my-job <Enter>      # filter
Space on each row    # mark
Ctrl+d               # delete with confirm (or Ctrl+k to force)
```

---

## 12. Gotchas & tips

- **`Ctrl+k` vs `Ctrl+d`** — `Ctrl+k` force-kills *without confirmation*. `Ctrl+d` asks first. Muscle memory matters.
- **Always `--readonly` in prod** — a single flag prevents a career-ending keystroke.
- **`0` is context-sensitive** — "all namespaces" in lists, "tail all" in log views.
- **`Space` marks are sticky** — they survive filter changes. If bulk-delete is picking up unexpected rows, `Ctrl+Space` clears marks.
- **CPU / memory columns need metrics-server** — otherwise you'll see dashes.
- **`e` uses `$EDITOR`** — set it: `export EDITOR=nvim`. Default may drop you in `vi`.
- **`:xray` empty trees** — usually an RBAC gap on the referenced resources, not a bug.
- **`:sd` (screen dump)** — writes YAML per resource to disk. Great for post-mortems.
- **Helm rollback** — in `:helm`, `Enter` shows revisions, select one, press `u`.
- **Favourites are global** — favourite a namespace with `Ctrl+s`, then digits `1`–`9` switch instantly from anywhere.

---

## 🚀 Deploy to GitHub Pages

The `index.html` in this repo is a self-contained, search-enabled version of this cheatsheet with nicer typography. Here's how to publish it.

### Option A — Personal / project site (recommended)

1. Create a new public GitHub repo, e.g. `k9s-cheatsheet`.
2. Drop `index.html` and `README.md` into it:
   ```bash
   git init
   git add index.html README.md
   git commit -m "k9s cheatsheet"
   git branch -M main
   git remote add origin https://github.com/<your-username>/k9s-cheatsheet.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages**.
4. Under **Build and deployment**:
   - Source: `Deploy from a branch`
   - Branch: `main`, folder: `/ (root)`
   - Click **Save**.
5. Wait ~30 seconds. Your site will be live at:
   ```
   https://<your-username>.github.io/k9s-cheatsheet/
   ```

### Option B — User site (no repo name in URL)

Name the repo exactly `<your-username>.github.io`. After pushing, the site is live at `https://<your-username>.github.io/`.

### Custom domain (optional)

1. Add a file called `CNAME` containing your domain (e.g. `k9s.example.com`).
2. In your DNS, point a `CNAME` record to `<your-username>.github.io`.
3. In **Settings → Pages → Custom domain**, enter the domain and tick **Enforce HTTPS**.

---

## License & credits

This cheatsheet summarises public documentation from the [k9s project](https://github.com/derailed/k9s) by Fernand Galiana. k9s is released under the Apache 2.0 license.

Feel free to fork, customise colours in `index.html` (`:root` CSS variables), and share with your team.
