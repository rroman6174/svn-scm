# Subversion source control for VS Code

# Info
This project is a fork of [JohnstonCode's VS Code SVN Extension](https://github.com/JohnstonCode/svn-scm) with the addition of an API that allows extension developers to contribute Client-side SVN Hooks.

# The API
## Integrating with your extension
Your extension will need to add the following dependancy to it's package.json

```
"extensionDependencies": [
  "RobertRoman.svn-scm"
]
```
This is necessary since extensions in vscode are lazy-loaded and there is no guarantee that this extension will be initialized and availible to provide the api before yours.
## Usage
Import the definitions from [svnHookApi.ts](./src/hooks/svnHookApi.ts) into your project.

```
npm i "https://github.com/rroman6174/svn-scm.git"
```

Register a hook by calling this extension's API with an SVNHook:

```
import {SVNHook, SVNHookApi} from 'svn-scm/src/hooks/svnHookApi';

const svnApi: SVNHookApi = vscode.extensions.getExtension('RobertRoman.svn-scm')?.exports;
svnApi.registerHook(myHook);
```

### Blocking Hooks
```
onPreCommit?(documents: string[]): Promise<void>;
onPreRevert?(documents: string[]): Promise<void>;
```
The `onPreCommit` and `onPreRevert` hooks your extension provides will be supplied with an array of strings
of absolute Working Copy filepaths that are about to be committed/reverted. 
If your async hook throws an error the action will be aborted and the user will not be allowed to commit/revert.

### Non-Blocking Hooks
```
onPostCommit?(documents: string[]): Promise<void>;
onPostRevert?(documents: string[]): Promise<void>;
onUpdate?(): Promise<void>;
```

Errors thrown on Non-Blocking hooks will be ignored. The onUpdate hook does not pass back a list of files that were updated. This may change in the future.


# Prerequisites

> **Note**: This extension leverages your machine's SVN installation,\
> so you need to [install SVN](https://subversion.apache.org) first.

## Windows

If you use [TortoiseSVN](https://tortoisesvn.net/), make sure the option
**Command Line Tools** is checked during installation and
`C:\Program Files\TortoiseSVN\bin` is available in PATH.

## Feedback & Contributing

* Please report any bugs, suggestions or documentation requests via the
  [Issues](https://github.com/rroman6174/svn-scm/issues)
* Feel free to submit
  [pull requests](https://github.com/rroman6174/svn-scm/pulls)

## [Contributors](https://github.com/rroman6174/svn-scm/graphs/contributors)

# Features

### Checkout

You can checkout a SVN repository with the `SVN: Checkout` command in the **Command Palette** (`Ctrl+Shift+P`). You will be asked for the URL of the repository and the parent directory under which to put the local repository.

----

* Source Control View
* Quick Diffs in gutter
* Status Bar
* Create changelists
* Add files
* Revert edits
* Remove files
* Create branches
* Switch branches
* Create patches
* Diff changes
* Commit changes/changelists
* See commit messages

## Blame

Please use a dedicated extension like [blamer-vs](https://marketplace.visualstudio.com/items?itemName=beaugust.blamer-vs)

## Settings
Here are all of the extension settings with their default values. To change any of these, add the relevant Config key and value to your VSCode settings.json file. Alternatively search for the config key in the settings UI to change its value.

<!--begin-settings-->
```js
{
  // Whether auto refreshing is enabled
  "svn.autorefresh": true,

  // Select all files when commit changes
  "svn.commit.changes.selectedAll": true,

  // Check empty message before commit
  "svn.commit.checkEmptyMessage": true,

  // Set file to status resolved after fix conflicts
  "svn.conflicts.autoResolve": null,

  // Encoding of svn output if the output is not utf-8. When this parameter is null, the encoding is automatically detected. Example: 'windows-1252'.
  "svn.default.encoding": null,

  // The default location to checkout a svn repository.
  "svn.defaultCheckoutDirectory": null,

  // When a file is deleted, what SVN should do? `none` - Do nothing, `prompt` - Ask the action, `remove` - automatically remove from SVN
  "svn.delete.actionForDeletedFiles": "prompt"  // values: ["none","prompt","remove"],

  // Ignored files/rules for `svn.delete.actionForDeletedFiles`(Ex.: file.txt or **/*.txt)
  "svn.delete.ignoredRulesForDeletedFiles": [],

  // Controls whether to automatically detect svn externals.
  "svn.detectExternals": true,

  // Controls whether to automatically detect svn on ignored folders.
  "svn.detectIgnored": true,

  // Show diff changes using latest revision in the repository. Set false to use latest revision in local folder
  "svn.diff.withHead": true,

  // Whether svn is enabled
  "svn.enabled": true,

  // Try the experimental encoding detection
  "svn.experimental.detect_encoding": null,

  // Priority of encoding
  "svn.experimental.encoding_priority": [],

  // Url for the gravitar icon using the <AUTHOR>, <AUTHOR_MD5> and <SIZE> placeholders
  "svn.gravatar.icon_url": "https://www.gravatar.com/avatar/<AUTHOR_MD5>.jpg?s=<SIZE>&d=robohash",

  // Use garavatar icons in log viewers
  "svn.gravatars.enabled": true,

  // Ignores the warning when SVN is missing
  "svn.ignoreMissingSvnWarning": null,

  // List of SVN repositories to ignore.
  "svn.ignoreRepositories": null,

  // Ignores the warning when working copy is too old
  "svn.ignoreWorkingCopyIsTooOld": null,

  // Regex to detect path for 'branches' in SVN URL, 'null' to disable. Subpath use 'branches/[^/]+/([^/]+)(/.*)?' (Ex.: 'branches/...', 'versions/...')
  "svn.layout.branchesRegex": "branches/([^/]+)(/.*)?",

  // Regex group position for name of branch
  "svn.layout.branchesRegexName": 1,

  // Set true to show 'branches/<name>' and false to show only '<name>'
  "svn.layout.showFullName": true,

  // Regex group position for name of tag
  "svn.layout.tagRegexName": 1,

  // Regex to detect path for 'tags' in SVN URL, 'null' to disable. Subpath use 'tags/[^/]+/([^/]+)(/.*)?'. (Ex.: 'tags/...', 'stamps/...')
  "svn.layout.tagsRegex": "tags/([^/]+)(/.*)?",

  // Regex to detect path for 'trunk' in SVN URL, 'null' to disable. (Ex.: '(trunk)', '(main)')
  "svn.layout.trunkRegex": "(trunk)(/.*)?",

  // Regex group position for name of trunk
  "svn.layout.trunkRegexName": 1,

  // Number of commit messages to log
  "svn.log.length": 50,

  // Maximum depth to find subfolders using SVN
  "svn.multipleFolders.depth": 4,

  // Allow to find subfolders using SVN
  "svn.multipleFolders.enabled": null,

  // Folders to ignore using SVN
  "svn.multipleFolders.ignore": ["**/.git","**/.hg","**/vendor","**/node_modules"],

  // Path to the svn executable
  "svn.path": null,

  // Only show previous commits for a given user. Requires svn >= 1.8
  "svn.previousCommitsUser": null,

  // Refresh remote changes on refresh command
  "svn.refresh.remoteChanges": null,

  // Set the interval in seconds to check changed files on remote repository and show in statusbar. 0 to disable
  "svn.remoteChanges.checkFrequency": 300,

  // Show the output window when the extension starts
  "svn.showOutput": null,

  // Show the update message when update is run
  "svn.showUpdateMessage": true,

  // Set left click functionality on changes resource state
  "svn.sourceControl.changesLeftClick": "open diff"  // values: ["open","open diff"],

  // Combine the svn external in the main if is from the same server.
  "svn.sourceControl.combineExternalIfSameServer": null,

  // Allow to count unversioned files in status count
  "svn.sourceControl.countUnversioned": true,

  // Hide unversioned files in Source Control UI
  "svn.sourceControl.hideUnversioned": null,

  // Ignore unversioned files like .gitignore, Configuring this will overlook the default ignore rule
  "svn.sourceControl.ignore": [],

  // Changelists to ignore on commit
  "svn.sourceControl.ignoreOnCommit": ["ignore-on-commit"],

  // Changelists to ignore on status count
  "svn.sourceControl.ignoreOnStatusCount": ["ignore-on-commit"],

  // Set to ignore externals definitions on update (add --ignore-externals)
  "svn.update.ignoreExternals": true
}
```
<!--end-settings-->
