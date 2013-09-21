HTTPAudioStreamer
========

This should actually be titled HTTPAudioPlayer. D:

My implementation of an Audio player over HTTP. Uses AVFoundation's AVAudioPlayer and my own simple HTTPFileSaver to accomplish this.

# How to use

```objc
//initialize the player
NSURL *httpURL = [NSURL URLWithString:@"http://example.com/somefile.mp3"];
HTTPAudioPlayer *player = [[HTTPAudioPlayer alloc] initWithURL:httpURL];
player.delegate = someDelegate;

//set the URL to save it to in a few ways
NSURL *someURL;
player.fileSaver.localURL = someURL;
player.fileSaver.documentsPath = @"test.mp3"; //localURL = ~mobile/Applicaitons/APP-FOLDER/Documents/XXX

//download and play!
[player download];
[player play];

//use these to pause and stop
[player pause];
[player stop];

//otherwise go ahead and call them on the AVAudioPlayer itself
player.audioPlayer.volume = 0.5;

```

## Delegate methods

```objc
-(void)audioPlayerDidStartBuffering:(HTTPAudioPlayer *)audioPlayer;
-(BOOL)audioPlayerDidFinishBuffering:(HTTPAudioPlayer *)audioPlayer; //return true to continue playing, return false to not, if not implemented will assume true
-(void)audioPlayerDidFinishPlaying:(HTTPAudioPlayer *)audioPlayer;
-(void)audioPlayerFailedToPlay:(HTTPAudioPlayer *)audioPlayer;

-(void)audioPlayerDidFinishDownloading:(HTTPAudioPlayer *)audioPlayer;
-(void)audioPlayerDownloadFailed:(HTTPAudioPlayer *)audioPlayer;

```

## HTTPFileSaver

You can also use methods on HTTPAudioPlayer's HTTPFileSaver to manipulate the download. For example:

```objc
HTTPAudioPlayer *player;

if(secondsSinceLastPacketRecieved > 20 && player.fileSaver.downloading)
{
    //cancels download and deletes local file
    [player.fileSaver deleteLocalFile];
}

```

There's other stuff you can do, too. Take a look at the header files.

# License

This uses an MIT license. You can read it at the stop of the source code of any of the files.

# What this is compatible with

The only thing I've tested this on is on a jailbroken iOS 6.0 using the iOS 7.0 SDK (built for iOS5+). So it should be compatible with iOS 5+.

Also, I'm like 99% sure this won't work on simulator. Only on the device.
