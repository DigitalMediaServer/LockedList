# Changelog for LockedList NSIS plug-in
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]
### Changed
- Reorganized readme and changelog - merged `Readme.txt` into `README.md` and moved the changelog to `CHANGELOG.md`.

## [3.1.0.0] - 2024-04-30
### Added
- Added `SilentCancel` NSIS function to cancel async search (cancel by callback may not help - the callback is called too rarely and for
found items only).
- Added `CleanupAsyncThread` function to cleanup the state of async search thread before stating new search.
- Added `IsAsyncDone` function to help determine the current async search thread state.
- Implemented termination of `rundll32.exe` spawned 64-bit child-process if the search is canceled.
### Fixed
- Fixed `Wow64DisableWow64FsRedirection` was not reverted to initial value on exit.
- Re-wrote `GetSystemHandleInformation()` - now `NtQuerySystemInformation` use `SystemExtendedHandleInformation` (instead of obsolete SystemHandleInformation). Large number of handles led to overflow fields of `SYSTEM_HANDLE` structure (`USHORT` HandleValue and others).
- Fixed erroneous `ResetEvent(g_hFinishNow)` in `SystemFuncInit` - would be called several times during one search: (`GetSystemHandlesCount` and `EnumSystemHandles`) or (`GetSystemModulesCount` and `EnumSystemProcesses`). Now calling `ResetEvent(g_hFinishNow)` moved to `SystemEnum` - the main entry point of any enum, called only once.
- Now `EnumSystemHandles` does not handle handles of processes that could not be opened using `OpenProcess` in `GetProcessFileName` (System, System Idle, and other privileged ones).
### Changed
- `g_hThreadFiles` handle now not closed after any async search to hold last search result (done, cancel or wait).
- Increased array sizes for lists (files, modules, classes, captions, custom items and folders) from 128 to 256.
- Removed unused `g_hThreadAutoClose`.
- Handle types are now cached to improve performance.

## [3.0.0.5] - 2024-02-26
### Fixed
- ANSI build did not include null-termination when converting Unicode characters from `LockedList64.dll` to ANSI.
- Fixed endless loop in `GetSystemHandleInformation()` on some Windows versions.

## 3.0.0.4 - 2014-04-19
### Fixed
- ANSI build did not convert Unicode characters from `LockedList64.dll` to ANSI.

## 3.0.0.3 - 2014-08-07
### Fixed
- `FindProcess` did not always push "no" (`/yesno`) or an empty string onto the stack when no processes were running.

## 3.0.0.2 - 2014-08-05
### Added
- Added `CloseProcess` function.
- Improved application window caption lookup to find "main" windows (no owner).

## 3.0.0.1 - 2013-12-07
### Added
- Added 64-bit modules counting via `LockedList64` for the progress bar and silent search.

## 3.0.0.0 - 2013-12-01
### Added
- 64-bit module support via `LockedList64.dll` (special thanks to Ilya Kotelnikov).
### Fixed
- Fixed `GetSystemHandleInformation()` failing due to change in the number of handles between `NtQuerySystemInformation()` calls (special thanks to voidcast).

## 2.6.1.4 - 2012-07-12
### Fixed
- Fixed a crash in `SystemEnum` (v1.6) for the Unicode build.

## 2.6.1.3 - 2012-07-12
### Fixed
- Fixed `Back` button triggering auto-next during scan when `/autonext` is used.

## 2.6.1.2 - 2012-07-01
### Fixed
- Kills processes with no windows when using autoclose with `SilentSearch`.

## 2.6.1.1 - 2012-05-03
### Added
- Added autoclose code to `SilentSearch`.
### Fixed
- Fixed some bugs in `SystemEnum` (v1.5).

## 2.6.1.0 - 2012-04-25
### Fixed
- Fixed `StartsWith()` matching incorrectly for some strings.

## 2.6.0.2 - 2012-04-22
### Fixed
- Fixed window message loop halting page leave until mouse move on Windows 2000.

## 2.6.0.1 - 2012-03-23
### Fixed
* Fixed clipboard list copy for the Unicode build.
* Fixed crashes and infinite looping after repeatedly going back to the `LockedList` page.

## 2.6 - 2012-01-09
### Fixed
* Added missing calls to `EnableDebugPriv()` in `FindProcess` and `EnumProcesses`.

## 2.5 - 2011-07-11
### Fixed
- Fixed crash on Windows XP 32-bit and below.

## 2.4 - 2011-07-02
### Added
- Improved support for Windows x64 - now retrieves 64-bit processes but still cannot enumerate 64-bit modules (this is not possible from a 32-bit process).
### Fixed
- Fixed infinite loop which sometimes occurred on `Cancel` button click.

## 2.3 - 2011-02-07
### Added
- Added `/ignorebtn [button_id]` switch to specify a new `Ignore` button. This button can be added to the UI using Resource Hacker (recommended) or at run time using the System plug-in.
### Fixed
- Fixed `EnumSystemProcesses` on Windows 2000.
- Fixed `System` being listed on Windows 2000.
### Changed
- `/autonext` now also applies when all open programs have been closed while the dialog is visible.

## 2.2 - 2010-10-19
### Fixed
- Fixed `AddCustom` not adding items.
- No longer returns processes with no file path.

## 2.1 - 2010-08-24
### Added
- Added `/autonext` to automatically go to the next page when no items are found.

## 2.0 - 2010-08-23
### Fixed
- Fixed `IsFileLocked()` returning `true` for missing directories (thanks ukreator).
### Changed
- Replaced `afxres.h` include with `<Windows.h>` in `LockedList.rc`.

## 1.9 - 2010-07-23
### Changed
- Now using `ExtractIconEx` instead of `ExtractIcon` for all icons (thanks jiake).

## 1.8 - 2010-07-17
### Fixed
- Fixed programs not being closable.
- RC2: Removed debug message box.

## 1.7 - 2010-07-10
### Added
- Added `EnumProcesses` plug-in function.
- Added `FindProcess` plug-in function.
- Now gets 64-bit processes (but not modules).
- RC2: Added version information resource.
- RC3: Added `/yesno` switch to `FindProcess` plug-in function.
### Fixed
- RC4: Fixed `FindProcess` plug-in function case sensitivity (now case insensitive).
### Changed
- Process file description now retreived by `SystemEnum` if no process caption found.
- `SilentSearch` now uses a callback function instead of the stack.
- `SilentSearch` `/thread` changed to `/async`.
- Previously added processes now stored in an array for look up to prevent repetitions rather than looked up in the list view control.

## 1.6 - 2010-06-04
### Added
- Added `AddFolder` plug-in function.
### Fixed
- Fixed processes getting repeated in the list.
- Fixed list not auto scrolling to absolute bottom.
- `Next` button text restored when using `/ignore` and no processes are found.
### Changed
- File description displayed for processes without a window caption.
- Process ID displayed for processes without a window caption or file description.

## 1.5 - 2010-04-28
### Fixed
- Fixed `IsFileLocked` plug-in function.
- Fixed `/noprograms` plug-in switch.

## 1.4 - 2010-04-22
### Changed
- Removed DLL manifest to fix Microsoft `VC90 CRT` dependency.
- Now using ANSI `pluginapi.lib` for non Unicode build.
- Switched from `my_atoi()` to pluginapi `myatoi()`.

## 1.3 - 2010-04-04
### Fixed
- Increased `FILE_INFORMATION.ProcessCaption` to 1024 characters to fix buffer overflow crash.
- Fixed `IsFileLocked()` failing if first plug-in call (`EXDLL_INIT()`) is missing.

## 1.2 - 2010-04-02
### Added
- Added `ignore` dialog result if `/ignore` was used and there were programs running.
- Added additional argument for `/autoclose` and `/autoclosesilent` to set `Next` button text.
- Added `IsFileLocked` NSIS function.
### Fixed
- Fixed possible memory leaks if plug-in arguments were passed multiple times.
### Changed
- `/ignore` no longer used to specify `Next` button text for `/autoclose` and `/autoclosesilent`.

## 1.1 - 2010-03-31
### Added
- Added `AddCustom` plug-in function.
### Fixed
- Reverted back to using `my_atoi()` (Unicode NSIS `myatoi()` has a bug).
- Fixed possible memory access violation in `AddItem()`.
- Improved `Copy List` context menu item code.
- Fixed `Copy List` not showing correct process ID's.
- Fixed memory leak from not freeing allocated memory for list view item paramaters.
- RC2: Fixed `AddCustom` not working (non debug builds).

## 1.0 - 2010-03-30
### Fixed
- Fixed CRT dependency.
- Improved percent complete calculations.
- Now pushes `/next` to stack in between stack items.
- Fixed memory leak in `AddItem()`.
- Fixed crashes caused by using `AddFile` plug-in function.
- RC3: Fixed 6 possible memory access violations.
- RC3: Removed debug `MessageBox`.
- RC3: Unicode plug-in build name changed to `LockedList.dll`.
- RC4: Removed unused includes.
- RC5: Fixed memory access violation when using `SilentSearch`.
### Changed
- RC2: Excluded process ID's #0 and #4 from searches (`System Idle Process` and `System`).
- General code cleanup.

## 0.9 - 2010-03-11
### Added
- Added `/menuitems` "close_text" "copy_list_text".
- Implemented Unicode build.
### Fixed
- Fixed memory access violation in `g_pszParams`.
- Various fixes and changes in `SystemEnum` (see `SystemEnum.cpp`).
- Now includes current process in search when using `SilentSearch`.
- RC2: Fixed crash if no search criteria was provided (division by zero).
- RC3: Fixed Unicode build crash (`my_zeromemory`) (and `SystemEnum` v0.5).
- RC4: Fixed garbage process appearing (`SystemEnum` v0.6).
- RC4: Fixed Unicode build not returning correct processes (`SystemEnum` v0.6).
### Changed
- Implemented new NSIS plugin API (`/NOUNLOAD` no longer necessary).

## 0.8 - 2009-07-24
### Changed
- Increased array sizes for processes and process modules from 128 to 256.

## 0.7 - 2008-02-24
### Added
- Added `AddClass` and `AddCaption` functions.
### Fixed
- Re-wrote `/autoclose` code and fixed crashing.
- Fixed `Copy List` memory read access error.
- Made thread exiting faster for page leave.
- Progress bar and `%` work better.
- Processing mouse cursor redrawn.
- Ignore button text only set when list is not empty.
- RC2: Fixed /autoclose arguments.

## 0.6 - 2008-02-12
### Added
- Added `/autoclose` "close_text" "kill_text" "failed_text" and `/autoclosesilent` "failed_text". The `/ignore` switch can be used along with this to set the `Next` button text.
- Added `/colheadings` "application_text" "process_text"

## 0.5 - 2007-11-25
### Fixed
- Fixed memory leak causing crash when re-visiting dialog. Caused by duplicate call to `GlobalFree` on the same pointer.

## 0.4 - 2007-09-27
### Added
- Added `/ignore` switch that prevents the `Next` button being disabled.
- Added `AddApplications` to add all running applications to the list.
- Added processing mouse cursor.
- Added right-click context menu with `Close` and `Copy List` options.
- Added progress bar.
- Added default program icon for processes without an icon.
- Added code to resize controls for different dialog sizes.
### Fixed
- Fixed typo in `AddModule` function (`ModulesCount>FilesCount`). Thanks kalverson.
- Debug privileges were not being set under `SilentSearch`.
### Changed
- Module or file names can now be just the file name as opposed to the full path.
- Folder paths are converted to full paths (some are short DOS paths) before comparison.
- List view is now scrolled into view while items are added.
- List changed to multiple columns.

## 0.3 - 2007-07-13
### Added
- Added `LVS_EX_LABELTIP` style to list view control for long item texts.
- Added `WM_SYSMENU` existence check when obtaining window captions.
- Added reference to Unload function to read-me.
### Fixed
- Files/modules lists memory is now freed when using SilentSearch.
- Files and Modules lists count now reset after a search.
### Changed
- Width of list header changed from `width-6` to `width-GetSystemMetrics(SM_CXHSCROLL)`.

## 0.2 - 2007-07-12
### Added
- Added two new examples.
### Fixed
- Fixed pointer error in `FileList` struct causing only first module/file added to be used.
- Fixed caption repetition over multiple processes.
- Fixed stack overflow in `DlgProc`. Special thanks, Roman Prysiazhniuk for locating the source.
### Changed
- Better percent complete indication.

## 0.1 - 2007-07-10
### First build

[Unreleased]: https://github.com/DigitalMediaServer/LockedList/compare/v3.1.0.0...HEAD
[3.1.0.0]: https://github.com/DigitalMediaServer/LockedList/compare/v3.0.0.5...v3.1.0.0
[3.0.0.5]: https://github.com/DigitalMediaServer/LockedList/releases/tag/v3.0.0.5
