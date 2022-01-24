# GNU Screen

Meta `Ctrl+a`.

## Window

Meta +

- `c` create new window
- `"` list all windows
- `p` previous window
- `n` next window
- `<number>` select window

`Ctrl+d` kill window

## Session

- `screen -ls` list sessions
- `screen -S <session name>` start session with given name
- `Ctrl+a` `d` detatch session
- `screen -x <session name>` resume session with given name
- `screen -S <session name> -X quit` terminate detached session

## Layout

- `Ctrl+a` `S` split horizontally
- `Ctrl+a` `|` split vertically
- `Ctrl+a` `X` close current region
- `Ctrl+a` `<TAB>` switch to next region
- `Ctrl+a` `Q` close all but current region

## Buffer mode

- `Ctrl+a` `<ESC>` enter copy mode
- `<SPACE>` select and copy text in copy mode

