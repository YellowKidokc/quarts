---
category: theophysics-research
date: '2025-08-26'
status: published
tags:
- o
- theophysics
- scripts
title: Scripts Notes
---

app.workspace.on('file-open', () => {
    app.commands.executeCommandById("editor.foldAll");
});

app.workspace.on('workspace-leave', () => {
    app.commands.executeCommandById("editor.foldAll");
});
