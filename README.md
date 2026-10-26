# dotfiles

Personal Linux (GNOME) setup. After a fresh OS install, follow the steps below.

## 1. Clone

```bash
git clone git@github.com:<your-user>/dotfiles.git ~/dotfiles
cd ~/dotfiles
```

## 2. Compact GNOME title bar (GTK4 / libadwaita)

Shrinks headerbars, the Files search field and the Ghostty new-tab button.

```bash
mkdir -p ~/.config/gtk-4.0
ln -sf ~/dotfiles/gtk-4.0/gtk.css ~/.config/gtk-4.0/gtk.css
```

Restart the apps (or log out and back in).

Notes:
- If `~/.config/gtk-4.0/gtk.css` already exists, back it up first or merge. Later rules win.
- Flatpak apps need access to the config dir:
  ```bash
  flatpak override --user --filesystem=xdg-config/gtk-4.0:ro
  ```
- To tune sizes, edit the `2px` paddings and the `28px`/`24px` tab heights in `gtk-4.0/gtk.css`.
- To find the right selector for a stubborn widget: `GTK_DEBUG=interactive ghostty` (or any app).

## 3. Gruvbox theme (Gruvbox-Green-Dark)

The theme folder is installed in `~/.themes`, and I edited
`gnome-shell/gnome-shell.css` inside it. The repo keeps the full theme
folder so the edit comes back after a reinstall.

```bash
mkdir -p ~/.themes
cp -r ~/dotfiles/themes/Gruvbox-Green-Dark ~/.themes/
```

Then enable it:

- GNOME Tweaks > Appearance > Shell (needs the **User Themes** extension enabled)
- Also set Legacy Applications to Gruvbox-Green-Dark if you use it for GTK3 apps

If the original theme is ever updated or re-downloaded, it overwrites the
edit, so re-copy from this repo afterwards.

## 4. ArcMenu and Forge (GNOME extensions)

Install both extensions first (Extension Manager or extensions.gnome.org), then:

```bash
dconf load /org/gnome/shell/extensions/arcmenu/ < ~/dotfiles/gnome/arcmenu.dconf
dconf load /org/gnome/shell/extensions/forge/ < ~/dotfiles/gnome/forge.dconf
```

Forge custom CSS (if edited):

```bash
mkdir -p ~/.config/forge/stylesheet/forge
cp ~/dotfiles/gnome/forge-stylesheet.css ~/.config/forge/stylesheet/forge/stylesheet.css
```

Log out and back in afterwards.

## Updating the repo

```bash
cp -r ~/.themes/Gruvbox-Green-Dark ~/dotfiles/themes/
dconf dump /org/gnome/shell/extensions/arcmenu/ > ~/dotfiles/gnome/arcmenu.dconf
dconf dump /org/gnome/shell/extensions/forge/ > ~/dotfiles/gnome/forge.dconf
cd ~/dotfiles && git add -A && git commit -m "Update dotfiles" && git push
```
