## SoapUI

See LinkedIn course [API Test Automation with SoapUI](https://www.linkedin.com/learning/api-test-automation-with-soapui/creating-your-first-project-in-soapui)

### First project

https://api.carbonintensity.org.uk/


SoapUI can do Groovy scripting. To get today’s date into some fields that require a date:


```groovy
${=def now = new Date();now.format("yyyy-MM-dd")}

${= new Date().format("yyyy-MM-dd")}

${= new Date().plus(365).format("yyyy-MM-dd")}
```

#### To extract value for *deck_id* from JSON
```
{
   "shuffled": true,
   "remaining": 52,
   "success": true,
   "deck_id": "qdsfmvpz4hau"
}
```
**`${<TestStepName>#Response#$deck_id>}`**

#### To extract value for *code* from JSON
```
{
   "remaining": 51,
   "cards": [   {
      "image": "https://deckofcardsapi.com/static/img/JS.png",
      "images":       {
         "svg": "https://deckofcardsapi.com/static/img/JS.svg",
         "png": "https://deckofcardsapi.com/static/img/JS.png"
      },
      "value": "JACK",
      "suit": "SPADES",
      "code": "JS"
   }],
   "success": true,
   "deck_id": "i2uawfwsc6kk"
}
```

`${<TestStepName>#Response#$cards[0].code}`


### To read data from csv file
```
def inputFilePath = testRunner.testCase.testSuite.getPropertyValue('inputFile')
File file = new File(inputFilePath.toString()).eachLine{
	 testRunner.testCase.testSuite.setPropertyValue('inArea', it.split(",")[0])
	 testRunner.testCase.testSuite.setPropertyValue('inLocation', it.split(",")[1])
	 
	 def testCase = testRunner.testCase.testSuite.testCases['TestCase 1']
	 def runner = testCase.run(null, false)

}
```

### Test suite setup and teardown

Add to 'Setup Script' of the 'TestStep'
```groovy
def inputFilePath = testRunner.testCase.testSuite.getPropertyValue('inputFile')

def myFile = new File(inputFilePath)

def lines = myFile.readLines()
context.mylines = lines
assert lines.size() == 4
```

Change script of reading file to:

```groovy
context.mylines.each{
	testRunner.testCase.testSuite.setPropertyValue('inArea', it.split(",")[0])
     testRunner.testCase.testSuite.setPropertyValue('inLocation', it.split(",")[1])
     def testCase = testRunner.testCase.testSuite.testCases['TestCase 1']
     def runner = testCase.run(null, false)
     sleep 500
}
```

### Script Assertions

```groovy
def response_message = messageExchange.responseContent
def response_json = new groovy.json.JsonSlurper().parseText(response_message)

if (1 <= response_json.day_of_year & response_json.day_of_year < 366) {
	assert true	
} else {
	log.info("wrong day of year: ${response_json.day_of_year}")
	assert false
}
```

### Global properties

*inputFile* from the examples above can be used only in the test case where it was defined.
To define it somewhere more global SoapUI lets us do that with Global Properties. 
Go to File -> Preferences -> Global Properties.

`def inputFilePath = context.expand('${#Global#inputFile}')`

*#Global#* - global variables
*#Env#* - environment variables

### MacOS: run test cases from command line

```sh
$ cd /Applications/SoapUI-5.5.0.app/Contents/java/app/bin/.

$ sudo ./testrunner.sh <path_to_project_xml> -r

$ sudo ./testrunner.sh <path_to_project_xml> -r -f <path_to_file_report_to_save> -a
```
where r - to generate report, -f store it to the file, -a - store all results, by default it stores results only for 
failed cases

### Run test cases inside docker container

```sh
$ service docker status

$ docker run --name soaprunner -d -p 3000:3000 ddavison/soapui

$ curl --form "project=@downloads/deckofcardsapi-soapui-project.xml" --form "suite=TestSuite 1 - deck" --form "option=-r" http://192.168.1.85:3000/

```
After container was started go to `http://192.168.1.85:3000/`, where `192.168.1.85` is a server where my container was started


### Useful links

- https://www.soapui.org/resources/tutorials/rest-sample-project.html?utm_source=soapui&utm_medium=starterpage
- https://api.carbonintensity.org.uk/
- https://deckofcardsapi.com/
- http://worldtimeapi.org/
- [Assertions in SoapUI](https://www.guru99.com/assertions-soapui-complete-tutorial.html)
- [Properties](https://www.soapui.org/docs/functional-testing/properties/working-with-properties.html)
- [SoapUI tips](https://www.soapui.org/scripting-properties/tips-tricks.html)


