//Script google

const sheets = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1Wq3gQ8H6OtbENDFUHfyS7VkKkUA6uwV4TO9ZFmC_gSU/edit#gid=0");
const sheet = sheets.getSheetByName("Sheet1");

function doPost(e){
    let data = e.parameter;

    // Handle the file upload:
    var base64String = data.PaymentInfo;
    var decodedData = Utilities.base64Decode(base64String);
    var blob = Utilities.newBlob(decodedData, 'application/octet-stream', 'uploadedFile');

    // Create a new file in Google Drive with the Blob
    var file = DriveApp.createFile(blob);

    // Get the URL of the uploaded file
    var fileUrl = file.getUrl();

    // Insert the file URL into the Google Sheet along with the other form data
    sheet.appendRow([data.Name, data.Email, data.SDT, data.Vocab, data.Months,fileUrl]);

    return ContentService.createTextOutput("Your message was successfully sent to the Googlesheet database!");
}
