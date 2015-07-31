# ios-pushkit
Qwasi iOS Pushkit (deprecated)

![Build Status](https://build.qwasi.com/plugins/servlet/wittified/build-status/IOSSDK-DEV)
![Github Deployment](https://build.qwasi.com/plugins/servlet/wittified/deploy-status/15761410)

```
//
//  AppDelegate.m
//  AcmePush
//
//  Created by Rob Rodriguez on 4/3/14.
//  Copyright (c) 2014 Qwasi Technology. All rights reserved.
//

#import "AppDelegate.h"

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
#if DEBUG
    [NotificationManager shared].debug = YES;
#endif
    
    [NotificationManager shared].delegate = self;
    [NotificationManager shared].enabled = YES;

    return YES;
}

/// MARK: - Qwasi Delegate
- (void)notificationManager:(NotificationManager *)manager didRegisterDevice:(NSString *)deviceId {

    NSLog(@"Device has been registered %@", deviceId);
    
    [NotificationManager shared].userToken = @"MyUserToken";
    [NotificationManager shared].pushEnabled = YES;
    
    [[NotificationManager shared] postEvent: @"loggedIn" withData: nil];
    [[NotificationManager shared] subscribeToChannels: @[@"auth_users"]];
    
    [LocationManager shared].delegate = self;
    [LocationManager shared].locationMode = PKLocationModeDefaultBackground;
    [LocationManager shared].enabled = YES;
}

#pragma mark - NotificationManagerDelegate
- (void)notificationManager:(NotificationManager *)manager didFailToRegisterDevice:(NSError *)err {
    NSLog(@"Device registration failed %@", err);
}

- (void)notificationManager:(NotificationManager *)manager didRegisterForPushNotifications:(NSString *)pushToken {
    NSLog(@"Device has registered for push with token %@", pushToken);
    
    [manager pollForNotifications];
}

- (void)notificationManager:(NotificationManager *)manager didFailToRegisterForPushNotifications:(NSError *)err {
    NSLog(@"Push registration failed %@", err);
}

- (void)notificationManager:(NotificationManager *)manager didReceiveNotification:(PKNotification *)note {
    NSLog(@"Got a message %@", note);
}

- (void)notificationManager:(NotificationManager *)manager didFailToReceiveNotification:(NSError *)err {
    NSLog(@"Message fetch failed %@", err);
}

- (void)notificationManager:(NotificationManager *)manager didSubscribeToChannels:(NSArray *)channels {
    NSLog(@"Subcribed to channels: %@ --> %@", channels, manager.channels);
}

#pragma mark - LocationManagerDelegate
- (void)locationManager:(LocationManager *)manager didUpdateLocation:(CLLocation *)location {
    //NSLog(@"Location upate: %@", location);
}

- (void)locationManager:(LocationManager *)manager didEnterPosition:(PKPosition *)pos {
    NSLog(@"Location enter: %@", pos);
}

- (void)locationManager:(LocationManager *)manager didExitPosition:(PKPosition *)pos {
    NSLog(@"Location exit: %@", pos);
}

- (void)locationManager:(LocationManager *)manager didDwellInPosition:(PKPosition *)pos dwellTime:(NSTimeInterval)dwellTime {
    NSLog(@"Location dwell: %@ %fs", pos, dwellTime);
}
@end
```
