# CEM-PIN-Brute
Brute forcing Cisco Extension Mobility voicemail PINs

Cisco Extension Mobility (EM) can be configured to allow voicemail login with username and PIN.  If you have the username associated with a phone number you can test response codes from the EM service URL to determine if you've guessed the PIN for the user's voice mailbox, then attempt to access it by dialing the Cisco Unity Connection Messaging System.

The test user in this example is:

`jdoe, (216) 555-0931`

Brute forcing occurs across two requests (1) Perform a logout on a fake device name followed by (2) an attempt to guess the PIN for the user.  This technique is not always reliable, you may need to introduce delays (15 seconds or more) between requests, which also slows down the attack significantly.  A 201 response code indicates that the PIN was incorrect.  A 205 response code indicates that the PIN may be correct.  Here are some links about using the EM service URL and response codes.

https://www.cisco.com/c/en/us/support/docs/unified-communications/unified-communications-manager-callmanager/212083-CUCM-12-X-Extension-Mobility-EM-and-Ext.pdf

https://community.cisco.com/t5/collaboration-knowledge-base/common-problems-in-extension-mobility/ta-p/3121896

The logout request resembles this:

`curl -s 'http://<EM_IP_Address>:8080/emapp/EMAppServlet?device=SEP123412341234&doLogout=true'`

The PIN guess attempt resembles this:

`curl -s 'http://<EM_IP_Address>:8080/emapp/EMAppServlet?device=SEP123412341234&userid=jdoe&seq=0931&loginType=UID' | grep -i 'Login is unavailable (205)'`

If you receive a 205 response code you could then try to log into the user's voice mailbox:<br />
* Dial the user's phone number
* Enter '*'
* Enter the user's extension
* Enter the PIN

Some suggestions for guessing the PIN based upon typical user behavior:<br />
* phone last 4 digits
* phone last 4 digits reversed
* sequences (1234, 12345, 0000, etc)
* keypad patterns (2580, 0825, 1397, etc)
