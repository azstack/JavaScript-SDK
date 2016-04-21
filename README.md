# 1. Quick Start:

Download JavaScript SDK and example at: https://developers.azstack.co/SDK/javascript

Please review example (authentication, chat) here: https://github.com/azstack/JavaScript-SDK/blob/master/Example/example_1.html


```javascript
<!-- optional -->
<script type="text/javascript" src="js/jquery-1.12.1.min.js"></script>

<!-- required libraries -->
<script type="text/javascript" src="js/socket.io.js"></script>
<script type="text/javascript" src="js/jsencrypt.js"></script>
<script type="text/javascript" src="js/azstack_sdk-1.0-build-20160305-2.js"></script>

<script type="text/javascript">
	var currentMsgId = Math.floor(Date.now() / 1000);

	$(document).ready(function () {
		//init SDK
		var azAppID = "26870527d2ac628002dda81be54217cf"; // you will get it after registered
		var publicKey = 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq9s407QkMiZkXF0juCGjti6iWUDzqEmP+Urs3+g2zOf+rbIAZVZItS5a4BZlv3Dux3Xnmhrz240OZMBO1cNcpoEQNij1duZlpJY8BJiptlrj3C+K/PSp0ijllnckwvYYpApm3RxC8ITvpmY3IZTrRKloC/XoRe39p68ARtxXKKW5I/YYxFucY91b6AEOUNaqMFEdLzpO/Dgccaxoc+N1SMfZOKue7aH0ZQIksLN7OQGVoiuf9wR2iSz3+FA+mMzRIP+lDxI4JE42Vvn1sYmMCY1GkkWUSzdQsfgnAIvnbepM2E4/95yMdRPP/k2Qdq9ja/mwEMTfA0yPUZ7LiywoZwIDAQAB';
		var azStackUserId = 'user2';
		var fullname = 'Dau Ngoc Huy';
		var userCredentials = '274f011805dad952bf234f1cac990f18';
		//log level
		azstack.logLevel = 'INFO';

		//SDK delegate --------------------------------------------- -->
		azstack.onAuthenticationCompleted = function (code, authenticatedUser) {
			azstack.log('INFO', 'onAuthenticationCompleted code ' + code + ', authenticatedUser: ');
			azstack.log('INFO', authenticatedUser);

			$msgDiv.append('onAuthenticationCompleted code ' + code + ', authenticatedUser: ' + '<br />');
			$msgDiv.append(JSON.stringify(authenticatedUser) + '<br />');
		}
		azstack.onMessageReceived = function (user, msg) {
			azstack.log('INFO', 'onMessageReceived');
			azstack.log('INFO', user);
			azstack.log('INFO', msg);
		}
		azstack.onMessagesDelivered = function (packet) {
			azstack.log('INFO', 'onMessagesDelivered');
			azstack.log('INFO', packet);
		}
		azstack.onMessagesSent = function (packet) {
			azstack.log('INFO', 'onMessagesSent');
			azstack.log('INFO', packet);
		}
		azstack.onMessageFromMe = function (packet) {//msg from me (from other device)
			azstack.log('INFO', 'onMessageFromMe');
			azstack.log('INFO', packet);
		}
		//SDK delegate --------------------------------------------- <--

		//start authentication
		azstack.connect(azAppID, publicKey, azStackUserId, userCredentials, fullname);//connect AZStack server
	});
</script>
```

# 2. Create and configuration your application:
>	a. Create your application (project) here: https://developers.azstack.co/project/add

>	b. After you created successfully, click on menu "Keys", press "Generate a new keys" button to get your Public key, Private key and Secret code. Private key and Secret code will be stored in in your server to do authentication. The 3-ways authentication model between Client (web app) - AZStack server - your server, as described below: 
https://github.com/azstack/iOS-SDK-Documentation#32-authentication

>	c. Please refer to example of server authentication is here: https://github.com/azstack/Backend-example/tree/master/php
	Authentication URL is configured at menu "Authen URL" in https://developers.azstack.co

# 3. Using JavaScript SDK:
Required libraries:

> a. socket.io: https://github.com/socketio/socket.io-client

> b. jsencrypt: https://github.com/travist/jsencrypt

> c. AZStack SDK

Optional libraries:
>  JQuery: https://jquery.com/download/


Sample code:
```javascript
		<script type="text/javascript" src="js/socket.io.js"></script>
		<script type="text/javascript" src="js/jsencrypt.js"></script>
		<script type="text/javascript" src="js/azstack_sdk-1.0-build-20160305-2.js"></script>
```
# 4. Delegates

Delegate functions is required to receive successful events (receive incoming message, send message):
```javascript
//Call this function after authentication process is completed
azstack.onAuthenticationCompleted = function (code, authenticatedUser) {
}

//Function triggered when receiving incoming message
azstack.onMessageReceived = function (user, msg) {
}

//Function triggered when receiver receive message
azstack.onMessagesDelivered = function (packet) {
}

//Function triggered when send message successfully
azstack.onMessagesSent = function (packet) {
}

//Function triggered when receive message in other devices
azstack.onMessageFromMe = function (packet) {
}
```
# 5. Connect:
Call this function to connect to AZStack server and implement authentication process:
```javascript
azstack.connect(azAppID, publicKey, azStackUserId, userCredentials, fullname);
```
Parameters:

> azAppID: Your application ID, get this value from menu "Keys" at https://developers.azstack.co/

> publicKey: RSA public key of your application, get this from menu "Keys" at https://developers.azstack.co/

> azStackUserId: Unique ID for each user (string) in each application in AZStack, which identify unique user in that application. ID can be mobile number, email, username, ...

> userCredentials: can be password or token, ... This information will be forwarded by AZStack to your server in authentication process.

> fullname: Name (is used to send push notification to mobile when user is not online)

