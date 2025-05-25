<%*
const BASE_FOLDER = tp.file.folder(true);  // Full path to current file's folder
const CURRENT_FILE_PATH = tp.file.path(); // Full path to this file
const CURRENT_FOLDER_NAME = BASE_FOLDER.split("/").pop();  // Last folder name
const files = app.vault.getMarkdownFiles();
const grouped = {};

// Group files by subfolder path (relative to BASE_FOLDER)
files.forEach(f => {
  if (f.path.startsWith(BASE_FOLDER + "/") && f.path !== CURRENT_FILE_PATH) {
    const relative = f.path.slice(BASE_FOLDER.length + 1);
    const parts = relative.split("/");
    const subfolder = parts.length > 1 ? parts.slice(0, -1).join("/") : ".";
    if (!grouped[subfolder]) grouped[subfolder] = [];
    grouped[subfolder].push(f);
  }
});

// Sort and render
const sortedFolders = Object.keys(grouped).sort();
sortedFolders.forEach(folder => {
  const label = folder === "." ? CURRENT_FOLDER_NAME : folder;
  tR += `### ${label}\n`;
  grouped[folder]
    .sort((a, b) => a.basename.localeCompare(b.basename))
    .forEach(f => {
      tR += `- [[${f.basename}]]\n`;
    });
  tR += `\n`;
});
%>
