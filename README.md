[![Build Status](https://travis-ci.org/jonycheung/deadsimple-less-watch-compiler.svg?branch=master)](https://travis-ci.org/jonycheung/deadsimple-less-watch-compiler) [![npm version](https://badge.fury.io/js/less-watch-compiler.svg)](https://badge.fury.io/js/less-watch-compiler) [![Dependencies](https://david-dm.org/jonycheung/deadsimple-less-watch-compiler.svg)](https://david-dm.org/jonycheung/less-watch-compiler) [![devDependency Status](https://david-dm.org/jonycheung/deadsimple-less-watch-compiler/dev-status.svg)](https://david-dm.org/jonycheung/less-watch-compiler#info=devDependencies) [![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)

Dead Simple LESS CSS Watch Compiler
===================

A command that watches folders(and subfolders) for file changes and automatically compile the less css files into css. This is a file system watcher and compiler. It also keep track of a dependency tree and recompiles the parent if an imported (child) LESS file is changed.

Parts of this script is modified from Mikeal Rogers's watch script (https://github.com/mikeal/watch)

## Prerequisites
Install [LESS](http://www.lesscss.org/) and make sure the `lessc` binary is accessible to the script. Installing LESS with the `-g`(global) flag will make the binary accessible to your system.

Use [yarn](https://yarnpkg.com/) 

```
(sudo) yarn global add less
```
or npm

```
(sudo) npm install -g less
```



## Installation
Install the `less-watch-compiler` command globally.

Use [yarn](https://yarnpkg.com/) 

```
(sudo) yarn global add
```
or npm

```
(sudo) npm install -g less-watch-compiler
```

## Usage
### With no main file 
You need to pass in the minimum 2 parameters - <source_dir> and <destination_dir> . First parameter is the source folder to watch for changes and second is the output folder in which the css files will be compiled

Usage:
```
less-watch-compiler [options] <source_dir> <destination_dir> [main_file_name]
```

### With main file
If you pass in the 3rd optional parameter, Any file change will trigger only to compile the main file specified in the 3rd parameter.
Assuming the 3rd is "main.less" 

Usage:
```
less-watch-compiler FOLDER_TO_WATCH FOLDER_TO_OUTPUT MAIN_FILE_DOT_LESS[Optional]
```

		
		input folder: src
		|____ src
		|________ main.less (import aux.less)
		|________ aux.less
		 output folder: dist
		|____ dist
		|________ main.css
        

Real usage:
```
less-watch-compiler src dist main.less
```
###Example #1
```
less-watch-compiler tests/less tests/css
```
The above command will watch the `tests/less` folder and compile the LESS CSS files into `tests/css` folder as soon as they are added/updated.

###Example #2
Add `less-watch-compiler.config.json` as follows in your project folder

```
{
    "allowedExtensions":[".less"],
    "minified": false,
    "sourceMap": false,
    "plugins": "less_plugin1,less_plugin2",
    "watchFolder": "tests/less",
    "outputFolder": "tests/css"
}
```

The above will do the same as in example 1. But you can just run the following without passing the folders everytime.

```
less-watch-compiler
```

## Options:

    -h, --help                       output usage information
    -V, --version                    output the version number
    --source-map                     Generate source map for css files
    --main-file <file>               Specify <file> as the file to always re-compile e.g. '--main-file style.less'
    --plugins <plugin-a>,<plugin-b>  List of plugins separated by commas

## Extras:
* By default, "minified" is turned on to always compress/minify output. You can set the minification to false by adding `"minified":false` in the config file.
* By default, "sourceMap" is turned off. You can generating sourcemap to true by adding `"sourceMap":true` in the config file.
* By default, this script only compiles files with `.less` extension. More file extensions can be added by modifying the `allowedExtensions` array in `config.json`.
* Files that start with underscores `_style.css` or period `.style.css` are ignored. This behavior can be changed in the `filterFiles()` function.


### Using the source files
Alternativelly, you can checkout the code and run things locally like this:

```
node less-watch-compiler.js [options]
```

To run unit tests: `yarn test` or `npm test` (see tests/test.js).
