# AppleTrace
Trace tool for iOS/macOS

`AppleTrace` is developed for analyzing app's performance on `iOS/macOS`.

Let's talk in [Gitter](https://gitter.im/appletrace/AppleTrace) or 加入微信群(页面最底部)

- [中文说明，开发思路及方法](http://everettjf.com/2017/09/21/appletrace/)
- [搭载MonkeyDev可trace第三方App](http://everettjf.com/2017/10/12/appletrace-dancewith-monkeydev/)

![appletrace](http://everettjf.github.io/stuff/appletrace/appletrace.gif)


## Feature

1. User-defined trace section.
2. **[arm64 only]** Trace all Objective C methods.

## FAQ

[Go to Wiki](https://github.com/everettjf/AppleTrace/wiki)

## Usage

1. Produce trace data.
2. Copy from app's sandbox directory.
3. Merge (all) trace data files into one file `trace.json`. (There may be more than 1 trace file.)
4. Generate `trace.html` based on `trace.json`.

See below for more detail.

### 1. Produce


Until now , there are 2 ways for generating trace data.

1. Manual set section.

	Call `APTBeginSection` at the beginning of method ,and `APTEndSection` at the end of method. For Objective C method (whether instance method or class method), there are `APTBegin` and `APTEnd` macro for easy coding.
	
	```
	void anyKindsOfMethod{
	    APTBeginSection("process");
	    // some code
	    APTEndSection("process");
	}
	
	- (void)anyObjectiveCMethod{
	    APTBegin;
	    // some code
	    APTEnd;
	}
	```
	
	Sample app is `sample/ManualSectionDemo`.
	
2. Dynamic library hooking all objc_msgSend.

	Hooking all objc_msgSend methods (based on HookZz). This only support arm64.
	
	Sample app is `sample/TraceAllMsgDemo`.

### 2. Copy

Using any kinds of method, copy `<app's sandbox>/Library/appletracedata` out of Simulator/RealDevice.

![appletracedata](image/appletracedata.png)


### 3. Merge

Merge/Preprocess the `appletracedata`.

```
python merge.py -d <appletracedata directory>
```

This will produce `trace.json` in appletracedata directory.

### 4. Generate

Generate `trace.html` using `catapult`.

```
python catapult/tracing/bin/trace2html appletracedata/trace.json --output=appletracedata/trace.html

open trace.html
```

## SampleData

Open `sampledata/trace.html`.


## Dependencies

1. [catapult](https://github.com/catapult-project/catapult)
2. [HookZz](https://github.com/jmpews/HookZz)



## Develop Plan

1. dtrace as data source.


## Group

[Gitter](https://gitter.im/appletrace/AppleTrace)

or 微信群

![wechatgroup](image/wechatgroup.png) 

(若二维码过期请先加微信 everettjf )

