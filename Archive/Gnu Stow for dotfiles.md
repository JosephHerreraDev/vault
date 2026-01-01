1. Create folder to move `dotfiles`

```bash
mkdir -p ~/dotfiles/nvim/.config
cd ~/dotfiles
```

2. Move the config file/directory to the folder

```bash
mv ~/.config/nvim nvim/.config
```

3. To return files to corresponding directory

```bash
stow nvim
```
