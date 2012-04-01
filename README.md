# njake -- Jake helper tasks for .NET developers
njake is a collection of helper tasks for jake (https://github.com/mde/jake). jake is a build script tool for node.js.

## Requirements
* node.js (http://nodejs.org/#download)
* `npm install -g jake`

## Api Docs

### msbuild

```js
var njake = require('./njake'),
    msbuild = njake.msbuild;
    
msbuild.setDefaults({
	properties: { Configuration: 'Release' },
	processor : 'x86',
	version	  : 'net4.0'
})
    
task('build', function() {
	msbuild({
		file: 'Source/Facebook-Net40.sln',
		targets: ['Clean', 'Build']
	})
}, { async: true })
```

### xunit.net

```js
var njake = require('./njake'),
    xunit = njake.xunit;
    
xunit.setDefaults({
	_exe: 'Tools/xunit/xunit.console.clr4.x86'
})

task('test', ['build'], function () {
	xunit({
		assembly: 'Facebook.Tests.dll'
	})
}, { async: true })
```

### nuget

```js
var njake = require('./njake'),
    nuget = njake.nuget;
    
nuget.setDefaults({
	_exe: 'Source/.nuget/NuGet.exe',
	verbose: true
})

task('nuget', ['build'], function () {
	nuget.pack({
		nuspec: 'Source/Facebook.nuspec',
		version: '6.0.10.0',
		properties: { 'owners': 'Prabir Shrestha' },
		outputDirectory: 'Dist/NuGet'
	})
}, { async: true })

```