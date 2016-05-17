# Module-MaxIM-iOS

##使用说明：
###一、设置appid和clientkey
1、在maxleap.cn中创建app，记录appid和clientkey。

2、更换AppDelegate.m中的宏定义为1中的appid和clientkey：

    #define MAXLEAP_APPID           @"your_app_id"
    #define MAXLEAP_CLIENTKEY       @"your_client_key"

3、如果要使用微博、QQ和微信第三方登录，需要先在相应的开发者后台注册app，输入正确的app信息。

4、更换AppDelegate.m中的以下宏定义：

    #define WECHAT_APPID            @"your_wechat_appid"
    #define WECHAT_SECRET           @"your_wechat_secret"
    #define WEIBO_APPKEY            @"your_weibo_appkey"
    #define WEIBO_REDIRECTURL       @"your_weibo_redirect_url"
    #define QQ_APPID                @"your_qq_appid"

5、更新Info.plist文件中URL Types中以上第三方登陆需要的设置。

###二、ViewController介绍
1、MCPersonalViewController： 在未登录情况下显示登录和注册按钮，登录后显示和更新个人信息。

2、MCContactsViewController：显示IM联系人和群组列表，点击进入聊天。

3、MCRecentChatsViewController：最近聊天列表。

4、MCMessagesViewController：联系人或者群组聊天界面，

###三、Modal介绍
1、MaxChatIMClient：负责创建MLIMClient，创建聊天窗口，接收消息并发送到聊天窗口。


	@interface MaxChatIMClient : NSObject
	
	+ (MaxChatIMClient*)sharedInstance;
	
	@property (nonatomic, strong) MLIMClient *client;
	
	// 最近聊天： MLIMFriendInfo or MLIMGroup
	@property (nonatomic, strong) NSMutableArray *recentChats;
	
	- (void)loginWithCurrentuserCompletion:(MLIMBooleanResultBlock)completion;
	
	- (void)pushMessagesControllerForFriend:(MLIMFriendInfo *)aFriend withNavigator:(UINavigationController *)navigator;
	- (void)pushMessagesControllerForGroup:(MLIMGroup *)aGroup withNavigator:(UINavigationController *)navigator;
	@end

2、MCMessagesModelData：用于给JSQMessagesViewController提供数据，并完成MLIMClient消息的实时收发。

	
	@interface MCMessagesModelData : NSObject
	
	@property (nonatomic, weak) JSQMessagesViewController *messagesController;
	
	@property (strong, nonatomic) NSMutableArray<JSQMessage*> *messages;
	@property (strong, nonatomic) NSMutableDictionary *avatars;
	
	@property (strong, nonatomic) JSQMessagesBubbleImage *outgoingBubbleImageData;
	@property (strong, nonatomic) JSQMessagesBubbleImage *incomingBubbleImageData;
	
	- (id)init NS_UNAVAILABLE;
	- (id)initWithFriend:(MLIMFriendInfo*)aFriend;
	- (id)initWithGroup:(MLIMGroup*)aGroup;
	
	- (void)sendMessage:(JSQMessage *)message;
	- (void)receiveMessage:(MLIMMessage *)obj;
	
	// cache url
	+ (NSURL *)cacheURLForMediaURL: (NSString *)attachmentUrl extension:(NSString * )extension;
	
	// message utilities
	+ (JSQMessage *)createVideoMediaMessageWithURL:(NSURL *)mediaURL;
	+ (JSQMessage *)createAudioMediaMessageWithURL:(NSURL *)mediaURL;
	+ (JSQMessage *)createPhotoMediaMessageWithImage:(UIImage *)image;
	+ (JSQMessage *)createLocationMediaMessageCompletion:(JSQLocationMediaItemCompletionBlock)completion;
	+ (JSQMessage *)createVideoMediaMessage;
	+ (JSQMessage *)createAudioMediaMessage;
	+ (JSQMessage *)createPhotoMediaMessage;
	
	
	// video utilities
	+ (BOOL) isVideoPortrait:(AVAsset *)asset;
	+ (void)cropVideo:(AVAsset *)asset toUrl:(NSURL *)outputURL renderSize:(CGSize)renderSize start:(NSTimeInterval)start duration:(NSTimeInterval)duration presetName:(NSString *)presetName completion:(void(^)())completion;
	+ (void)cropVideo:(AVAsset *)asset toUrl:(NSURL *)outputURL renderSizeScale:(CGSize)renderSizeScale start:(NSTimeInterval)start duration:(NSTimeInterval)duration presetName:(NSString *)presetName completion:(void(^)())completion;
	+ (void)cropVideo:(AVAsset *)asset toUrl:(NSURL *)outputURL toSquareWithSide:(CGFloat)sideLength start:(NSTimeInterval)start duration:(NSTimeInterval)duration presetName:(NSString *)presetName completion:(void(^)())completion;
	+ (void)cropVideo:(AVAsset *)asset toUrl:(NSURL *)outputURL toSquareWithScale:(CGFloat)scale start:(NSTimeInterval)start duration:(NSTimeInterval)duration presetName:(NSString *)presetName completion:(void(^)())completion;
	@end

