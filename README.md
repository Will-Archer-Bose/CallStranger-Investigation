
# CallStranger-Investigation
UPnP CallStranger vulnerability investigation

Background Materials:
 - https://github.com/yunuscadirci/CallStranger
 - https://callstranger.com/

There is an upper limit to the size of the callbackURL, the device needs to store the entire thing and if it can't the SUBSCRIBE request will be rejected.  To test what the upper limit is on a specific device, I slightly modified the `CallStranger.py` and `CallDirect.py` filed from @yunuscadirci's repo.  Specifically, additional command line arguments that will add length to the callbackURL.

### Examples:
##### CallStranger.py
> original implementation
```
$ python3 CallStranger.py
```  
> force all callbackURLs to be 4096 characters long by appending `"&additional={random letters}"`
```
$ python3 CallStranger.py 4096
```
> force all callbackURLs to be in range of 4096 - 5 = 4091 to 4096 + 5 = 4101 characters long
```
$ python3 CallStranger.py 4096 5
```
_______________________
##### CallDirect.py
*you will be able to see the xml file names for individual devices printed to the console if you run CallStranger.py first*
> original implementation
```
$ python3 CallDirect.py http://1.2.3.4:5678/file_path.xml
```
> force all callbackURLs to be 8192 characters long
```
$ python3 CallDirect.py http://1.2.3.4:5678/file_path.xml 8192
```
I did not add the range option on CallDirect.py, but you easily could.

_________________________
### Interpreting results
If the CallStranger verification server determines a particular service is a "Verified vulnerable service" with a shorter URL and then returns it as an "Unverified service" with a longer callbackURL, this would be an indication that there is a threshold in between that is the maximum callbackURL length that device will allow.  This limit is the most data that could potentially be exfiltrated in one SUBSCRIBE call.
