
## Config `~/.tmux.conf`:

```tmux
set -g history-limit 50000
set -g allow-rename off # don't rename window iif static name was defined
set-window-option -g mode-keys vi # set search mode as vi (default is emac)
run-shell /opt/tmux-logging/logging.tmux
```

## Keys

- `ctrl` + `b`: **prefix key** (default)
- **prefix key** + `?`: pulls up everything you can do
- **prefix key** + `c`: create a new window
- **prefix key** + `,`: give a static window
- **prefix key** + `0..N`: go to window X
- **prefix key** + `[`: enter to search mode
  + hit `spacebar` and it puts you to copy mode
  + **prefix key** + `]`: paste
- **prefix key** + `alt` + `shift` + `p`: save to a log file
- **prefix key** + `t`: show time

## Windows

- **prefix key** + `%` : vertical split
- **prefix key** + `"` : horizontal split
- **prefix key** + ` `  : switch horizontal/vertical
- **prefix key** + `{`  : move pane to the left
- **prefix key** + `}`  : move pane to the right
- **prefix key** + arrow : change pane size

## Commands

```bash
$ tmux new -s $SESSION_NAME # create a new session

$ tmux ls # list tmux sessions in current shell

$ tmux attach -t $SESSION_NAME # attach the session to the current shell
```
