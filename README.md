# 1. Quick Start:

Download JavaScript SDK and example at: https://developers.azstack.co/SDK/javascript

Please review example (authentication, chat):


```javascript
<!-- optional -->
<script type="text/javascript" src="js/jquery-1.12.1.min.js"></script>

<!-- required libraries -->
<script type="text/javascript" src="js/socket.io.js"></script>
<script type="text/javascript" src="js/jsencrypt.js"></script>
<script type="text/javascript" src="js/azstack_sdk-1.2-build-20160524.js"></script>

<script type="text/javascript">
	var currentMsgId = Math.floor(Date.now() / 1000);

	$(document).ready(function () {
		//init SDK
		var azAppID = "26870527d2ac628002dda81be54217cf"; // you will get it after registered
		var publicKey = 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq9s407QkMiZkXF0juCGjti6iWUDzqEmP+Urs3+g2zOf+rbIAZVZItS5a4BZlv3Dux3Xnmhrz240OZMBO1cNcpoEQNij1duZlpJY8BJiptlrj3C+K/PSp0ijllnckwvYYpApm3RxC8ITvpmY3IZTrRKloC/XoRe39p68ARtxXKKW5I/YYxFucY91b6AEOUNaqMFEdLzpO/Dgccaxoc+N1SMfZOKue7aH0ZQIksLN7OQGVoiuf9wR2iSz3+FA+mMzRIP+lDxI4JE42Vvn1sYmMCY1GkkWUSzdQsfgnAIvnbepM2E4/95yMdRPP/k2Qdq9ja/mwEMTfA0yPUZ7LiywoZwIDAQAB';
		var azStackUserId = 'user2'; // userid defined in your system
		var fullname = 'Dau Ngoc Huy'; // username goes with userid
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
		<script type="text/javascript" src="js/azstack_sdk-1.2-build-20160524.js"></script>
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

//Triggered when receive message from group
azstack.onGroupMessageReceived = function (msg) {
}

//Triggered when retrieving list conversation
azstack.onListModifiedConversationReceived = function (packet) {
}

//Triggered when retrieving list of read messages
azstack.onListModifiedMessagesReceived = function (packet) {
}

//Triggered when retrieving list of Unread messages
azstack.onListUnreadMessagesReceived = function (packet) {
}

//Triggered when you are a member in a newly created group (someone created group that have you as member)
azstack.onMakeGroupNotification = function (packet) {
}

//Triggered when another member is typing in this group
azstack.chat_group_typing_processor = function (service, body) {
}

//Triggered when someone is typing with current user
azstack.chat_typing_processor = function (service, body) {
}

//Triggered when someone invited another member in the group that you are currently a member
azstack.onInviteGroupNotification = function (packet) {
}

//Triggered when someone quit group
azstack.onLeaveGroupNotification = function (packet) {
}

//Triggered when group name is edited
azstack.onRenameGroupNotification = function (packet) {
}

//Triggered when conversation is deleted
azstack.onDeleteConversation = function (packet) {
}

//Triggered when message is seen
azstack.onSeenMessage = function (packet) {
}

//Triggered when all messages are seen
azstack.onSeenMessages = function (packet) {
}

//Triggered when list of message is deleted
azstack.onDeleteMessage = function (packet) {
}

//Triggered when get list of chat group
azstack.onListChatGroup = function (packet) {
}

//Triggered when group admin has been assigned to another member
azstack.onChatGroupChangeAdmin = function (packet) {
}

//Triggered when receiving list of files
azstack.onListFilesReceived = function (packet) {
}

//Triggered when user status is changed (background or foreground)
azstack.onApplicationChangeState = function (packet) {
}

//Triggered when broadcast is created
azstack.onBroadCastCreateList = function (packet) {
}

//Triggered when broadcast is edited
azstack.onBroadCastEditList = function (packet) {
}

//Triggered when receiving list of broadcast
azstack.onBroadCastGetList = function (packet) {
}

//Triggered when broadcast is sent
azstack.onMessageBroadCast = function(packet){
}

//Triggered when there is new message in broadcast
azstack.onHaveMessageBroadCast = function(packet){
}

//Triggered when broadcast is deleted
azstack.onChatBroadCastDeleteList = function(packet){
}

//Triggered when getting message from broadcast
azstack.onChatBroadCastGetMessages = function(packet){
}

//Triggered when joining channel
azstack.onChatGroupJoinPublic = function(packet){
}

//Triggered when receiving list of channel
azstack.onChatGroupGetListPublic = function(packet){
}

```
# 5. connect:
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

# 6. azGetListModifiedConversations:
Lấy danh sách conversation sau khi authen thành công:
```javascript
azstack.azGetListModifiedConversations(lastUpdate);
```
Parameters:

> lastUpdate: only get list of conversations that have change after [lastUpdate] (tính bằng miliseconds)

# 7. azGetListModifiedMessages:
Get list of message from 1 conversation:
```javascript
azstack.azGetListModifiedMessages(lastUpdate, type, chatId);
```
Parameters:

> lastUpdate: only get list of conversations that have change after [lastUpdate] (tính bằng miliseconds)

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 8. azGetListUnreadMessages:
Get list of unread message from 1 conversation:
```javascript
azstack.azGetListUnreadMessages(type, chatId);
```
Parameters:

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 9. getUserInfoAndRequestToServer:
Get information from user by userId
```javascript
azstack.getUserInfoAndRequestToServer(userId);
```
Parameters:

> chatId: userId of user

# 10. getUserInfoAndRequestToServerWithCallBack:
Get information of user by userId with callback
```javascript
azstack.getUserInfoAndRequestToServerWithCallBack(userId, callback);
```
Parameters:

> chatId: userId of user

> callback: after request to server and get user data, function will be called

# 11. getUserInfoByUsernameAndRequestToServer:
Get information of user by azStackUserID
```javascript
azstack.getUserInfoByUsernameAndRequestToServer(azStackUserID);
```
Parameters:

> azStackUserID: azStackUserID of user

# 12. getUserInfoByUsernameAndRequestToServerWithCallBack:
Get information of user by azStackUserID with callback
```javascript
azstack.getUserInfoByUsernameAndRequestToServerWithCallBack(azStackUserID, callback);
```
Parameters:

> chatId: azStackUserID of user

> callback: after request to server and get user data, function will be called

# 13. azSendMessage:
Send 1 message text chat 1 - 1
```javascript
azstack.azSendMessage(msgContent, azStackUserID, msgId);
```
Parameters:

> msgContent: content of message

> azStackUserID: azStackUserID of user who receive message

> msgId: Id of message must be unique related to conversation and sender. Should be: currentMsgId++

# 14. azSendMessageFileUrl:
Send 1 message picture/audio/video/file chat 1 - 1
```javascript
azstack.azSendMessageFileUrl(url, fileName, msgType, azStackUserID, msgId, fileLength, width, height);
```
Parameters:

> url: file's path

> fileName: file's name

> msgType: msgType = 1 (Photo), msgType = 2 (Voice), msgType = 8 (File), msgType = 9 (Video)

> azStackUserID: azStackUserID of user who receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> fileLength: file size (kilobytes)

> width: width of picture (photo message)

> height: height of picture (photo message)

# 15. azSendMessageSticker:
Send message sticker chat 1 - 1
```javascript
azstack.azSendMessageSticker(azStackUserID, imgName, catId, msgId, url, width, height);
```
Parameters:

> azStackUserID: azStackUserID of user who receive chat message

> imgName: name of sticker

> catId: category of sticker

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of sticker

> width: width of sticker

> height: height of sticker

# 16. azSendMessageGroup:
Send one message text chat group
```javascript
azstack.azSendMessageGroup(msgContent, groupId, msgId);
```
Parameters:

> msgContent: content of message

> groupId: groupId of group that receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 17. azSendMessageFileUrlGroup:
Send one message ảnh/audio/video/file chat group
```javascript
azstack.azSendMessageFileUrlGroup(groupId, msgId, url, msgType, fileName, fileLength, width, height);
```
Parameters:

> groupId: groupId of group that receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of file

> msgType: msgType = 1 (Photo), msgType = 2 (Voice), msgType = 8 (File), msgType = 9 (Video)

> fileName: file name

> fileLength: File size of file, (kilobytes)

> width: width of picture, (photo message)

> height: height of picture, (photo message)

# 18. azSendMessageStickerGroup:
Send one message sticker chat group
```javascript
azstack.azSendMessageStickerGroup(groupId, imgName, catId, msgId, url, width, height);
```
Parameters:

> groupId: groupId of group that receive message

> imgName: name sticker

> catId: category of sticker

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of sticcker

> width: width of sticker

> height: height of sticker

# 19. azSendMessageReport:
Report that message has receive chat message 1 - 1
```javascript
azstack.azSendMessageReport(toUserId, msgId);
```
Parameters:

> toUserId: userId of sender

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 20. azSendMessageGroupReport:
Report message has receive chat group
```javascript
azstack.azSendMessageGroupReport(group, msgId, msgSender);
```
Parameters:

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> msgSender: userId of sender

# 21. azSendTyping:
Send typing status to 1 conversation
```javascript
azstack.azSendTyping(type, chatId);
```
Parameters:

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 22. azMakeGroup:
Tạo 1 group
```javascript
azstack.azMakeGroup(members, name, msgId, type, callback);
```
Parameters:

> members: array userId of user is member in group

> name: name of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> type: type = 0 (group private), type = 1 (group public / channel)

> callback: after created group, this function will be triggered

# 23. azInviteToGroup:
Add members into group
```javascript
azstack.azInviteToGroup(members, group, msgId, callback);
```
Parameters:

> members: array of userId of user wanted to add into group

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after add new members, this function will be triggered

# 24. azLeaveGroup:
Leave group/kick member of group
```javascript
azstack.azLeaveGroup(leaveUser, group, msgId, callback);
```
Parameters:

> leaveUser: userId of user wanted to leave group/kick member

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after leave group/kick member, this function will be triggered

# 25. azRenameGroup:
Đổi name group
```javascript
azstack.azRenameGroup(name, group, msgId, callback);
```
Parameters:

> name: name group wanted to change

> group: Id of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after change name group, this function will be triggered

# 26. azGetGroupInfo:
Get group information from server
```javascript
azstack.azGetGroupInfo(groupId, callback);
```
Parameters:

> groupId: Id of group

> callback: after receive group information, this function will be triggered

# 27. azSeenMessage:
See 1 message
```javascript
azstack.azSeenMessage(msgId, senderId, type, chatId);
```
Parameters:

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> senderId: userId of user saw message

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

# 28. azSeenMessages:
See list of message
```javascript
azstack.azSeenMessages(chatId, type, lastMsgCreated, list);
```
Parameters:

> chatId: userId of user or Id of group

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> lastMsgCreated: Time of latest seen message

> list: list of message, example: [{msgId: msgId, sender: senderId}, ...]

# 29. disconnect:
Disconnect with azstack
```javascript
azstack.disconnect();
```
Parameters:

# 30. azDeleteMessage:
Delete 1 message
```javascript
azstack.azDeleteMessage(msgId, senderId, chatId, type, callback);
```
Parameters:

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> senderId: userId of user

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

> callback: after delete message, this function will be triggered

# 31. azListChatGroup:
Get list of groups that user is joining
```javascript
azstack.azListChatGroup();
```
Parameters:

# 32. azListFiles:
Get list of files in all conversation
```javascript
azstack.azListFiles(lastUpdate);
```
Parameters:

> lastUpdate: Only get files that have updated after this time (miliseconds)

# 33. azChatGroupChangeAdmin:
Change admin to group
```javascript
azstack.azChatGroupChangeAdmin(group, newAdmin, msgId);
```
Parameters:

> group: Id of group

> newAdmin: userId of user is going to be admin

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 34. azApplicationChangeState:
Change status of user (background or foreground)
```javascript
azstack.azApplicationChangeState(state);
```
Parameters:

> state: state = 1 (background), state = 2 (foreground)

# 35. azChatBroadCastCreateList:
Create broadcast
```javascript
azstack.azChatBroadCastCreateList(members, name);
```
Parameters:

> members: array userId of users to be added in broadcast

> name: name of broadcast

# 36. azChatBroadCastEditList:
Sửa broadcast
```javascript
azstack.azChatBroadCastEditList(removeUsers, name, id, addUsers, msgId);
```
Parameters:

> removeUsers: array userId of users to be deleted from broadcast

> name: name of broadcast that want to change name (keep old name if dont want to change)

> id: Id of broadcast

> addUsers: array userId of users to be added into broadcast

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 37. azChatBroadCastGetList:
Sửa broadcast
```javascript
azstack.azChatBroadCastGetList(lastUpdate);
```
Parameters:

> lastUpdate: Only get broadcast with changes after this point (miliseconds)

# 38. azMessageBroadCast:
Send text message to broadcast
```javascript
azstack.azMessageBroadCast(broadcast, msg, msgId);
```
Parameters:

> broadcast: Id of broadcast

> msg: content of message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 39. azMessageStickerBroadCast:
Send sticker message to broadcast
```javascript
azstack.azMessageStickerBroadCast(broadcast, msgId, imgName, catId, url, width, height);
```
Parameters:

> broadcast: Id of broadcast

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> imgName: name sticker

> catId: category of sticker

> url: file path of sticker

> width: width of sticker

> height: height of sticker

# 40. azMessageFileBroadCast:
Send one message ảnh/audio/video/file chat broadcast
```javascript
azstack.azMessageFileBroadCast(broadcast, msgId, type, url, fileName, fileLength, width, height);
```
Parameters:

> broadcast: Id of broadcast receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of file

> msgType: msgType = 1 (Photo), msgType = 2 (Voice), msgType = 8 (File), msgType = 9 (Video)

> fileName: file name

> fileLength: File size of file, (kilobytes)

> width: width of picture, (photo message)

> height: height of picture, (photo message)

# 41. azChatBroadCastDeleteList:
Delete broadcast
```javascript
azstack.azChatBroadCastDeleteList(broadcastId);
```
Parameters:

> broadcastId: Id of broadcast to be deleted

# 42. azChatBroadCastGetMessages:
Delete broadcast
```javascript
azstack.azChatBroadCastGetMessages(broadcastId, lastUpdate);
```
Parameters:

> broadcastId: Id of broadcast

> lastUpdate: Only get messages with changes after this point (miliseconds)

# 43. azChatGroupJoinPublic:
Join public group/channel
```javascript
azstack.azChatGroupJoinPublic(group, msgId);
```
Parameters:

> group: Id of group/channel to be joined

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

# 44. azChatGroupGetListPublic:
Get list of public group/channel
```javascript
azstack.azChatGroupGetListPublic();
```
