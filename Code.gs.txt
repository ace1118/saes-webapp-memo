function doGet() {
    var html = HtmlService.createHtmlOutputFromFile('index');
    return html.setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
}

function uploadFiles(data) {
    try {
        var file = data.myFile;
        var folder = DriveApp.getFolderById('1nnrvb-jew7Z_szGs-w3zqemUOV9jTcrx');
        var newFileName = data.newFileName || file.name; // Use the input value as the new file name or use the original file name if not provided
        var createFile = folder.createFile(file).setName(newFileName); // Set the new file name for the created file
        return createFile.getUrl();
    } catch (error) {
        throw new Error("Upload Failed");
    }
}

function deleteFile(fileId) {
    try {
        var file = DriveApp.getFileById(fileId);  // Use the fileId passed in
        file.setTrashed(true); // Moves to Trash instead of permanent deletion
        return true;
    } catch (e) {
        return false;
    }
}

function getFilesInFolder() {
    var folderId = '1nnrvb-jew7Z_szGs-w3zqemUOV9jTcrx'; // Replace with your Google Drive folder ID
    var folder = DriveApp.getFolderById(folderId);
    var files = folder.getFiles();

    var fileList = [];

    while (files.hasNext()) {
        var file = files.next();
        fileList.push({
            name: file.getName(),
            id: file.getId()
        });
    }

    return fileList;
}
