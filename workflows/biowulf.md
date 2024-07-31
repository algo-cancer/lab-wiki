# Persistent interactive session setup

## 1. `ssh biowulf`

## 2. Start interactive session

[`start-interactive.sh`](https://gist.github.com/michael-ford/f81aa1621a22fa341f6e57b5829d9bb0):
```
sinteractive --mem=40g --cpus-per-task=16 --tunnel --time 36:00:00
```

## 3. SSH into interactive node
`ssh cn4319`

Requires `~/.ssh/config`:
```
Host cn*
        User fordmk
        ProxyCommand /usr/bin/ssh -o ForwardAgent=yes fordmk@biowulf.nih.gov nc -w 120ms %h %p
```

## 4. Start tmux, jupyter:
[`jupyter-tmux.sh`](https://gist.github.com/michael-ford/8264ae57370e6b1da80f81145340623c):
```
#!/bin/bash

port=$(/usr/local/slurm/bin/reconnect_tunnels --list 2> /dev/null)

tmux new-session -d -s jupyter
tmux send-keys -t jupyter "cd /data/$USER; jupyter lab --no-browser --port=$port" C-m

tmux new-session -d -s main
sleep 5
tmux send-keys -t main "cd /data/$USER; get-jupyter-url.sh" C-m
tmux attach -t main

exit 0
```

[`get-jupyter-url.sh`](https://gist.github.com/michael-ford/14f585937217e8bd7e952e8a16971fba):
```
#!/bin/bash

result=""

while [[ -z "$result" ]]; do
    result=$(jupyter server list 2> /dev/null | grep http | cut -f1 -d' ')
    sleep 1
done

echo "$result"
```

## 5. Setup proxy tunnel, open jupyter

`ssh biowulf.nih.gov /usr/local/slurm/bin/reconnect_tunnels`

Copy the URL from the output and paste it into your browser.

## 6. [Setup Visual Studio Code](https://hpc.nih.gov/apps/vscode.html)

- Command + shift + p: `Remote-SSH: Connect current window to host...`
- `cn4319`

