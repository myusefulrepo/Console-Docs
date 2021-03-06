---
title: WriteConsoleOutput function
description: Writes character and color attribute data to a specified rectangular block of character cells in a console screen buffer.
author: miniksa
ms.author: miniksa
ms.topic: article
keywords: console, character mode applications, command line applications, terminal applications, console api
f1_keywords:
- consoleapi2/WriteConsoleOutput
- wincon/WriteConsoleOutput
- WriteConsoleOutput
- consoleapi2/WriteConsoleOutputA
- wincon/WriteConsoleOutputA
- WriteConsoleOutputA
- consoleapi2/WriteConsoleOutputW
- wincon/WriteConsoleOutputW
- WriteConsoleOutputW
MS-HAID:
- '\_win32\_writeconsoleoutput'
- 'base.writeconsoleoutput'
- 'consoles.writeconsoleoutput'
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/desktop'
ms.assetid: a98b8118-97ce-4dd4-a337-122d4a76f232

topic_type:
- apiref
api_name:
- WriteConsoleOutput
- WriteConsoleOutputA
- WriteConsoleOutputW
api_location:
- Kernel32.dll
- API-MS-Win-Core-Console-l2-1-0.dll
- KernelBase.dll
- API-MS-Win-DownLevel-Kernel32-l1-1-0.dll
api_type:
- DllExport
---

# WriteConsoleOutput function

[!INCLUDE [not-recommended-banner](./includes/not-recommended-banner.md)]

Writes character and color attribute data to a specified rectangular block of character cells in a console screen buffer. The data to be written is taken from a correspondingly sized rectangular block at a specified location in the source buffer.

## Syntax

```C
BOOL WINAPI WriteConsoleOutput(
  _In_          HANDLE      hConsoleOutput,
  _In_    const CHAR_INFO   *lpBuffer,
  _In_          COORD       dwBufferSize,
  _In_          COORD       dwBufferCoord,
  _Inout_       PSMALL_RECT lpWriteRegion
);
```

## Parameters

*hConsoleOutput* \[in\]  
A handle to the console screen buffer. The handle must have the **GENERIC\_WRITE** access right. For more information, see [Console Buffer Security and Access Rights](console-buffer-security-and-access-rights.md).

*lpBuffer* \[in\]  
The data to be written to the console screen buffer. This pointer is treated as the origin of a two-dimensional array of [**CHAR\_INFO**](char-info-str.md) structures whose size is specified by the *dwBufferSize* parameter.

*dwBufferSize* \[in\]  
The size of the buffer pointed to by the *lpBuffer* parameter, in character cells. The **X** member of the [**COORD**](coord-str.md) structure is the number of columns; the **Y** member is the number of rows.

*dwBufferCoord* \[in\]  
The coordinates of the upper-left cell in the buffer pointed to by the *lpBuffer* parameter. The **X** member of the [**COORD**](coord-str.md) structure is the column, and the **Y** member is the row.

*lpWriteRegion* \[in, out\]  
A pointer to a [**SMALL\_RECT**](small-rect-str.md) structure. On input, the structure members specify the upper-left and lower-right coordinates of the console screen buffer rectangle to write to. On output, the structure members specify the actual rectangle that was used.

## Return value

If the function succeeds, the return value is nonzero.

If the function fails, the return value is zero. To get extended error information, call [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360).

## Remarks

**WriteConsoleOutput** treats the source buffer and the destination screen buffer as two-dimensional arrays (columns and rows of character cells). The rectangle pointed to by the *lpWriteRegion* parameter specifies the size and location of the block to be written to in the console screen buffer. A rectangle of the same size is located with its upper-left cell at the coordinates of the *dwBufferCoord* parameter in the *lpBuffer* array. Data from the cells that are in the intersection of this rectangle and the source buffer rectangle (whose dimensions are specified by the *dwBufferSize* parameter) is written to the destination rectangle.

Cells in the destination rectangle whose corresponding source location are outside the boundaries of the source buffer rectangle are left unaffected by the write operation. In other words, these are the cells for which no data is available to be written.

Before **WriteConsoleOutput** returns, it sets the members of *lpWriteRegion* to the actual screen buffer rectangle affected by the write operation. This rectangle reflects the cells in the destination rectangle for which there existed a corresponding cell in the source buffer, because **WriteConsoleOutput** clips the dimensions of the destination rectangle to the boundaries of the console screen buffer.

If the rectangle specified by *lpWriteRegion* lies completely outside the boundaries of the console screen buffer, or if the corresponding rectangle is positioned completely outside the boundaries of the source buffer, no data is written. In this case, the function returns with the members of the structure pointed to by the *lpWriteRegion* parameter set such that the **Right** member is less than the **Left**, or the **Bottom** member is less than the **Top**. To determine the size of the console screen buffer, use the [**GetConsoleScreenBufferInfo**](getconsolescreenbufferinfo.md) function.

**WriteConsoleOutput** has no effect on the cursor position.

[!INCLUDE [setting-codepage-mode-remarks](./includes/setting-codepage-mode-remarks.md)]

> [!TIP]
> This API has a **[virtual terminal](console-virtual-terminal-sequences.md)** equivalent in the **[text formatting](console-virtual-terminal-sequences.md#text-formatting)** and **[cursor positioning](console-virtual-terminal-sequences.md#cursor-positioning)** sequences. Move the cursor to the location to insert, apply the formatting desired, and write out the text. _Virtual terminal sequences_ are recommended for all new and ongoing development.

## Examples

For an example, see [Reading and Writing Blocks of Characters and Attributes](reading-and-writing-blocks-of-characters-and-attributes.md).

## Requirements

| &nbsp; | &nbsp; |
|-|-|
| Minimum supported client | Windows 2000 Professional \[desktop apps only\] |
| Minimum supported server | Windows 2000 Server \[desktop apps only\] |
| Header | ConsoleApi2.h (via WinCon.h, include Windows.h) |
| Library | Kernel32.lib |
| DLL | Kernel32.dll |
| Unicode and ANSI names | **WriteConsoleOutputW** (Unicode) and **WriteConsoleOutputA** (ANSI) |

## See also

[Console Functions](console-functions.md)

[**CHAR\_INFO**](char-info-str.md)

[**COORD**](coord-str.md)

[**GetConsoleScreenBufferInfo**](getconsolescreenbufferinfo.md)

[Low-Level Console Output Functions](low-level-console-output-functions.md)

[**ReadConsoleOutput**](readconsoleoutput.md)

[**ReadConsoleOutputAttribute**](readconsoleoutputattribute.md)

[**ReadConsoleOutputCharacter**](readconsoleoutputcharacter.md)

[**SetConsoleCP**](setconsolecp.md)

[**SetConsoleOutputCP**](setconsoleoutputcp.md)

[**SMALL\_RECT**](small-rect-str.md)

[**WriteConsoleOutputAttribute**](writeconsoleoutputattribute.md)

[**WriteConsoleOutputCharacter**](writeconsoleoutputcharacter.md)
