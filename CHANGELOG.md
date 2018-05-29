# Changelog

A history of releases.

### v2.0.0 (Date TBD)
***
#### BREAKING
- Full retrofit of the SoundJS architecture. Web Audio is now the main focus, and plugins have been removed. New classes to assist in sound management.
- Change in fallback method: If a url with a file extension is provided, SoundJS will try to play that file extension even if the browser reports a 'maybe' on whether it supports that file type. Providing a URL without a file extension will go down the fallbacks list, like the old behaviour.

### v1.0.0 (September 14, 2017)
***
##### CRITICAL
- Removed deprecated properties, particularly getValue/setValue properties, in favour of .value this particularly affects instance and global properties like mute, volume, pan, etc.
- Removed deprecated TYPE duplicates, notably the loader types (LoadQueue.SOUND, AbstractLoader.SOUND) in place of the Types.SOUND, and request methods (LoadQueue.POST, AbstractLoader.POST) in place of Methods.POST.
- Deprecated get/set methods are now protect with an underscore (eg _setEnabled) The deprecated methods and properties will still work, but will display a console warning when used.
- Sound.play is no longer overloaded, and only takes a source and an initialization object (or PlayPropsConfig) instead of a bunch of arguments. A console warning will be displayed if  the old approach is used, and additional arguments will be ignored.
- Changed version naming to use soundjs.js, instead of containing the version number

##### MINOR
- Changed XHR error conditions to only include 400-599. Specifically removed status=0 as an error, which affects compiled applications.
- Fixed issue where values don't get appended to GET requests in XHRRequest
- Added "touchstart" as an audio unlocking event, which seems to be the best for iOS
- Checked if preload candidates are videos, since SoundJS mistakenly handles these when they are mp4.
- Added missing Methods and Types classes from recent PreloadJS changes
- Handled case where audio duration returned < 0, resulting in a DOM Error
- Fixed issue where new instances wouldn't get the global mute/volume (thanks @PythonFanboy)
- Added a shared createjs.deprecate() method, which wraps old methods and property getter/setters to display a console warning when used.
- Fixed an issue with the initial 1.0.0 build that used a deprecated property that was improperly removed


### v0.6.2 [November 26, 2015]
***
##### CRITICAL
- Fixed pan issue introduced in Chrome 44, where undefined pan defaulted to left channel only

##### MINOR
- Fixed an issue in HTMLAudioPlugin that prevented SoundInstances created before loading was complete from playing (github issue 167)
- Additional cleanup when setting duration
- Documentation and README updates
- Added a reuseable scratch buffer, instead of recreating each time (thanks @konistehrad/@naranjamcanica)
- Added automatic "playEmptySound" support on iOS to unlock the WebAudio context
- Bower updates: Removed bower.json from exclusions


### v0.6.1-HOTFIX [October 8, 2015]
***
##### CRITICAL
- Fixed issue with panning in Chrome, caused by an unset pan value in WebAudio. This minor fix is provided to fix existing content online that was broken with a browser update, and has been patched on the CDN
 If you need to use 0.6.1, there is a 0.6.1-HOTFIX branch in GitHub. Otherwise use 0.6.2 or newer.


### v0.6.1 [May 21,2015]
***
##### CRITICAL
- Removed support for ie8 and deprecated related get/set methods

##### MINOR
- Fixed typo in HTMLAudioSoundInstance for this._stalledHandler from this.playFailed to this._playFailed
- Fixed a reference in WebAudioLoader from this._handleError to this._sendError
- Updated HTMLAudio loading to properly dispatch error events
- Fixed issue with reloading canceled audio when used with PreloadJS (github issue 143)
- Fixed issue that would break loaded audio if you registered it a 2nd time (github issue 148)
- Added AudioSprite class to document audiosprite more clearly
- Added muted and volume properties to Sound, and deprecated get/setMute and get/setVolume
- Added capabilities property to Sound, and deprecated getCapabilities and getCapability
- Refactor HTMLAudioTagPool as Chrome no longer requires tags be pre-created
- Deprecate HTMLAudioPlugin defaultNumChannels and cleaned up associated code
- Fixed github issue 154, FlashAudioPlugin not properly setting volume and pan on play calls
- Fixed FlashAudioSoundInstance bug that did not change loop value properly on playing instances
- Fixed registerSounds to support an object file with a path and manifest property (mimics preloadjs option)
- Added support for alternate loading paths and for loading files without extensions
  via loading objects where src is an object that contains extension properties along with paths
  example: {id:"music", src:{mp3:"myMP3Path/music.mp3", ogg:"myOggPath/musicWithoutExtension"}}
- Fixed setting duration on an existing SoundInstance in HTMLAudioSoundInstance
- Fixed setting duration on an existing SoundInstance in WebAudioPlugin
- Added SoundInstance.startTime support
- Added PlayPropsConfig class which is now used in all play calls.  Deprecated passing individual properties
- Added startTime and duration handling to SoundInstance.play when passed in an Object or PlayPropsConfig
- Added applyPlayProps to AbstractSoundInstance, to apply multiple properties at once
- Added defaultPlayProps to registerSound and registerSounds, allowing the setting of default play properties that will be applied to each new instance of a given src
  example: {id: "Humm (mp3)", src: "Humm.mp3", defaultPlayProps:{interrupt:createjs.Sound.INTERRUPT_ANY, loop:-1, offset: 2000}}
- Added get/setDefaultPlayProps to Sound, allowing the retrieval and setting of default play properties that will be applied to each new instance of a given src
- Fix HTMLAudioPlugin.capabilities.panning value to be false
- Fix HTMLAudioPlugin.updateStartTime to correct HTMLAudioPlugin._updateStartTime
- Added CordovaAudioPlugin, for use with Cordova based apps including PhoneGap and Ionic
- Fix Sound.loadComplete throwing errors for src that has not been loaded instead of expected false
- Fix missing ._volume property on AbstractPlugin

### v0.6.0 [December 12, 2014]
***
*Please note SoundJS 0.6.0 is only compatible with PreloadJS 0.6.0 and later. Earlier versions are incompatible.*

##### CRITICAL
- Removed preload property from Sound.registerSound and Sound.registerManifest, as it is only used by PreloadJS
- Changed WebAudioPlugin dynamicsCompressorNode and gainNode to be instance level rather than static class level
  this is now accessed via createjs.Sound.activePlugin.dynamicsCompressorNode rather than
  createjs.WebAudioPlugin.dynamicsCompressorNode
- Removed iOS disabling by default on HTMLAudioPlugin
- SoundInstance stop, setVolume, setPan, setPosition now return a reference to the instance, instead of a Boolean, which is useful for chaining calls.
- Deprecated SoundInstance pause/resume methods, please use SoundInstance.paused property instead.
- Renamed FlashPlugin to FlashAudioPlugin, deprecated FlashPlugin for next release, including the VERSION file.
- Removed deprecated code and cleaned up supporting code for deprecated "|" loading approach
- Re-architected the class and inheritance model
 - initialize methods removed, use MySuperClass_constructor instead
 - helper methods: extend & promote (see the "Utility Methods" docs)
 - property definitions are now instance level, and in the constructor (performance gains)
 - the .constructor is now set correctly on all classes (thanks kaesve)

##### MINOR
- Added constructors to all non-static classes (Thanks kaesve)
- Fixed an issue that caused interrupt early/late to pick a playing sound over an finished sound when all instances are used
- Added new event methods on and off to defaultSoundInstance to prevent errors
- Fixed issue with windows phone reporting as iPhone
- Fixed an issue with WebAudioPlugin that was not passing the correct parameter for disconnecting nodes
- Added audio sprite support and documentation
- Added AudioSprite.html to examples
- Fixed an issue with HTMLAudioPlugin that was not removing tags from DOM when removing src from SoundJS
- Altered HTMLAudioPlugin so that SoundInstance will have duration immediately on creation
- Updated removeAllSounds to check for activePlugin before calling to prevent errors
- Corrected error display wording in MusicVisualizer example
- Fixed HTMLAudioPlugin typo on SoundInstance.src declaration
- Fixed WebAudioPlugin bug, sourceNode was not being properly set to null on pause and setPosition
- Added readonly paused property to SoundInstance
- Added Bower support, including Grunt task for automatically updating the bower.json
- Added .gitignore to subfolders under /docs (thanks mcfarljw)
- Improved EventDispatcher's handling of redispatched event objects
- Changed behavior of SoundInstance.play to be consistent across all plugins.
  - The new behavior is the following:
    - if playing, keep playing (aka do nothing)
    - if paused, resume
    - if stopped, start
  - Note this will still accept and apply play call arguments, so you can change offset, volume, etc
- Changed SoundInstance.play to handle all arguments on playing, paused, and stopped instances
- Added public loop property to SoundInstance
- Fixed an issue with FlashAudioPlugin that would set position to an incorrect value when pausing
- Changed FlashAudioPlugin to grab the duration of audio as soon as a SoundInstance is created
- Fixed progress event of WebAudioPlugin Loader, for use with PreloadJS
- Implemented new base classes for Plugin, SoundInstance, and SoundLoader.
- SoundInstance added properties duration, position, loop, mute, pause (with backup methods for browser that do not support getter/setters)
- SoundInstance added method destroy
- Deprecated registerManifest and removeManifest in favor of registerSounds and removeSounds
- Moved BrowserDetect into a util and changed namespace to createjs.BrowserDetect
- Updated documentation to include abstract classes and all plugin classes.
- Added support for setting WebAudioPlugin.context.  Note that it needs to be set before plugins are instantiated.
- Added fileerror event, dispatched when internal loading fails.
- RegisterSounds & removeSounds now handles a path property in the passed in manifest of sounds.


### v0.5.2 [December 12, 2013]
***
##### MINOR
- Updated tutorials for changes in this build
- Updates and fixes to all docs
- Added {"AllowScriptAccess" : "always"} to swfObject for FlashPlugin
- All Sound, WebAudioPlugin, HTMLAudioPlugin, FlashPlugin, and SoundInstance internal properties and methods renamed to start with _ (underscore)
- Fixed a bug with default SoundInstance, it did not have playFailed function that is called by Sound
- Fixed a bug that prevented interrupt value from being read in play call if it was passed in an object
- Changed SoundInstance to extend createjs.EventDispatcher rather than mix in
- Alterations to basePath approach that require full src (basePath + src) in create and play calls
- Include basePath in removeSound and removeManifest, which is now required if it was included in loading
- Introduced createjs.Sound.alternateExtensions, which is replacing a delimited list as a means to load alternate file types
- Deprecated "|" approach to alternate files, in favor of class level alternateExtensions approach
- Deprecated registerPlugin in favor of registerPlugins with a single argument
- Deprecated FlashPlugin BASE_PATH in favor swfPath
- Added console logs deprecated calls above are used
- Added willTrigger() method to EventDispatcher


### v0.5.1 [November 15, 2013]
***
##### MINOR
- Suppressing errors in WebAudioPlugin and HTMLAudioPlugin in old browsers that do not properly support object.defineProperty
- Changes to WebAudioPlugin to allow it to work with latest working draft of Web Audio API
- WebAudioPlugin changed SoundInstance node order to SourceNode -> PanNode -> GainNode --> context.destination to get
  around firefox bug https://bugzilla.mozilla.org/show_bug.cgi?id=933304


### v0.5.0 [September 25, 2013]
***
##### CRITICAL
- Removed all onEvent handlers (ex. onClick, onTick, onAnimationEnd, etc)
- Updated EventDispatcher with latest bubbling model, and the Event class

##### MINOR
- Altered all libraries to use defined object properties instead of object literal notation.
- Namespaced all sub apis to related plugin, ie createjs.WebAudioPlugin.SoundInstance
- Implemented createjs Utils
- Implemented "use strict" mode
- Removed deprecated methods and properties, doc'd as removed.
- Updated WebAudioPlugin to handle new calls and deprecated calls (http://www.w3.org/TR/webaudio/#DeprecationNotes).
- Added enableIOS property to HTMLAudioPlugin,allowing advanced users to enable HTMLAudioPlugin on iOS (not recommended).
- Overloaded play call in Sound and SoundInstance to allow options to be passed in as an object, ie play("music", {loop: -1, volume: 0.5})
- Changed WebAudioPlugin to test if XHR is available for local files rather than assuming it is not.
- Implement basePath support for local loading and with PreloadJS
- Updated registerSound and registerManifest to return true if a source has already been loaded.
- Added getter/setter to volume and pan of SoundInstance, to allow tweening.
- Added polyfil for Array.indexOf to allow IE8 support
- Fixed an issue with Sound.removeSound not stopping audio with that src.
- Fixed an issue with pause and setPosition on audio files that are not looping, which would generate an error in WebAudioPlugin.
- Fixed a bug with WebAudioPlugin that stopped SoundInstances from being reusable.
- Fixed an issue with EventDispatcher when adding the same listener to an event twice
- Updated the build process to use NodeJS & Grunt.js. Please refer to the readme in the build folder.


### v0.4.1 [May 10, 2013]
***
##### MINOR
- Added removeSound, removeManifest, and removeAllSounds functions to Sound, to enable unloading of sounds.
- Added MobileSafe demo to show launching an "app" inside a touch event, enabling audio playback on mobile devices
- Added playEmptySound() method, which facilitates playback on mobile devices without user interaction
- HTMLAudioPlugin now using tag loop property to provide more reliable looping
- WebAudioPlugin added a look ahead approach to enable smooth looping
- Updated WebAudioPlugin to use a specific panning model (equal power). Matches the sound quality of the other plugins
- Fixed an issue that stopped WebAudioPlugin from loading sound files other than ogg, mp3, and wav with PreloadJS
- Fixed playback for secondary sounds in HTMLAudio Plugin that affected Firefox
- Fixed an issue with duration for secondary sounds in HTMLAudioPlugin in Firefox
- Fixed issue where failed or interrupted sound channels could not be used, resulting in less instances available then expected.
- Updated file validation RegExp. Supports double-byte characters, prevents partial matches, better support for relative paths, improved matching of domains, and modified the "file name" match to include the extension (file.mp3 instead of file). The match arguments have not changed otherwise.
- Added support for PhoneGap in WebAudioPlugin that allows it to be used on mobile devices
- Fixed naming of the fileload event (used to be "loadComplete"). The old event is still dispatched, but is deprecated
- Renamed the internal "sendLoadComplete" method to "sendFileLoadEvent"
- Fixed cleanup routine to happen before the Sound and instance events are dispatched, which caused issues if affecting the sound immediately after receiving the event.
- Corrected missing namespaces in SoundInstance overview
- Fixed playing sounds by ID that are preloaded using the Sound.registerSound() method
- Fixed path parsing in the createInstance method (thanks maljub01)
- Moved embedded SWF into a container, and positioned it off-screen
- Fixed documentation:
  - FlashPlugin: Main description caused FlashPlugin to not export correct documentation
  - SoundInstance/setMute was documented as a duplicate of mute
  - SoundInstance main example had incorrect event for "failed"
  - Added code samples throughout
  - Minor updates throughout documentation to formatting, wording, parameters, and private/protected
- Updated browser limitations:
  - iOS6: include info on web audio distortion bug when video element is present
  - Android: issues with HTML audio in Android Chrome
  - Flash: Audio delays when using the FlashPlugin
  - IE9: Information on the audio tag limit.
  - Safari: added info about requiring Quicktime for audio playback.


### v0.4.0 [Feb 12, 2013]
***
*Please note PreloadJS 0.3.0 requires SoundJS 0.4.0 to preload audio. Earlier versions are incompatible.*
##### MINOR
- Class name change to createjs.Sound from createjs.SoundJS
- Added versions file that is automatically updated via the build process, which provides run-time version information on the new SoundJS object
- A few major performance updates
- Added default support for M4A, MP4, aiff, wma, and mid audio formats
- Changed how file extension support is determined so changes only need to be made to SoundJS
- Revised path parsing to support a larger range of file path formats
- Added sound registration and manifest registration allowing simple internal preloading, so Sounds can preload and play without PreloadJS.  This includes callback and EventDispatch as files load.
- Added default behavior to load src when play is called if src has not been registered or preloaded
- Removed global pause/resume
- Removed global setMasterVolume, in place of SoundJS.setVolume(), which is now global volume
- Added global volume/mute methods on plugins, can be used in place of setting properties of all instances and exist independent of those same properties on instances
- Added proper global mute, which affects sounds globally, instead of just applying mute to sounds
- Changed mute() to getMute() and setMute() on SoundJS and SoundInstances
- Removed id-based lookup
- Revised plugin approach, and simplified internal APIs
- Added EventDispatcher functionality to SoundJS and SoundInstance
- Added onSuccess callback and success event to SoundInstance to report successful play.
- Official WebAudio support via the new WebAudioPlugin, which is now the default audio handler
- Added create() method on SoundJS, which can be used to create a stopped sound
- Changed setPosition() method on SoundInstance so it is available on stopped instances
- Changed getDuration() method on SoundInstance so it returns the duration of stopped instances, instead of 0
- Changed default values set when SoundJS.play is called so instances retain position, volume, and pan unless explicitly changed
- Replaced proxy on Sound with a proxy on createjs namespace, createjs.proxy(method, scope, args*);
- Fixed issue to how delay was handled in SoundInstance so it will not fire if pause() or stop() is called before playback begins
- Fixed issues with indexOf that were sometimes preventing stop/mute, etc.
- Fixed issue with initial mute state in FlashPlugin
- Fixed an issue in FlashPlugin that caused getDuration to always return 0
- Fixed an issue in FlashPlugin that would cause a looping sound to loop from the same point it was paused from or set position to
- Fixed loop callback in FlashPlugin SoundInstance (previously it would not be called).
- Fixed an issue with FlashPlugin in IE that caused a race condition due to caching, which would stop it from working sometimes
- Fixed an issue with FlashPlugin that would cause a paused instance, once resumed, to not fire onComplete callback.
- Fixed an issue in HTMLAudioPlugin that caused it to incorrectly return isSupported as true when it should be false
- Better documentation throughout
- Improved examples in documentation
- Added tutorials: Basics, Mobile-safe approach, and SoundJS & PreloadJS.


### v0.3.0 [Aug 24, 2012]
***
##### MINOR
- Moved all classes into a configurable createjs namespace
- Added better support for missing sounds. SoundJS returns a lightweight instance that won't fail when calls are made on it.
- Added static mute/unmute methods to independently control a global mute property.
- Added support for preloading WAV files
- Fixed flash preload support when in tag mode
- Added lightweight flash instance, which is code only
- Added a debug flag [showOutput] to FlashPlugin, which will log Flash Activity
- Fixed issue with canPlayType throwing runtime in non-supported browsers.


### v0.2.0
***
- Second release, corresponding with the release of the CreateJS suite of tools (createjs.com).
- This version includes a target plugin model that abstracts audio playback to various plugins, which can be prioritized.
- Other updates include controllable sound instances, which are returned when a sound is played, providing a much easier way to control audio once it has started playback.


### v0.1.0
***
- Initial release.