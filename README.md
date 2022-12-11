# Windows Dummy Keystroke

This package exports `sendDummyKeystroke()`, which will send a generic key down/up event on Windows.

## But why?

This is a workaround for an issue with Windows Notifications. Normally [you can call the native `AllowSetForegroundWindow` method to permit another application to take foreground focus](https://devblogs.microsoft.com/oldnewthing/20090220-00/?p=19083). But when an app is started from a Windows notification activation, this call fails with `ERROR_ACCESS_DENIED`. One of a [list of conditions has to be met](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-allowsetforegroundwindow) for the call to succeed.

Sending a key press event satisfies this check and lets `AllowSetForegroundWindow` succeed.

This is useful for letting a second instance of a program activate the original instance. In Electron, you would call this before calling `requestSingleInstanceLock()`, as this can call `AllowSetForegroundWindow` for the initial instance.

This workaround was [used by Chromium to solve a foreground focus issue](https://bugs.chromium.org/p/chromium/issues/detail?id=837796), and is adapted here for use with Node.
