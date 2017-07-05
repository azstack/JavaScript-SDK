# 1. Quick Start:

Download JavaScript SDK and example at: https://developers.azstack.co/SDK/javascript

Please review example (authentication, chat):


```javascript
<!-- optional -->
<script type="text/javascript" src="js/jquery-1.12.1.min.js"></script>

<!-- required libraries -->
<script type="text/javascript" src="js/socket.io.js"></script>
<script type="text/javascript" src="js/jsencrypt.js"></script>
<script type="text/javascript" src="js/adapter-1.3.0.js"></script>
<script type="text/javascript" src="js/azstack_sdk-1.5-build-20160608.js"></script>

<script type="text/javascript">
	var currentMsgId = Math.floor(Date.now() / 1000);

	$(document).ready(function () {
		//init SDK
		var azAppID = "26870527d2ac628002dda81be54217cf"; // you will get it after registered
		var publicKey = 'MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAq9s407QkMiZkXF0juCGjti6iWUDzqEmP+Urs3+g2zOf+rbIAZVZItS5a4BZlv3Dux3Xnmhrz240OZMBO1cNcpoEQNij1duZlpJY8BJiptlrj3C+K/PSp0ijllnckwvYYpApm3RxC8ITvpmY3IZTrRKloC/XoRe39p68ARtxXKKW5I/YYxFucY91b6AEOUNaqMFEdLzpO/Dgccaxoc+N1SMfZOKue7aH0ZQIksLN7OQGVoiuf9wR2iSz3+FA+mMzRIP+lDxI4JE42Vvn1sYmMCY1GkkWUSzdQsfgnAIvnbepM2E4/95yMdRPP/k2Qdq9ja/mwEMTfA0yPUZ7LiywoZwIDAQAB';
		var azStackUserId = 'user2';
		var fullname = 'Dau Ngoc Huy';
		var userCredentials = '274f011805dad952bf234f1cac990f18';
                var namespace = '';
		//log level
		azstack.logLevel = 'INFO';

		//call options
		azstack.azSetCallOptions('AUTO-IFRAME', { enableAudio: true, enableVideo: true });// call mode: MANNUALLY, AUTO-IFRAME, AUTO-POPUP; second parameter: init enable audio and video (can toggle on/off later)

		//SDK delegate --------------------------------------------- -->
		azstack.onAuthenticationCompleted = function (code, authenticatedUser, msg) {
			azstack.log('INFO', 'onAuthenticationCompleted code: ' + code + ', msg: ' + msg + ', authenticatedUser');
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
		azstack.connect(azAppID, publicKey, azStackUserId, userCredentials, fullname, namespace);//connect AZStack server
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
		<script type="text/javascript" src="js/adapter-1.3.0.js"></script>
                <script type="text/javascript" src="js/azstack_sdk-1.5-build-20160608.js"></script>
```
# 4. Delegates

Delegate functions is required to receive successful events (receive incoming message, send message):
```javascript
//Call this function after authentication process is completed
azstack.onAuthenticationCompleted = function (code, authenticatedUser, msg) {
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

//Triggered when retrieving list conversation by page and tag
azstack.onListModifiedConversationByPageAndTagReceived = function (packet) {
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

//Triggered when get push customize event
azstack.onPushCustomizeEvent = function(packet){
}

//Triggered when get resut from join chat conference
azstack.onJoinConferenceChat = function(packet){
}

//Triggered when co cuoc goi video/voice (MANNUALLY mode only)
azstack.onInviteVideoCall = function(packet){
}

//Triggered when cuoc goi video/voice stop (MANNUALLY mode only)
azstack.onVideoCallStop = function(packet){
}

//Triggered when cuoc goi video/voice dang bat dau (MANNUALLY mode only)
azstack.onVideoCallConnecting = function(packet){
}

//Triggered when cuoc goi video/voice dang đổ chuông (MANNUALLY mode only)
azstack.onVideoCallRinging = function(packet){
}

//Triggered when cuoc goi video/voice bị từ chối (MANNUALLY mode only)
azstack.onVideoCallRejected = function(packet){
}

//Triggered when cuoc goi video/voice đã trả lời (MANNUALLY mode only)
azstack.onVideoCallAnswered = function(packet){
}

//Triggered when cuoc goi video/voice bận (MANNUALLY mode only)
azstack.onVideoCallBusy = function(packet){
}

//Triggered when cuoc goi video/voice không trả lời (MANNUALLY mode only)
azstack.onVideoCallNotAnswered = function(packet){
}

//Triggered when cuoc goi video/voice lỗi (MANNUALLY mode only)
azstack.onVideoCallError = function(packet){
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình bân (MANNUALLY mode only)
azstack.onVideoCallBusyByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình trả lời (MANNUALLY mode only)
azstack.onVideoCallAnsweredByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình từ chối (MANNUALLY mode only)
azstack.onVideoCallRejectedByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình stop (MANNUALLY mode only)
azstack.onVideoCallStopByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình không trả lời (MANNUALLY mode only)
azstack.onVideoCallNotAnsweredByMe = function(packet) {
}

//Triggered when cuoc goi video/voice thiết bị khác trên tài khoản của mình lỗi (MANNUALLY mode only)
azstack.onVideoCallErrorByMe = function(packet) {
}

//Triggered when cuoc goi callout dang bat dau (MANNUALLY mode only)
azstack.onCalloutConnecting = function(packet){
}

//Triggered when cuoc goi callout dang đổ chuông (MANNUALLY mode only)
azstack.onCalloutRinging = function(packet){
}

//Triggered when cuoc goi callout bị từ chối (MANNUALLY mode only)
azstack.onCalloutRejected = function(packet){
}

//Triggered when cuoc goi callout đã trả lời (MANNUALLY mode only)
azstack.onCalloutNotAnswered = function(packet){
}

//Triggered when cuoc goi callout stop (MANNUALLY mode only)
azstack.onCalloutStop = function(packet){
}

//Triggered when cuoc goi callout bận (MANNUALLY mode only)
azstack.onCalloutBusy = function(packet){
}

//Triggered when cuoc goi callout không trả lời (MANNUALLY mode only)
azstack.onCalloutNotAnswered = function(packet){
}

//Triggered when cuoc goi callout lỗi (MANNUALLY mode only)
azstack.onCalloutError = function(packet){
}

//Triggered when co cuoc goi callin (MANNUALLY mode only)
azstack.onCallinStart = function(packet){
}

//Triggered when cuoc goi callin stop when ringing (MANNUALLY mode only)
azstack.onCallinRingingStop = function(packet){
}

//Triggered when co cuoc goi callin stop (MANNUALLY mode only)
azstack.onCallinStop = function(packet){
}

//Triggered when cuoc goi callin error (MANNUALLY mode only)
azstack.onCallinError = function(packet){
}

//Triggered when cuoc goi callin thiết bị khác trên tài khoản của mình bân (MANNUALLY mode only)
azstack.onCallinBusyByMe = function(packet) {
}

//Triggered when cuoc goi callin thiết bị khác trên tài khoản của mình trả lời (MANNUALLY mode only)
azstack.onCallinAnsweredByMe = function(packet) {
}

//Triggered when cuoc goi callin thiết bị khác trên tài khoản của mình từ chối (MANNUALLY mode only)
azstack.onCallinRejectByMe = function(packet) {
}

//Triggered when cuoc goi callin thiết bị khác trên tài khoản của mình stop (MANNUALLY mode only)
azstack.onCallinStopByMe = function(packet) {
}

//Triggered when cuoc goi callin thiết bị khác trên tài khoản của mình không trả lời (MANNUALLY mode only)
azstack.onCallinNotAnsweredByMe = function(packet) {
}

//Triggered when cuoc goi callin thiết bị khác trên tài khoản của mình lỗi (MANNUALLY mode only)
azstack.onCallinErrorByMe = function(packet) {
}

//Triggered when cuoc goi video/voice load được video máy của mình (MANNUALLY mode only)
azstack.onVideoCallLocalVideoLoaded = function(){
}

//Triggered when cuoc goi video/voice load được video máy của người gọi mình (MANNUALLY mode only)
azstack.onVideoCallRemoteVideoLoaded = function(){
}

//trigger when WebRTC inited in call mode MANNUALLY
azstack.onVideoCallWebRTCInit = function() {
}

//trigger when WebRTC destroyed in call mode MANNUALLY
azstack.onVideoCallWebRTCDestroy = function(packet) {
}

//trigger when iframe inited in call mode AUTO-IFRAME
azstack.onVideoCallIframeInit = function() {
}


//trigger when iframe destroyed in call mode AUTO-IFRAME
azstack.onVideoCallIframeDestroy = function(packet) {
}

//trigger when popup-window inited in call mode AUTO-POPUP
azstack.onVideoCallPopupInit = function() {
}

//trigger when popup-window destroyed in call mode AUTO-POPUP
azstack.onVideoCallPopupDestroy = function(packet) {
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

> userId: userId of user

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

> azStackUserID: azStackUserID of user

> callback: after request to server and get user data, function will be called

# 13. azSendMessage:
Send 1 message text chat 1 - 1
```javascript
azstack.azSendMessage(msgContent, azStackUserID, msgId, callback);
```
Parameters:

> msgContent: content of message

> azStackUserID: azStackUserID of user who receive message

> msgId: Id of message must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called

# 14. azSendMessageFileUrl:
Send 1 message picture/audio/video/file chat 1 - 1
```javascript
azstack.azSendMessageFileUrl(url, fileName, msgType, azStackUserID, msgId, fileLength, width, height, duration, callback);
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

> duration: thời gian của message (voice message)

> callback: after request to server and get data, function will be called

# 15. azSendMessageSticker:
Send message sticker chat 1 - 1
```javascript
azstack.azSendMessageSticker(azStackUserID, imgName, catId, msgId, url, width, height, callback);
```
Parameters:

> azStackUserID: azStackUserID of user who receive chat message

> imgName: name of sticker

> catId: category of sticker

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of sticker

> width: width of sticker

> height: height of sticker

> callback: after request to server and get data, function will be called

# 16. azSendMessageGroup:
Send one message text chat group
```javascript
azstack.azSendMessageGroup(msgContent, groupId, msgId, callback);
```
Parameters:

> msgContent: content of message

> groupId: groupId of group that receive message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called

# 17. azSendMessageFileUrlGroup:
Send one message ảnh/audio/video/file chat group
```javascript
azstack.azSendMessageFileUrlGroup(groupId, msgId, url, msgType, fileName, fileLength, width, height, duration, callback);
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

> duration: thời gian của message (voice message)

> callback: after request to server and get data, function will be called

# 18. azSendMessageStickerGroup:
Send one message sticker chat group
```javascript
azstack.azSendMessageStickerGroup(groupId, imgName, catId, msgId, url, width, height, callback);
```
Parameters:

> groupId: groupId of group that receive message

> imgName: name sticker

> catId: category of sticker

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> url: file path of sticcker

> width: width of sticker

> height: height of sticker

> callback: after request to server and get data, function will be called

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
azstack.azListFiles(lastUpdate, chatType, chatId);
```
Parameters:

> lastUpdate: Only get files that have updated after this time (miliseconds)

> chatType: optional, chat type of conversation which getting files

> chatId: optional, chat id of conversation which getting files

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
azstack.azMessageBroadCast(broadcast, msg, msgId, callback);
```
Parameters:

> broadcast: Id of broadcast

> msg: content of message

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after sent message, this function will be triggered

# 39. azMessageStickerBroadCast:
Send sticker message to broadcast
```javascript
azstack.azMessageStickerBroadCast(broadcast, msgId, imgName, catId, url, width, height, callback);
```
Parameters:

> broadcast: Id of broadcast

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> imgName: name sticker

> catId: category of sticker

> url: file path of sticker

> width: width of sticker

> height: height of sticker

> callback: after sent message, this function will be triggered

# 40. azMessageFileBroadCast:
Send one message ảnh/audio/video/file chat broadcast
```javascript
azstack.azMessageFileBroadCast(broadcast, msgId, type, url, fileName, fileLength, width, height, callback);
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

> callback: after sent message, this function will be triggered

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
Parameters:

# 45. azGetListModifiedConversationsPage:
Lấy danh sách conversation sau khi authen thành công theo page, option callback:
```javascript
azstack.azGetListModifiedConversationsPage(page, lastCreated, callback);
```
Parameters:

> page: Lấy danh sách theo page

> lastCreated: only get list of conversations that have created after [lastCreated] (tính bằng miliseconds)

> callback: after request to server and get user data, function will be called

# 46. azGetListModifiedMessagesPage:
Get list of message from 1 conversation theo page, option callback:
```javascript
azstack.azGetListModifiedMessagesPage(page, lastCreated, type, chatId, callback);
```
Parameters:

> page: Lấy danh sách theo page

> lastCreated: only get list of messages that have created after [lastCreated] (tính bằng miliseconds)

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

> callback: after request to server and get user data, function will be called

# 47. azGetListUnreadMessagesPage:
Get list of unread message from 1 conversation theo page, option callback:
```javascript
azstack.azGetListUnreadMessagesPage(page, type, chatId, callback);
```
Parameters:

> page: Lấy danh sách theo page

> type: type = 1 (chat 1 - 1), type = 2 (chat group)

> chatId: userId of user or Id of group

> callback: after request to server and get user data, function will be called

# 48. azStartVideoCall:
Start a call to a user with option video or voice call:
```javascript
azstack.azStartVideoCall(toChatId, callId, localVideoId, remoteVideoId, hasVideo);
```
Parameters:

> toChatId: id của user cuộc gọi thực hiện đến

> callId: 1 số độc nhất không lặp lại

> localVideoId: id của thẻ video/audio hiện hình ảnh / tiếng nói của mình

> remoteVideoId: id của thẻ video/audio hiện thị hình ảnh / tiếng nói của mục tiêu cuộc gọi hướng đến

> hasVideo: true/false ứng với đây là cuộc gọi video/audio

# 49. azAcceptVideoCall:
Accept to anwser a call (MANNUALLY mode only):
```javascript
azstack.azAcceptVideoCall(fromChatId, callId, localVideoId, remoteVideoId, hasVideo);
```
Parameters:

> fromChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

> localVideoId: id của thẻ video/audio hiện hình ảnh / tiếng nói của mình

> remoteVideoId: id của thẻ video/audio hiện thị hình ảnh / tiếng nói của mục tiêu thực hiện cuộc gọi

> hasVideo: true/false ứng với đây là cuộc gọi video/audio, nhận được trong event onInviteVideoCall

# 50. azRejectVideoCall:
Reject to answer a call (MANNUALLY mode only):
```javascript
azstack.azRejectVideoCall(fromChatId, callId);
```
Parameters:

> fromChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

# 51. azStopVideoCall:
Stop a call (MANNUALLY mode only):
```javascript
azstack.azStopVideoCall(fromChatId, callId);
```
Parameters:

> fromChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

# 52. azNotAnsweredVideoCall:
Emit a call is not answered (MANNUALLY mode only):
```javascript
azstack.azNotAnsweredVideoCall(toChatId, callId);
```
Parameters:

> toChatId: id của user cuộc gọi thực hiện đến

> callId: id của cuộc gọi nhận được trong event onInviteVideoCall

# 53. azStartCallOut:
Start a call to a user with option video or voice call:
```javascript
azstack.azStartCallOut(toPhoneNumber, callId, localVideoId, remoteVideoId);
```
Parameters:

> toPhoneNumber: số điện thoại thực hiện cuộc gọi đến

> callId: 1 số độc nhất không lặp lại

> localVideoId: id của thẻ audio hiện thị tiếng nói của mình

> remoteVideoId: id của thẻ audio hiện thị tiếng nói của mục tiêu cuộc gọi hướng đến

# 54. azStopCallOut:
Stop a call (MANNUALLY mode only):
```javascript
azstack.azStopCallOut(toPhoneNumber, callId);
```
Parameters:

> toPhoneNumber: số điện thoại thực hiện cuộc gọi đến

> callId: id của cuộc gọi

# 55. azAcceptCallIn:
accept a call (MANNUALLY mode only):
```javascript
azstack.azAcceptCallIn(destination, callId, localVideoId, remoteVideoId);
```
Parameters:

> destination: số điện thoại của cuộc gọi đến

> callId: callId của cuộc gọi gọi đến

> localVideoId: id của thẻ audio hiện thị tiếng nói của mình

> remoteVideoId: id của thẻ audio hiện thị tiếng nói của mục tiêu cuộc gọi hướng đến

# 56. azRejectCallin:
reject a call (MANNUALLY mode only):
```javascript
azstack.azRejectCallin(destination, callId);
```
Parameters:

> destination: số điện thoại của cuộc gọi đến

> callId: callId của cuộc gọi gọi đến

# 57. azStopCallin:
stop a call (MANNUALLY mode only):
```javascript
azstack.azStopCallin(destination, callId);
```
Parameters:

> destination: số điện thoại của cuộc gọi đến

> callId: callId của cuộc gọi gọi đến

# 58. azNotAnsweredCallin:
emit not answer a call (MANNUALLY mode only):
```javascript
azstack.azNotAnsweredCallin(destination, callId);
```
Parameters:

> destination: số điện thoại của cuộc gọi đến

> callId: callId của cuộc gọi gọi đến

# 59. toggleVideoState (MANNUALLY mode only):
toggle local video with optional param state
```javascript
azstack.azWebRTC.toggleVideoState(state);
```
Parameters:

> state: true/false ứng với trạng thái video bật/tắt, nếu không có biến này, thay đổi trạng thái hiện tại của video tắt -> bật, bật -> tắt

# 60. toggleAudioState (MANNUALLY mode only):
toggle local audio with optional param state
```javascript
azstack.azWebRTC.toggleAudioState(state);
```
Parameters:

> state: true/false ứng với trạng thái audio bật/tắt, nếu không có biến này, thay đổi trạng thái hiện tại của audio tắt -> bật, bật -> tắt

# 61. updateUserInfoWithCallBack:
cập nhật thông tin user theo userId có callback
```javascript
azstack.updateUserInfoWithCallBack(userId, callback);
```
Parameters:

> userId: user Id của user

> callback: after request to server and get user data, function will be called

# 62. updateUserInfoByUsernameWithCallBack:
cập nhật thông tin user theo azStackUserID có callback
```javascript
azstack.updateUserInfoByUsernameWithCallBack(azStackUserID, callback);
```
Parameters:

> azStackUserID: azStackUserID của user

> callback: after request to server and get user data, function will be called

# 63. azGetBroadcastInfo:
Get broadcast information from server
```javascript
azstack.azGetBroadcastInfo(broadcastId, callback);
```
Parameters:

> broadcastId: Id of broadcast

> callback: after receive broadcast information, this function will be triggered

# 64. azLeaveGroupAndChangeAdmin:
Rời nhóm và đổi admin của nhóm
```javascript
azstack.azLeaveGroupAndChangeAdmin(leaveUser, newAdmin, group, msgId, callback);
```
Parameters:

> leaveUser: User id của người rời nhóm

> newAdmin: User id của admin mới muốn chuyển đến

> group: Id của group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> callback: hàm thực hiện sau khi rời nhóm thành công

# 65. azChargingGetBalance:
Lấy thông tin balace của user
```javascript
azstack.azChargingGetBalance(callback);
```
Parameters:

> callback: hàm thực hiện sau có dữ liệu trả về

# 66. getUserInfoByListUsernameAndRequestToServerWithCallBack:
Get information of user by list azStackUserID with callback
```javascript
azstack.getUserInfoByListUsernameAndRequestToServerWithCallBack(listAzStackUserID, callback);
```
Parameters:

> listAzStackUserID: listAzStackUserID of user

> callback: after request to server and get user data, function will be called

# 67. getUserInfoByListUsernameAndRequestToServer:
Get information of user by list azStackUserID
```javascript
azstack.getUserInfoByListUsernameAndRequestToServer(listAzStackUserID);
```
Parameters:

> listAzStackUserID: listAzStackUserID of user

# 68. updateUserInfoByListUsernameWithCallBack:
cập nhật thông tin user theo list azStackUserID có callback
```javascript
azstack.updateUserInfoByListUsernameWithCallBack(listAzStackUserID, callback);
```
Parameters:

> listAzStackUserID: listAzStackUserID của user

> callback: after request to server and get user data, function will be called

# 69. azSendMessageLocation:
Send 1 message location chat 1 - 1
```javascript
azstack.azSendMessageLocation(long, lat, addr, azStackUserID, msgId, callback);
```
Parameters:

> long: địa chỉ long của location

> lat: địa chỉ lat của location

> addr: địa chỉ của location

> azStackUserID: azStackUserID of user who receive message

> msgId: Id of message must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called

# 70. azSendMessageLocationGroup:
Send 1 message location chat group
```javascript
azstack.azSendMessageLocationGroup(groupId, groupName, long, lat, addr, msgId, callback);
```
Parameters:

> groupId: id của group

> groupName: tên của group

> long: địa chỉ long của location

> lat: địa chỉ lat của location

> addr: địa chỉ của location

> msgId: Id of message must be unique related to conversation and sender. Should be: currentMsgId++

> callback: after request to server and get data, function will be called

# 71. azMakeGroupWithTag:
Create group
```javascript
azstack.azMakeGroupWithTag(members, name, msgId, type, tag, callback);
```
Parameters:

> members: array userId of user is member in group

> name: name of group

> msgId: Id of message, must be unique related to conversation and sender. Should be: currentMsgId++

> type: type = 0 (group private), type = 1 (group public / channel)

> tag: tag of group

> callback: after created group, this function will be triggered

# 72. azGetListModifiedConversationsByPageAndTag:
Get list conversation with page and tag, option callback:
```javascript
azstack.azGetListModifiedConversationsByPageAndTag(type, page, lastCreated, tag, callback);
```
Parameters:

> type: type of conversations; 0: all, 1: one one, 2: group

> page: Lấy danh sách theo page

> lastCreated: only get list of conversations that have created after [lastCreated] (tính bằng miliseconds)

> tag: tag of conversations

> callback: after request to server and got conversations list, function will be called

# 73. azPushCustomizeEvent:
push customize event to selected users:
```javascript
azstack.azPushCustomizeEvent(eventType, eventData, users);
```
Parameters:

> eventType: any integer number

> eventData: any object/array of data

> users: string of userId will be received push, separate by comma ','

# 74. azJoinConferenceChat:
Join conference chat with groupId
```javascript
azstack.azJoinConferenceChat(groupId, callback);
```
Parameters:

> groupId: id og group

> callback: after request to server and get result, function will be called