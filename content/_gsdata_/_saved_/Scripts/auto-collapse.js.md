app.workspace.on('file-open', () => {
    app.commands.executeCommandById("editor.foldAll");
});

app.workspace.on('workspace-leave', () => {
    app.commands.executeCommandById("editor.foldAll");
});
