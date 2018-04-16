# Ansidot

A simple Ansible playbook to deploy and manage your dotfiles.

## Requirements

* Ansible >= 2.4
* Git >= 1.7.1

## Usage

```shell
ansible-playbook ansidot.yml \
  --inventory localhost,     \
  --connection local         \
  --extra-vars @apps.yml # var_file containing the `apps` variable
```

## Configuration example

```yaml
---
apps:
  - name: zsh
    # Check for required packages
    # If you need root permissions to install them,
    # you can run `ansidot` with the `--tags packages` and `--become` arguments
    packages:
      - zsh
    # A list of git repositories to clone
    # `repo`: the repository URL (required)
    # `dest`: where goes your local copy (required)
    # `version`: the commit hash, tag or branch to check out
    git:
      - repo: https://github.com/robbyrussell/oh-my-zsh.git
        dest: ~/.oh-my-zsh
      - repo: https://github.com/zsh-users/zsh-syntax-highlighting.git
        dest: ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting.git
        version: 0.6.0
    # You might need to ensure some directories exit
    # before linking your dotfiles
    # `path`: the directory to create (required)
    # `mode`: set permissions on the created permissions
    directories:
      - path: ~/.oh-my-zsh/custom/plugins/itscustom
    # Your dotfiles and where to link them
    # `path`: where to link your dotfile (required)
    # `src`: the path to your versioned file (required)
    # `force`: force symlink creation,
    #          useful the destination exists and is a file
    #          but will also force if the source does not exit
    #          and will not warn you in both cases (use carefully)
    dotfiles:
      - path: ~/.zshrc
        src: ~/.dotfiles/zshrc
      - path: ~/.oh-my-zsh/custom/plugins/itscustom/itscustom.plugin.zsh'
        src: ~/.dotfiles/itscustom.plugin.zsh
  - name: vim
    packages:
      - vim
    dotfiles:
      - path: ~/.dotfiles/vimrc
        src: ~/.vim_runtime/vimrcs/basic.vim
```
