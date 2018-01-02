---
layout: post
title: Tmux Part 1
comments: true
subdir: tmux
tags:
- Tmux
---

## Installation

```bash
$ brew install tmux
$ brew info tmux
```

## Customization

```bash
# ~/.tmux.conf
unbind C-b
set -g prefix C-s

$ tmux source-file ~/.tmux.conf
```

## Usage

```bash
# list sessions
$ tmux list-sessions

# create sessions
$ tmux new-session -s upcase
$ tmux new-session -s home

# switch session by name
# or simply prefix L
$ tmux switch-client -t upcase
$ tmux switch-client -t home

# split the screen
$ prefix % # split middle
$ prefix double quotes # split center

# jump the screen
prefix o
```