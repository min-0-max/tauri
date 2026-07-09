---
tauri: patch:bug
tauri-runtime-wry: patch:bug
---

Fix a retain leak in `WebviewMessage::WithWebview` on Apple targets. Each call moved the WKWebView, its user-content manager and (macOS) the NSWindow out via `Retained::into_raw` without ever recovering them, leaking one strong reference each. On macOS the leaked WKWebView retain kept the webview's WebContent process alive for the whole lifetime of the app. The handles are now borrowed for the duration of the closure (`Retained::as_ptr`), and the runtime `Webview` pointers are `*const` to reflect the borrow; `PlatformWebview`'s public accessors are unchanged (`*mut`).
