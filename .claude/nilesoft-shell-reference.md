# Nilesoft Shell Complete Reference

> Auto-generated knowledge base for AI assistants. Read this file at session start for instant Nilesoft Shell mastery.

## Table of Contents

1. [Syntax Fundamentals](#syntax-fundamentals)
1. [Properties Reference](#properties-reference)
1. [Expression System](#expression-system)
1. [Built-in Functions - Complete List](#built-in-functions---complete-list)
1. [Undocumented Functions](#undocumented-functions)
1. [Type System](#type-system)
1. [Image Syntax](#image-syntax)
1. [Command System](#command-system)
1. [Common Patterns](#common-patterns)
1. [Validation Rules](#validation-rules)
1. [Implementation Notes](#implementation-notes)

-----

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

### Operators

```nss
// Arithmetic
+  -  *  /  %

// Comparison
==  !=  >  <  >=  <=

// Logical
&&  ||  !
and or not                          // Alternative syntax

// Bitwise
&  |  ^  ~  <<  >>

// Ternary
condition ? true_value : false_value

// Assignment
=  +=  -=  *=  /=
```

### Escape Sequences

```nss
\n    // Newline
\r    // Carriage return
\t    // Tab
\q    // Quote
\a    // Alert
\x    // Hex byte
\u    // Unicode (4 digits)
\U    // Unicode (8 digits)
```

-----

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

-----

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

**Bitwise**

```nss
&  |  ^  ~  <<  >>                // Bitwise operations
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

-----

## Built-in Functions - Complete List

> This is the authoritative function list derived from Verification.cpp (2049 lines) - the source of truth for all recognized functions.

### STR.* (String Functions)

```nss
str.null(str)              str.empty(str)           str.len(str)
str.length(str)            str.upper(str)           str.lower(str)
str.capitalize(str)        str.hash(str)            str.trim(str, [chars])
str.trimstart(str, [chars]) str.trimend(str, [chars]) str.set(str, index)
str.get(str, index)        str.char(str, index)     str.at(str, index)
str.left(str, count)       str.right(str, count)    str.sub(str, start, [len])
str.eq(str, str2, [case])  str.equals(str, str2, [case])
str.not(str, str2, [case]) str.start(str, prefix, [case])
str.end(str, suffix, [case]) str.find(str, needle, [start])
str.findlast(str, needle, [start]) str.contains(str, needle, [case])
str.remove(str, what, [count]) str.padleft(str, len, [char])
str.padright(str, len, [char]) str.padding(str, len, [char])
str.replace(str, old, new, [count]) str.format(fmt, ...)
str.guid([format])         str.join(sep, ...)       str.split(str, sep)
str.tag(str, tag, [close]) str.res(str, [default])
str.decode.url(str)        str.encode.url(str)
```

**String Extension Methods** (hasdot = true, used with dot notation on variables):

```nss
var.null                   var.empty                var.len
var.length                 var.upper                var.lower
var.capitalize             var.trim([chars])        var.trimstart([chars])
var.trimend([chars])       var.set(index)           var.get(index)
var.char(index)            var.at(index)            var.left(count)
var.right(count)           var.sub(start, [len])    var.eq(str, [case])
var.equals(str, [case])    var.not(str, [case])     var.start(prefix, [case])
var.end(suffix, [case])    var.find(needle, [start]) var.findlast(needle, [start])
var.contains(needle, [case]) var.remove(what, [count])
var.padleft(len, [char])   var.padright(len, [char]) var.padding(len, [char])
var.replace(old, new, [count]) var.format(...)
```

### SEL.* (Selection Functions)

```nss
sel.mode()                 sel.type([index])        sel.get(index, [prop])
sel.path([index])          sel.full([index])        sel.dir([index])
sel.directory([index])     sel.short([index])       sel.raw([index])
sel.name([index])          sel.title([index])       sel.item([index])
sel.back                   sel.count                sel.len / sel.length
sel.root                   sel.readonly             sel.hidden
sel.workdir([path])        sel.curdir([path])       sel.currentdirectory([path])
sel.tofile([path, sep, quote])                      // Write selection to file
sel.parent([index])        sel.location([index])    
sel.index(var, [count])    sel.i(var, [count])      // Index-based selection access
sel.file([index])          sel.files([sep, quote])
sel.paths([sep, quote])    sel.dirs([sep, quote])   sel.directories([sep, quote])
sel.types([sep])           sel.shorts([sep])        sel.titles([sep])
sel.names([sep])           sel.exts([sep])          sel.roots([sep])
sel.drivers([sep])         sel.namespaces([sep])    sel.quote([quote])
sel.lnk([index])           sel.lnktarget([index])   sel.meta([index, prop])
```

**Selection Sub-properties** (dot notation):

```nss
// sel.path sub-properties
sel.path.len               sel.path.title           sel.path.name
sel.path.quote

// sel.file sub-properties  
sel.file.ext               sel.file.title           sel.file.name

// sel.files/dirs/paths count
sel.files.count            sel.dirs.count           sel.paths.count

// sel.lnk (shortcut) sub-properties
sel.lnk.dir                sel.lnk.location         sel.lnk.target
sel.lnk.icon               sel.lnk.desc             sel.lnk.args
sel.lnk.workdir            sel.lnk.window
```

### PATH.* (Path Functions)

```nss
// Basic path operations
path.join(part1, ...)      path.combine(part1, ...)
path.parent(path)          path.name(path)          path.title(path)
path.ext(path)             path.root(path)          path.normalize(path)
path.short(path)           path.long(path)          path.full(path)

// File operations
path.exists(path)          path.open(path)
path.file.box([title, path, filter, ext])          // File picker dialog
path.file.title(path)      path.file.name(path)
path.file.ext(path)

// Directory operations
path.dir.exists(path)      path.dir.parent(path)
path.dir.name(path)        path.dir.title(path, ?)
path.dir.make()            path.dir.box([title, path, new, show])  // Directory picker
path.dir.files([path, pattern])

// File listing
path.files([path, pattern, recursive, full, dirs])
path.files.name(path, pattern)
path.files.title(path, pattern)
path.files.count()

// Type checking
path.type(path, [?, ?])    path.isabsolute(path)
path.isrelative(path)      path.isfile(path)
path.isdir(path)           path.isdirectory(path)
path.isdrive(path)         path.isroot(path)
path.isnamespace(path)     path.isclsid(path)
path.isexe(path)

// Utilities
path.removeextension(path) path.getknownfolder(path)
path.absolute(path)        path.relative(path)
path.wsl(path)             path.refresh([path])
path.compact(path, [len])  path.quote_spaces(path)
path.remove_args(path)     path.package(path)
path.rename_extension(path, ext)
path.add_extension(path, ext)

// Shortcut creation (MAJOR UNDOCUMENTED FEATURE)
path.lnk.create(target, lnk_path, [desc, icon, iconindex, args, workdir, window])
    // Parameters:
    // target      - Required: Target path the shortcut points to
    // lnk_path    - Required: Where to save the .lnk file
    // desc        - Optional: Description/comment
    // icon        - Optional: Icon path
    // iconindex   - Optional: Icon index within file
    // args        - Optional: Command line arguments
    // workdir     - Optional: Working directory
    // window      - Optional: Window state (normal/min/max)
```

### IO.* (File Operations)

```nss
io.exists(path)            io.rename(old, new)      io.copy(src, dest)
io.move(src, dest)         io.delete(path)          io.attributes(path)
io.size(path)              io.meta(path, [prop])    io.dt([path, year, month, ...])
io.datetime([path, ...])

// File creation/writing
io.file.create(path, content)
io.file.write(path, content)
io.file.append(path, content)
io.file.read(path)
```

### REG.* (Registry Functions)

```nss
reg.get(key, [name, default])
reg.exists(key, [name, type])
reg.delete(key, [name])
reg.set(key, [name, value, type])
reg.keys(key)              reg.values(key)

// Registry types (constants)
reg.none                   reg.sz                   reg.expand
reg.expand_sz              reg.multi_sz             reg.multi
reg.dword                  reg.qword                reg.binary
```

### INI.* (INI File Functions)

```nss
ini.get(file, section, key)
ini.exists(file, section, key)
ini.set(file, section, key, value)
```

### CLIPBOARD.* (Clipboard Functions)

```nss
clipboard([text])          clipboard.empty          clipboard.is_empty
clipboard.is_bitmap        clipboard.len            clipboard.length
clipboard.get()            clipboard.set(text)
```

### INPUT() (Dialog Box)

```nss
input([title, text, default])
input.result()
```

### KEY.* (Keyboard State)

```nss
key.shift()                key.ctrl()               key.alt()
key.lbutton()              key.rbutton()            key.mbutton()
key.xbutton1()             key.xbutton2()
key.back()                 key.tab()                key.return()
key.escape()               key.space()              key.prior()
key.next()                 key.end()                key.home()
key.left()                 key.up()                 key.right()
key.down()                 key.insert()             key.delete()
key.numlock()              key.scroll()             key.capital()
```

### SYS.* (System Info)

```nss
// System checks
sys.is64                   sys.is11                 sys.is10
sys.is7orgreater           sys.is8orgreater         sys.is10orgreater
sys.dark                   // Dark mode detection

// System paths
sys.root                   sys.path                 sys.bin
sys.dir                    sys.prog                 sys.temp
sys.appdata                sys.users

// Version info
sys.ver.major              sys.ver.minor            sys.ver.build

// Date/time
sys.datetime.time          sys.datetime.date        sys.datetime.hour
sys.datetime.minute        sys.datetime.second      sys.datetime.day
sys.datetime.month         sys.datetime.year        sys.datetime.dayofweek

// Language/locale
sys.lang.id                sys.lang.name            sys.lang.country
sys.loc.id                 sys.loc.name             sys.loc.country

// Environment
sys.var(name)              sys.expand(str)
```

### WINDOW.* / WND.* (Window Functions)

```nss
window.post(hwnd, msg, wparam, [lparam])
window.send(hwnd, msg, wparam, [lparam])
window.command([cmd, wparam])
window.handle              window.name              window.title
window.owner               window.parent
window.owner.handle        window.owner.name        window.owner.title
window.parent.handle       window.parent.name       window.parent.title

// Context checks
window.is_desktop          window.is_taskbar        window.is_explorer
window.is_nav              window.is_tree           window.is_side
window.is_edit             window.is_start          window.is_titlebar
window.is_contextmenuhandler

window.module              window.instance          window.id
```

### PROCESS.* (Process Functions)

```nss
process.id                 process.handle           process.name
process.path               process.is_explorer      process.used
process.is_started(name)
```

### APP.* (Application Info)

```nss
app.about                  app.cfg                  app.dir
app.dll                    app.exe                  app.reload
app.unload                 app.ver.major            app.ver.minor
app.ver.build              app.var                  app.used
app.process
```

### USER.* (User Paths)

```nss
user.home                  user.appdata             user.localappdata
user.desktop               user.documents           user.downloads
user.music                 user.pictures            user.videos
user.favorites             user.recent              user.sendto
user.startmenu             user.startup             user.templates
user.expand(str)
```

### FONT.* (Font Functions)

```nss
font.icon                  font.segoe_fluent_icons  font.segoe_mdl2
font.segoe_ui_symbol       font.segoe_ui            font.segoe_ui_emoji
font.segoe_ui_historic     font.webdings            font.wingdings
font.wingdings2            font.wingdings3          font.size
font.scale                 font.system(name)        font.loaded(name)
font.exists(name)
```

### THIS.* (Context Functions)

```nss
this.id([index])           this.title([index])      this.name([index])
this.parent                this.parent.id([index])
this.parent.name([index])  this.parent.title([index])
this.verb([index])         this.disabled([index])   this.type([index])
this.checked([index])      this.pos([index])        this.count([index])
this.sys([index])          this.level([index])      this.isuwp([index])
this.clsid([index])
```

### IMAGE.* / ICON.* (Image/Icon Functions)

```nss
icon([code, color, size])  image([code, color, size])
icon.auto([code, color, size]) icon.default([code, color, size])
icon.glyph(code, [color, size, font])
icon.fluent(code, [color, size]) icon.mdl(code, [color, size])
icon.segoe(code, [color, size])
icon.svg(path)             icon.svgf(path)
icon.res(path, [index])    icon.box([title, default])  // Icon picker dialog
icon.change(...)           icon.color1              icon.color2
icon.color3

img.*                      // Similar functions for image resources
```

### COLOR.* (Color Functions)

```nss
color([value])             color.box([title, default])  // Color picker dialog
color.random([min, max])   color.adjust(color, [amount])
color.opacity(color, [alpha])
color.r(color)             color.g(color)           color.b(color)
color.a(color)             color.rgb(r, g, b)       color.argb(a, r, g, b)
color.invert(color)        color.darken(color, amount)
color.lighten(color, amount)

// Named colors
color.red                  color.green              color.blue
color.white                color.black              color.transparent
// ... (many more named colors)
```

### REGEX.* (Regular Expression Functions)

```nss
regex.match(text, pattern)
regex.matches(text, pattern)
regex.replace(text, pattern, replacement)
```

### COMMAND.* (Navigation Commands)

```nss
command.copy(text)         command.paste            command.cut
command.delete             command.selectall        command.undo
command.redo               command.navigate(path)   command.goback
command.goforward          command.gohome           command.refresh
command.properties         command.rename
```

### PACKAGE.* / UWP.* / APPX.* (UWP Functions)

```nss
package.id(name)           package.path(name)       package.name(name)
package.title(name)        package.family(name)     package.run(name)
package.launch(name)       package.shell(name)      package.version(name)
package.ver(name)          package.exists(name)     package.list([filter])
uwp.*                      appx.*                   // Aliases for package.*
```

### MSG.* (Message Functions)

```nss
msg.ok(text, [title])      msg.info(text, [title])
msg.warn(text, [title])    msg.error(text, [title])
msg.yesno(text, [title])   msg.okcancel(text, [title])
msg.result                 msg.yes                  msg.no
msg.ok                     msg.cancel
```

### Utility Functions (Global)

```nss
eval(expr)                 print(value)             quote(str)
char(code)                 len(value)               length(value)
tohex(value)               toint(value)             todouble(value)
touint(value)              tofloat(value)
not([value])               null([value])            nil([value])
random([min, max])         indexof(array, value, [start])
if(condition, [true, false])
for(init, condition, [increment])
foreach(var, array, action)
break                      continue
equal(a, b)                greater(a, b)            less(a, b)
equals(...)                shl(value, bits)         shr(value, bits)
invoke.cancel()
```

-----

## Undocumented Functions

> These functions exist in Verification.cpp but have minimal or no official documentation.

### CONFIRMED UNDOCUMENTED (verified in source code):

|Function                    |Parameters|Purpose                                                       |
|----------------------------|----------|--------------------------------------------------------------|
|`path.lnk.create()`         |2-8 params|**Creates Windows shortcuts programmatically** - MAJOR feature|
|`sel.tofile()`              |0-3 params|Writes selection paths to a file                              |
|`sel.index()` / `sel.i()`   |1-2 params|Index-based selection access                                  |
|`str.hash()`                |1 param   |String hashing                                                |
|`str.split()`               |2 params  |Split string by separator                                     |
|`str.join()`                |2+ params |Join strings with separator                                   |
|`str.tag()`                 |2-3 params|Wrap string in tags                                           |
|`str.decode.url()`          |1 param   |URL decode                                                    |
|`str.encode.url()`          |1 param   |URL encode                                                    |
|`path.compact()`            |1-2 params|Compact path display                                          |
|`path.refresh()`            |0-1 params|Refresh path                                                  |
|`path.absolute()`           |1 param   |Get absolute path                                             |
|`path.relative()`           |1 param   |Get relative path                                             |
|`path.quote_spaces()`       |1 param   |Quote path if contains spaces                                 |
|`path.remove_args()`        |1 param   |Remove arguments from path                                    |
|`path.rename_extension()`   |2 params  |Rename file extension                                         |
|`path.add_extension()`      |2 params  |Add extension to path                                         |
|`user.expand()`             |1 param   |Expand user path variables                                    |
|`app.var`                   |-         |Application variable                                          |
|`app.used`                  |-         |Application used status                                       |
|`app.process`               |-         |Application process info                                      |
|`font.*` (entire namespace) |various   |Font functions - limited docs                                 |
|`process.*` (most functions)|various   |Process functions - limited docs                              |

### Usage Examples for Undocumented Functions:

**path.lnk.create() - Create shortcuts:**

```nss
// Basic shortcut
path.lnk.create(sel.path, user.desktop + '\\My Shortcut.lnk')

// Full parameters
path.lnk.create(
    'C:\\Program Files\\App\\app.exe',    // target
    user.desktop + '\\App.lnk',           // shortcut location
    'My Application',                      // description
    'C:\\icons\\app.ico',                 // icon path
    0,                                     // icon index
    '--arg1 --arg2',                      // arguments
    'C:\\Program Files\\App',             // working directory
    window.normal                          // window state
)
```

**sel.tofile() - Export selection:**

```nss
// Write selection to file
sel.tofile(user.desktop + '\\selection.txt', '\n', '"')
```

**str.split() and str.join():**

```nss
// Split a path
$parts = str.split(sel.path, '\\')

// Join with separator
$result = str.join(', ', 'a', 'b', 'c')
```

-----

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

-----

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

-----

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

-----

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

-----

## Validation Rules

### Required Properties

- Menu/Item MUST have either `title` OR `image` (at least one required)
- Separator doesn’t require title or image

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

-----

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
1. Use variables for repeated values
1. Prefer `modify()` over recreating system menus
1. Use `vis.remove` instead of complex `where` to hide items
1. Group related items in submenus
1. Use localization for multi-language support
1. Test with different DPI settings
1. Avoid deep nesting (>3 levels) for UX

### Limits

- Max identifier length: 50 parts (e.g., `a.b.c...`)
- Max menu nesting: No hard limit, but UX degrades
- Property value size: Limited by available memory
- Image file size: Recommended <1MB for performance

-----

## Source Code Reference

### Key Files (from github.com/moudey/Shell)

- **Verification.cpp** (2049 lines) - THE authoritative function registry
- **FuncExpression.cpp** - Function implementations
- **Lexer.cpp** - Tokenizer/lexical analysis
- **Parser.cpp** - Expression parsing
- **ContextMenu.cpp** (~3,300 lines) - Menu rendering with Direct2D/GDI
- **Main.cpp** - DLL entry, API hooking
- **Initializer.cpp** - Configuration loading
- **Selections.cpp** - File/folder selection handling

### Technical Stack

- COM interfaces (IContextMenu, IShellBrowser, IShellItem)
- Detours-style API hooking for TrackPopupMenu
- PlutoVG for SVG rendering
- Direct2D and DirectWrite for modern rendering
- UxTheme for Windows theming

-----

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

-----

**End of Reference**

*This file generated from complete source code analysis of Nilesoft Shell.*
*Source: Verification.cpp (2049 lines) - the authoritative function registry*
*Last updated: 2025-12-09*
*For use by AI assistants to maintain context across sessions.*