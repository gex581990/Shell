# Nilesoft Shell Complete Reference
> Auto-generated knowledge base for AI assistants. Read this file at session start for instant Nilesoft Shell mastery.

## Table of Contents
1. [Syntax Fundamentals](#syntax-fundamentals)
2. [Properties Reference](#properties-reference)
3. [Expression System](#expression-system)
4. [Built-in Functions](#built-in-functions)
5. [Type System](#type-system)
6. [Image Syntax](#image-syntax)
7. [Command System](#command-system)
8. [Common Patterns](#common-patterns)
9. [Validation Rules](#validation-rules)
10. [Implementation Notes](#implementation-notes)

---

## Syntax Fundamentals

### Basic Structure
```nss
// Configuration file structure
settings { ... }                    // Global settings
theme { ... }                       // Theme configuration
import 'path/to/file.nss'          // Include external files

// Menu definitions
menu(type='file' where=sel.count>0) {
    item(title="Action" cmd="notepad.exe")
    separator
    menu(title="Submenu") { ... }
}

// Modify existing system items
modify(where=this.id==id.copy_as_path) { ... }

// Variables
$var = expression
$global_var = "value"
```

### Comments
```nss
// Single-line comment
/* Multi-line
   comment */
```

### String Interpolation
```nss
// Variable interpolation
"@variablename"                     // Insert variable value
'@(expression)'                     // Evaluate expression

// Examples
title = "@sel.file.name"
title = 'Selected: @(sel.count) files'
```

---

## Properties Reference

### Core Properties (All Items)

**title** - Display text (string or expression)
```nss
title = "Static text"
title = loc.variable_name          // Localized string
title = @sel.file.name             // Expression
title = expression                 // Any expression
```

**image** - Icon specification (see Image Syntax section)
```nss
image = \uE253                     // Unicode glyph
image = #ff0000                    // Color block
image = 'path\to\icon.ico'         // File path
image = [\uE253, #f00]            // Glyph with color
image = inherit                    // Inherit from parent
```

**where** - Conditional visibility (expression returning boolean)
```nss
where = sel.count > 1              // Multiple selection
where = sel.type == 1              // Directories only
where = key.shift()                // Shift key pressed
where = sel.count > 0 && !wnd.is_taskbar
```

**type** - Context filter (pipe-separated string or expression)
```nss
type = 'file'                      // Files only
type = 'file|dir'                  // Files or directories
type = 'drive|back'                // Drives or background
type = 'namespace'                 // Namespace items
type = 'taskbar'                   // Taskbar context
type = 'desktop'                   // Desktop background
type = 'edit'                      // Text edit fields
```

**mode** - Selection mode (string)
```nss
mode = 'single'                    // Single selection only
mode = 'multiple'                  // Multiple selection allowed
mode = 'multi'                     // Alias for multiple
```

**vis** - Visibility control (identifier or boolean)
```nss
vis = vis.remove                   // Remove item
vis = vis.hidden                   // Hide item
vis = vis.disable                  // Disable item
vis = vis.label                    // Display as label (non-clickable)
vis = vis.static                   // Static item
vis = vis.extended                 // Show only with Shift
vis = inherit                      // Inherit from parent
```

**sep** - Separator placement
```nss
sep = sep.before                   // Separator before item
sep = sep.after                    // Separator after item
sep = sep.both                     // Both before and after
sep = sep.top                      // At top of menu
sep = sep.bottom                   // At bottom of menu
sep = sep.none                     // No separator
```

**pos** - Position in menu
```nss
pos = 0                            // Auto position
pos = pos.top                      // Top of menu
pos = pos.bottom                   // Bottom of menu
pos = pos.middle                   // Middle position
pos = 1                            // Specific index
```

**col** / **column** - Column layout (number)
```nss
col = 2                            // Display in 2 columns
column = 3                         // Display in 3 columns
```

**expanded** - Auto-expand submenu (boolean)
```nss
expanded = true                    // Auto-expand
expanded = false                   // Normal behavior
```

**checked** - Checkmark state (boolean or expression)
```nss
checked = true                     // Show checkmark
checked = expression               // Dynamic checkmark
```

**tip** - Tooltip text (string)
```nss
tip = "Tooltip text"
```

**keys** - Keyboard shortcut display (string)
```nss
keys = "Ctrl+C"
keys = "Shift+Del"
```

### Command Properties

**cmd** / **command** - Command to execute (string or expression)
```nss
cmd = 'notepad.exe'
cmd = @sel.path                    // Use selection path
command = 'explorer.exe'
```

**Command Variants** (mutually exclusive - cannot combine)
```nss
cmd-line = '/K dotnet run'         // cmd.exe with /K
cmd-prompt = '/K cmd'              // cmd.exe prompt
cmd-ps = 'Get-ChildItem'           // PowerShell
cmd-pwsh = 'Get-Process'           // PowerShell Core
cmd-shell = 'command'              // Shell command
cmd-open = 'path'                  // Open with default app
```

**args** / **arguments** - Command arguments (string)
```nss
args = '/select, @sel.path'
arguments = '@sel.path'
```

**admin** - Run as administrator (boolean or expression)
```nss
admin = true
admin = expression
```

**verb** - Shell verb (string)
```nss
verb = 'runas'
verb = 'properties'
```

**window** - Window state (identifier)
```nss
window = window.normal
window = window.hidden
window = window.minimized
window = window.maximized
```

**wait** - Wait for completion (boolean)
```nss
wait = true
wait = false
```

**invoke** - Invocation mode (identifier)
```nss
invoke = invoke.multiple           // Run for each selection
invoke = invoke.single             // Run once
```

**directory** / **dir** - Working directory (string)
```nss
directory = @sel.parent
dir = 'C:\temp'
```

### Modify Properties

**find** - Find pattern (string with wildcards)
```nss
find = "copy*"                     // Wildcard pattern
find = "paste"                     // Exact match
```

**menu** - Target menu name (string)
```nss
menu = "file manage"
menu = "Pin/Unpin"
```

**parent** - Parent menu (string)
```nss
parent = "main"
parent = "send to"
```

### Type Filter Details
Available type values:
- `file` - Regular files
- `dir` / `directory` - Directories
- `drive` - All drive types
- `fixed` - Fixed drives
- `usb` - USB drives
- `vhd` - Virtual hard drives
- `dvd` - DVD/CD drives
- `removable` - Removable drives
- `remote` - Network drives
- `namespace` - Shell namespace items
- `computer` - Computer namespace
- `recyclebin` - Recycle Bin
- `desktop` - Desktop background
- `taskbar` - Taskbar
- `back` - Any background context
- `edit` - Text edit fields
- `start` - Start menu

---

## Expression System

### Operators

**Arithmetic**
```nss
+  -  *  /  %                      // Basic math
```

**Comparison**
```nss
==  !=  >  <  >=  <=              // Comparison
```

**Logical**
```nss
&&  ||  !                          // And, Or, Not
and or not                         // Alternative syntax
```

**Ternary**
```nss
condition ? true_value : false_value
```

### Literals

**Numbers**
```nss
42                                 // Integer
3.14                              // Float
0xFF00FF                          // Hex color
```

**Strings**
```nss
"double quotes"
'single quotes'
```

**Colors**
```nss
#f00                              // RGB shorthand
#ff0000                           // RGB
#88ff0000                         // ARGB with alpha
```

**Arrays**
```nss
[1, 2, 3]
["a", "b", "c"]
[\uE253, #f00]                    // Image array
```

**Boolean**
```nss
true
false
```

**Null**
```nss
null
nil
```

---

## Built-in Functions

### sel.* (Selection)
Current file/folder selection state

```nss
sel.path                          // First selected path
sel.count                         // Number of selections
sel.type                          // Selection type (0=file, 1=dir, 2=drive)
sel.dir                           // Selection directory
sel.parent                        // Parent directory
sel.file                          // First file info
sel.file.name                     // File name with extension
sel.file.title                    // File name without extension
sel.file.ext                      // File extension
sel.files                         // All selected files (multi)
sel.dirs                          // All selected directories (multi)
```

### str.* (String Functions)
String manipulation

```nss
str.len(value)                    // String length
str.contains(str, substr)         // Check if contains substring
str.equals(str1, str2)            // String equality
str.split(str, delimiter)         // Split string
str.indexof(str, substr)          // Find substring index
str.replace(str, old, new)        // Replace substring
str.lower(str)                    // To lowercase
str.upper(str)                    // To uppercase
str.trim(str)                     // Trim whitespace
```

### io.* (File Operations)
File system operations

```nss
io.exists(path)                   // Check if file exists
io.rename(old, new)               // Rename file
io.copy(src, dest)                // Copy file
io.move(src, dest)                // Move file
io.delete(path)                   // Delete file
io.attributes(path)               // Get file attributes
io.size(path)                     // Get file size
```

### path.* (Path Functions)
Path manipulation

```nss
path.join(dir, file)              // Join path components
path.parent(path)                 // Get parent directory
path.name(path)                   // Get file name
path.title(path)                  // Get file name without extension
path.ext(path)                    // Get file extension
path.root(path)                   // Get root directory
path.normalize(path)              // Normalize path
```

### key.* (Keyboard State)
Keyboard key state during right-click

```nss
key.shift()                       // Shift key pressed
key.ctrl()                        // Ctrl key pressed
key.alt()                         // Alt key pressed
key.lbutton()                     // Left mouse button
key.rbutton()                     // Right mouse button
```

### wnd.* (Window State)
Window context detection

```nss
wnd.is_desktop()                  // Desktop window
wnd.is_taskbar()                  // Taskbar
wnd.is_explorer()                 // Explorer window
wnd.is_edit()                     // Edit field
wnd.is_tree()                     // Tree view
wnd.is_start()                    // Start menu
```

### sys.* (System Info)
System information

```nss
sys.dir                           // System directory
sys.prog                          // Program Files directory
sys.ver                           // Windows version
sys.is10                          // Windows 10 check
sys.is11                          // Windows 11 check
sys.is64                          // 64-bit Windows
```

### app.* (Application Info)
Nilesoft Shell application info

```nss
app.exe                           // Application executable path
app.cfg                           // Config file path
app.dir                           // Application directory
app.ver                           // Application version
```

### this.* (Current Item)
Properties of current menu item (used in modify)

```nss
this.id                           // Item ID
this.title                        // Item title
this.image                        // Item image
this.pos                          // Item position
this.parent                       // Parent menu
```

### image.* (Image Functions)
Image manipulation

```nss
image.color(color)                // Solid color
image.glyph(unicode, color)       // Glyph with color
image.svg(path)                   // Load SVG file
image.icon(path, index)           // Extract icon from file
```

### theme.* (Theme Access)
Access theme colors/values

```nss
theme.color                       // Current theme color
theme.dark                        // Is dark theme
theme.light                       // Is light theme
```

---

## Type System

### FileSystemObjects (FSO) Types

**Window Contexts:**
- `FSO_TITLEBAR` - Window title bar
- `FSO_EDIT` - Text edit control
- `FSO_START` - Start menu
- `FSO_TASKBAR` - Taskbar

**File System:**
- `FSO_DESKTOP` - Desktop background
- `FSO_DIRECTORY` - Directory/folder
- `FSO_FILE` - Regular file
- `FSO_DRIVE` - Any drive type
- `FSO_NAMESPACE` - Shell namespace item

**Drive Types:**
- `FSO_DRIVE_FIXED` - Fixed disk (HDD/SSD)
- `FSO_DRIVE_VHD` - Virtual hard drive
- `FSO_DRIVE_USB` - USB drive
- `FSO_DRIVE_DVD` - DVD/CD drive
- `FSO_DRIVE_REMOVABLE` - Removable media
- `FSO_DRIVE_REMOTE` - Network drive

**Namespace Types:**
- `FSO_NAMESPACE_COMPUTER` - This PC / Computer
- `FSO_NAMESPACE_RECYCLEBIN` - Recycle Bin
- `FSO_NAMESPACE_LIBRARIES` - Libraries

**Background Contexts:**
All above types have `FSO_BACK_*` variants for background (no selection) context

### Selection Modes
- `SelectionMode::None` - No specific mode
- `SelectionMode::Single` - Single item required
- `SelectionMode::MultiUnique` - Multiple items, same type
- `SelectionMode::MultiSingle` - Multiple items allowed
- `SelectionMode::Multiple` - Any multiple selection

### Visibility States
- `vis.remove` - Remove from menu
- `vis.hidden` - Hidden
- `vis.disable` - Disabled (grayed)
- `vis.label` - Non-clickable label
- `vis.static` - Static item
- `vis.extended` - Show with Shift only
- `inherit` - Inherit from parent

### Position Constants
- `pos.auto` / `0` - Auto position
- `pos.top` - Top of menu
- `pos.bottom` - Bottom of menu
- `pos.middle` - Middle position
- Numeric values - Specific index

### Separator Constants
- `sep.none` - No separator
- `sep.before` - Before item
- `sep.after` - After item
- `sep.both` - Both sides
- `sep.top` - Top of menu
- `sep.bottom` - Bottom of menu

---

## Image Syntax

### Unicode Glyphs
```nss
image = \uE253                    // Single glyph
image = [\uE253]                  // Array form
image = [\uE253, #f00]           // Glyph with color overlay
image = [\uE253, #f00, #00f]     // Glyph with multiple colors
```

**Common Glyph Fonts:**
- Default: `Nilesoft.Shell` (embedded icon font)
- System: `Segoe Fluent Icons` (Windows 11)
- System: `Segoe MDL2 Assets` (Windows 10)

### Color Blocks
```nss
image = #ff0000                   // Red color block
image = #88ff0000                 // Red with 50% transparency
image = #f00                      // Shorthand RGB
```

### File Paths
```nss
image = 'C:\path\to\icon.ico'
image = 'icon.png'                // Relative to config location
image = inherit                   // Inherit from parent menu
image = icon.path(@sel.path)      // Extract from file
```

### SVG Images
```nss
image = '<svg>...</svg>'          // Inline SVG
image = svg.file('path.svg')      // Load SVG file
```

### Image Arrays
```nss
image = [\uE253, #f00]           // Glyph + color
image = [#f00, \uE253]           // Color + glyph
image = ['icon.ico', #f00]       // File + tint color
```

### Special Values
```nss
image = inherit                   // Inherit from parent
image = null                      // No image
image = default                   // Default system image
```

---

## Command System

### Command Types (Mutually Exclusive)

**Standard Command:**
```nss
cmd = 'notepad.exe'
args = '@sel.path'
```

**CMD Variants:**
```nss
cmd-line = '/K dotnet run'        // Opens cmd with /K flag
cmd-prompt = '/K'                 // Opens cmd prompt
```

**PowerShell Variants:**
```nss
cmd-ps = 'Get-ChildItem'          // Windows PowerShell
cmd-pwsh = 'Get-Process'          // PowerShell Core
```

**Shell Commands:**
```nss
cmd-shell = 'explorer /select,@sel.path'
cmd-open = '@sel.path'            // Open with default app
```

### Command Modifiers

**Administrator Elevation:**
```nss
admin = true                      // Always run as admin
admin = key.shift()               // Run as admin if Shift pressed
```

**Window State:**
```nss
window = window.normal            // Normal window
window = window.hidden            // Hidden
window = window.minimized         // Minimized
window = window.maximized         // Maximized
```

**Working Directory:**
```nss
directory = @sel.parent           // Parent of selection
dir = 'C:\workspace'              // Fixed directory
```

**Wait for Completion:**
```nss
wait = true                       // Wait for process to exit
wait = false                      // Don't wait (default)
```

**Invocation Mode:**
```nss
invoke = invoke.multiple          // Run for each selected item
invoke = invoke.single            // Run once for all items
```

**Shell Verb:**
```nss
verb = 'runas'                    // Run as administrator
verb = 'properties'               // Show properties
verb = 'edit'                     // Edit with associated app
```

---

## Common Patterns

### Basic Menu Item
```nss
menu(type='file' where=sel.count>0 title='Tools' image=\uE253)
{
    item(title='Open in Notepad' cmd='notepad.exe' args='@sel.path')
    item(title='Copy Path' cmd=command.copy(sel.path))
    separator
    item(title='Properties' where=sel.count==1 cmd='properties')
}
```

### Conditional Visibility
```nss
// Show only for single text files
item(
    where = sel.count==1 && sel.file.ext=='.txt'
    title = 'Edit Text File'
    cmd = 'notepad.exe'
)

// Show only when Shift is pressed
item(
    where = key.shift()
    title = 'Advanced Options'
    vis = vis.extended
)

// Show for multiple selections
item(
    where = sel.count > 1
    title = 'Process @(sel.count) files'
    mode = 'multiple'
)
```

### Dynamic Titles
```nss
item(title = '@sel.file.name')
item(title = 'Selected: @(sel.count) items')
item(title = '@if(sel.count==1, "One file", "@(sel.count) files")')
```

### Modify System Items
```nss
// Change position of system item
modify(where = this.id==id.copy_as_path)
{
    pos = top
    image = \uE8C8
}

// Hide system items by pattern
modify(find = 'share*' vis = vis.remove)

// Move to submenu
modify(find = 'pin*' menu = 'organize')
```

### Multiple File Types
```nss
menu(type='file|dir' where=sel.count>0)
{
    // This menu shows for both files and directories
}

menu(type='file' where=sel.file.ext in ['.jpg', '.png', '.gif'])
{
    // Image files only
}
```

### Command Patterns
```nss
// Run as admin conditionally
item(
    title = 'Regedit'
    cmd = 'regedit.exe'
    admin = true
)

// PowerShell command
item(
    title = 'List Items'
    cmd-ps = 'Get-ChildItem | Format-Table'
)

// CMD with /K to keep window open
item(
    title = 'Build Project'
    cmd-line = '/K dotnet build'
    directory = @sel.parent
)
```

### Variables
```nss
// Global variables
$editor = 'code.exe'
$workspace = 'C:\projects'

// Use in items
item(title='Open in VSCode' cmd=$editor args='@sel.path')
```

### Localization
```nss
// In lang/en.nss
file_manage = "File Management"
copy_path = "Copy Path"

// In shell.nss
menu(title=loc.file_manage)
{
    item(title=loc.copy_path cmd=command.copy(sel.path))
}
```

### Arrays and Loops
```nss
// Array of extensions
$image_exts = ['.jpg', '.png', '.gif', '.bmp']

// Check if selection is in array
where = sel.file.ext in $image_exts
```

---

## Validation Rules

### Required Properties
- Menu/Item MUST have either `title` OR `image` (at least one required)
- Separator doesn't require title or image

### Mutually Exclusive Properties
Cannot combine these command properties:
- `cmd`, `cmd-line`, `cmd-prompt`, `cmd-ps`, `cmd-pwsh`, `cmd-shell`, `cmd-open`
- Use only ONE command type per item

### Type Compatibility
- `mode='single'` incompatible with `where=sel.count>1`
- `type='file'` requires file system context (not taskbar/desktop alone)
- Background types (`type='back'`) require no selection

### Expression Types
- `where` must evaluate to boolean
- `title` evaluates to string
- `image` evaluates to image expression or null
- Numeric properties (`pos`, `col`) must be numbers

### Identifier Scope
- Variables: `$name` for user-defined
- System: `sel.*`, `sys.*`, `key.*` etc.
- Constants: `vis.*`, `pos.*`, `sep.*`, `window.*`

---

## Implementation Notes

### Parser Behavior
- Case-insensitive identifiers (hashed with lowercase)
- Comments: `//` single-line, `/* */` multi-line
- String quotes: Both `"` and `'` supported
- Escape sequences: `\n`, `\t`, `\uXXXX` (Unicode)
- Line continuation: Expressions can span multiple lines

### Hash System
All identifiers converted to FNV-1a hashes:
- `offset_basis = 5381`
- `prime = 33`
- Case-insensitive by default

Key identifier hashes (const uint32_t):
```cpp
IDENT_SEL      = 0x0B88A989U
IDENT_STR      = 0x0B88AB7EU
IDENT_PATH     = 0x7C9C25F2U
IDENT_WHERE    = 0x10A32840U
IDENT_TITLE    = 0x106DAA27U
IDENT_IMAGE    = 0x0FA87CA8U
IDENT_TYPE     = 0x7C9EBD07U
MENU_VIS       = 0x0B88B6D7U
```

### Expression Evaluation
- Runtime context includes: Selections, Theme, Keyboard, Window
- Variables scoped: global → runtime → local
- Lazy evaluation: `where` clauses evaluated at display time
- Type coercion: String ↔ Number automatic when needed

### Image Rendering
- D2D1 (Direct2D) for rendering
- SVG via PlutoSVG library
- Glyphs from embedded font or system fonts
- DPI-aware scaling throughout

### Performance Notes
- `where` expressions evaluated for every menu display
- Complex expressions in tight loops can slow menu display
- Image caching: Repeated images cached automatically
- Static menus pre-built, dynamic menus built on-demand

### Error Handling
- Parser stops on first error
- Line/column tracking for precise error location
- TokenError enum with 100+ specific error types
- Errors logged to Nilesoft Shell log

### Best Practices
1. Keep `where` expressions simple for performance
2. Use variables for repeated values
3. Prefer `modify()` over recreating system menus
4. Use `vis.remove` instead of complex `where` to hide items
5. Group related items in submenus
6. Use localization for multi-language support
7. Test with different DPI settings
8. Avoid deep nesting (>3 levels) for UX

### Limits
- Max identifier length: 50 parts (e.g., `a.b.c...`)
- Max menu nesting: No hard limit, but UX degrades
- Property value size: Limited by available memory
- Image file size: Recommended <1MB for performance

---

## Quick Reference Card

### Most Common Properties
```nss
title    = "text" | expression
image    = \uEXXX | #color | 'path' | [\uEXXX, #color]
where    = boolean_expression
type     = 'file|dir|drive|back|taskbar|desktop|namespace'
mode     = 'single' | 'multiple'
cmd      = 'executable' | expression
args     = "arguments" | expression
vis      = vis.remove | vis.hidden | vis.disable | inherit
sep      = sep.before | sep.after | sep.both | sep.none
pos      = 0 | pos.top | pos.bottom | number
admin    = true | false | expression
```

### Most Common Where Clauses
```nss
where = sel.count > 0              // Any selection
where = sel.count == 1             // Single selection
where = sel.count > 1              // Multiple selection
where = sel.type == 1              // Directories
where = sel.type == 0              // Files
where = key.shift()                // Shift pressed
where = !wnd.is_taskbar            // Not on taskbar
where = sel.file.ext == '.txt'     // Specific extension
```

### Common Image Patterns
```nss
image = \uE253                     // Simple glyph
image = [\uE253, #f00]            // Colored glyph
image = #ff0000                    // Color block
image = inherit                    // Inherit from parent
image = 'icon.ico'                 // File path
```

---

**End of Reference**

*This file generated from complete source code analysis of Nilesoft Shell.*
*Last updated: 2025-12-07*
*For use by AI assistants to maintain context across sessions.*
