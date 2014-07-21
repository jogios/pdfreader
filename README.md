#PDF Reader

## Description

This module extends https://github.com/JoanZapata/android-pdfview in titanium. 

## Usage

```javascript
var win = Ti.UI.createWindow({
	backgroundColor : 'white',
	exitOnClose : true
});
if (Ti.Platform.name == "android") {

	var READER_MODULE = require('com.mykingdom.pdfreader');
	var enableSwipe = true;
	//swipe enabled by default

	var pdfReader = READER_MODULE.createReader({
		top : 70
	});
	win.add(pdfReader);

	var toggleSwipeBtn = Ti.UI.createButton({
		top : 10,
		title : "Toggle Swipe"
	});
	toggleSwipeBtn.addEventListener("click", function() {
		enableSwipe = !enableSwipe;
		console.log("enableSwipe = " + enableSwipe);
		pdfReader.enableSwipe(enableSwipe);
	});
	win.add(toggleSwipeBtn);

	var backwardBtn = Ti.UI.createButton({
		top : 10,
		title : "Backward",
		left : 10
	});
	backwardBtn.addEventListener("click", function() {
		var page = pdfReader.getCurrentPage() - 1;
		if (page >= 1) {
			console.log("Backward = " + page);
			pdfReader.setCurrentPage(pdfReader.getCurrentPage() - 1);
		}
	});
	win.add(backwardBtn);

	var forwardBtn = Ti.UI.createButton({
		top : 10,
		title : "Forward",
		right : 10
	});
	forwardBtn.addEventListener("click", function() {
		var page = pdfReader.getCurrentPage() + 1;
		if (page <= pdfReader.getPageCount()) {
			console.log("Forward = " + page);
			pdfReader.setCurrentPage(page);
		}
	});
	win.add(forwardBtn);

	pdfReader.addEventListener("pagechanged", function(evt) {
		/*
		 *
		 * properties of evt
		 * currentPage - being viewed
		 * pageCount - number of pages in pdf
		 *
		 */
		console.log("Viewing " + evt.currentPage + " / " + evt.pageCount);
	});

	//Make sure the file exists
	var file = Ti.Filesystem.getFile(Ti.Filesystem.externalStorageDirectory, "sample.pdf");

	if (!file.exists()) {
		var source = Ti.Filesystem.getFile(Ti.Filesystem.resourcesDirectory, "sample.pdf");
		file.write(source.read());
	}

	/*
	 *
	 * args : loadPDFFromFile
	 * file (Ti.Filesystem.File) - PDF file to load
	 * defaultPageNumber (int) - page to be loaded by default
	 *
	 */
	console.log(">>EXISTS>>>" + file.exists());
	pdfReader.loadPDFFromFile(file, 5);
	// start at page no. 5
}
win.open();
```
