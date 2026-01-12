---
title: "Vibe Coding in Production on DigitalOcean"
date: 2025-08-05T08:00:00-08:00
description: ""
images:
 - 2025/08/vibe-coding-in-production-on-digitalocean/Images/hero.png
draft: true
---

![](images/hero.png)

notes

1/ create a DO droplet

2/ ssh into the droplet

#!/bin/bash

  #!/bin/bash
  # Save this as setup-vibe-coding.sh and run with: bash setup-vibe-coding.sh

  set -e

  echo "🚀 Setting up your vibe coding environment..."

  # Update system
  sudo apt update && sudo apt upgrade -y

  # Install essential packages
  sudo apt install -y curl wget git unzip build-essential software-properties-common apt-transport-https ca-certificates gnupg lsb-release

  # Install zsh and oh-my-zsh
  sudo apt install -y zsh
  sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
  chsh -s $(which zsh)

  # Install Claude Code
  curl -fsSL https://claude.ai/install.sh | bash
  echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc

  # Install GitHub CLI
  curl -fsSL https://cli.github.co[48;71;202;2414;2828tm/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
  sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee
  /etc/apt/sources.list.d/github-cli.list > /dev/null
  sudo apt update && sudo apt install -y gh

  # Install development tools
  curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
  sudo apt-get install -y nodejs python3 python3-pip python3-venv

  # Install modern CLI tools
  sudo apt install -y htop neofetch tree jq bat exa fd-find ripgrep tmux neovim

  # Add useful aliases
  cat >> ~/.zshrc << 'EOF'
  alias ll='exa -la'
  alias cat='batcat'
  alias find='fdfind'
  alias grep='rg'
  alias vim='nvim'
  EOF

  echo "✅ Setup complete! Now run:"
  echo "1. claude auth login"
  echo "2. gh auth login"
  echo "3. Copy your SSH key from local machine"
  echo "4. Restart your shell or run: exec zsh"
