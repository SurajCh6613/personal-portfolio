# personal-portfolio

<h2><u>Add Google Sheets in contact form</u></h2>
<ol>
  <li>Open your Google Sheet and create a blank sheet</li>
  <li>Rename column names as you given name in the contact form</li>
  <img width="923" alt="s1" src="https://github.com/SurajCh6613/portfolio/assets/114335583/8d7ffc80-dbbd-4b0a-bc48-937efc58e4cd">
  <li>Go to Extension=>App Scripts </li>
  <li>Copy the below code run the code and wait for succesfull execution</li>
  
      var sheetName = 'Sheet1'
      var scriptProp = PropertiesService.getScriptProperties()
    
    function intialSetup () {
      var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
      scriptProp.setProperty('key', activeSpreadsheet.getId())
    }
    
    function doPost (e) {
      var lock = LockService.getScriptLock()
      lock.tryLock(10000)
    
      try {
        var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
        var sheet = doc.getSheetByName(sheetName)
    
        var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
        var nextRow = sheet.getLastRow() + 1
    
        var newRow = headers.map(function(header) {
          return header === 'timestamp' ? new Date() : e.parameter[header]
        })
    
        sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])
    
        return ContentService
          .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
          .setMimeType(ContentService.MimeType.JSON)
      }
    
      catch (e) {
        return ContentService
          .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
          .setMimeType(ContentService.MimeType.JSON)
      }
    
      finally {
        lock.releaseLock()
      }
    }
  <li>Then click on Deploy=> New Deployment => Give Descrition => set Who Has Access to <b>Anyone => Click on Deploy</b></li>
  <li>After deployment copy the Scripts code</li>
  <li>Copy the below code and paste in your JS file</li>
  <li>Paste the copied deployed script code in scriptURL in below code</li>

    const scriptURL = 'Script-URL'
    const form = document.forms['submit-to-google-sheet']
    const success = document.getElementById('success');
    form.addEventListener('submit', e => {
      e.preventDefault()
      fetch(scriptURL, { method: 'POST', body: new FormData(form)})
        .then(response => {
          success.innerHTML = "Message sent successfully";
  
          setTimeout(function() {
              success.innerHTML = "";
          },4000)
          form.reset();
        })
        .catch(error => console.error('Error!', error.message))
    })
  <li>Paste 'submit-to-google-sheet' in form tag : name="submit-to-google-sheet" in your html file</li>
  <li>Now its done. Your Google Sheet linked with your form.</li>
</ol>
