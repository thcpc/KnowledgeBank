---
tags:
  - homebrew
  - mac
description: 避免 install的总是自动更新影响速度
---
``` shell
echo 'export HOMEBREW_NO_AUTO_UPDATE=1' >> ~/.zshrc
source ~/.zshrc

echo 'export HOMEBREW_NO_AUTO_UPDATE=1' >> ~/.bash_profile
source ~/.bash_profile

```