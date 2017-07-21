# winapi-json

Windows API function in JSON format. Machine readable and grouped by category.

The data was scraped from MSDN. Errors are possible, if you find any please do report.

Each JSON file in api_by_category folder contains information about all API functions in that category.

Each JSON entry has the following fields:

* category - e.g. 'Accessibility'
* name - name of the function e.g. RegisterPointerInputTarget
* is_callback - 1 if function is a callback, 0 otherwise
* description - short description on the function
* n_arguments - number of arguments
* arguments - an array of entries one per argument:
  * in_out - indicates if it's an input of output argument
  * type - type of an argument e.g. HWND, LONG, etc.
  * name - argument name
  * description - argument description
* return_type - what function returns e.g. BOOL WINAPI
* return_value - decription of return value
* remarks - some remarks from MSDN page
* header - header file that has this function's protype
* library -  .lib that has this function
* dll - .dll that has this function
* min_server - minimum supported version of server
* min_client - minimum supported version of client

Typical entry looks like this:

```
{
  "category": "Accessibility",
  "n_arguments": 2,
  "description": "Allows the caller to register a target window to which all pointer input of the specified type is redirected.",
  "is_callback": 0,
  "library": "User32.lib",
  "dll": "User32.dll",
  "min_server": "Windows Server 2012 [desktop apps only]",
  "header": "WinUser.h (include Windows.h)",
  "return_value": "If the function succeeds, the return value is non-zero. If the function fails, the return value is zero. To get extended error information, call GetLastError. ",
  "arguments": [
    {
      "in_out": "_In_",
      "type": "HWND",
      "name": "hwnd",
      "description": "The window to register as a global redirection target. Redirection can cause the foreground window to lose activation (focus). To avoid this, ensure the window is a message-only window or has the WS_EX_NOACTIVATE style set."
    },
    {
      "in_out": "_In_",
      "type": "POINTER_INPUT_TYPE",
      "name": "pointerType",
      "description": "Type of pointer input to be redirected to the specified  window. This is any valid and supported value from the POINTER_INPUT_TYPE enumeration. Note that the generic PT_POINTER type and the PT_MOUSE type are not valid in this parameter."
    }
  ],
  "remarks": "An application with the UI Access privilege can use this function to register its own window to receive all input of the specified pointer input type. Each desktop allows only one such global redirection target window for each pointer input type at any given time. The first window to successfully register remains in effect until the window is unregistered or destroyed, at which point the role is available to the next qualified caller. While the registration is in effect, all input of the specified pointer type, whether from an input device or injected by an application, is redirected to the registered window. However, when the process that owns the registered window injects input of the specified pointer type, such injected is not redirected but is instead processed normally. An application that wishes to register the same window as a global redirection target for multiple pointer input types must call the RegisterPointerInputTarget function multiple times, once for each pointer input type of interest. If the calling thread does not have the UI Access privilege, this function fails with the last error set to ERROR_ACCESS_DENIED. If the specified pointer input type is not valid, this function fails with the last error set to ERROR_INVALID_PARAMETER. If the calling thread does not own the specified window, this function fails with the last error set to ERROR_ACCESS_DENIED. If the specified window's desktop already has a registered global redirection target for the specified pointer input type, this function fails with the last error set to ERROR_ACCESS_DENIED. ",
  "return_type": "BOOL",
  "min_client": "Windows 8 [desktop apps only]",
  "name": "RegisterPointerInputTarget"
}
```

## F.A.Q
* Why are they split into categories?

Because it's more manageable

* Why are JSON files pretty printed?

Convenient for search and diffing
