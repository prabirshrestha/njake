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

### assymblyinfo

```js
var njake = require('./njake'),
    assemblyinfo = njake.assemblyinfo;
    
task('assemblyinfo', function () {
	assemblyinfo({
		file: 'Source/Facebook/Properties/AssemblyInfo.cs',
		language: 'c#',
		namespaces: ['System.Reflection', 'System.Runtime.InteropServices'],
		assembly: {
			notice: function () {
				return '// Do not modify this file manually, use jakefile instead.\r\n';
			},
			AssemblyTitle: 'MyApp',
			AssemblyDescription: 'My App Decription',
			AssemblyCompany: 'Prabir Shrestha',
			AssemblyProduct: 'My App',
			AssemblyCopyright: 'Copyright (c) 2012, Prabir Shrestha.',
			ComVisible: false,
			AssemblyVersion: '6.0.10.0',
			AssemblyFileVersion: '6.0.10.0'
		}
	})
}, { async: true })
```

*Note: Default language is 'c#' and can be ommited. `System.Reflection` and `System.Runtime.InteropServices` are
added by default which is also optional. `assembly` can contain a value containg function which returns string. This will
allow you to write your custom code which may not be supported by assemblyinfo.*