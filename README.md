# GetLib - A Modern LibStub Replacement with Semantic Versioning

GetLib is a drop-in replacement for LibStub that adds full semantic versioning support for World of Warcraft libraries. It maintains full backward compatibility with existing LibStub libraries while offering modern versioning capabilities.

## Features

- **Semantic Versioning**: Uses full `Major.Minor.Patch` versioning instead of just major version numbers
- **Version Matching**: Libraries can request specific version ranges that best match their requirements
- **Backward Compatibility**: Automatically converts and works with all existing LibStub libraries
- **Forward Compatibility**: Libraries designed for GetLib can use semantic versioning while still working with addons using LibStub
- **No Breaking Changes**: Addons using older versions of libraries will continue to work even when newer versions are present

## Why GetLib?

LibStub has been the standard library management system for WoW addons for years, but it has limitations:

- LibStub only uses simple numbered versions (just a major version)
- LibStub always loads the newest library version, which can break addons requiring older API
- No way to request specific version patterns or ranges

GetLib solves these issues while maintaining compatibility with the existing ecosystem.

## Usage

### Including GetLib in your addon

Add GetLib to your addon's directory structure (typically in a Libs folder) and include it in your TOC and XML files:

```xml
<!-- In your embeds.xml or equivalent -->
<Include file="Libs\RasuForge-GetLib\lib.xml"/>
```

### Registering a Library

```lua
-- Register a library with semantic versioning
local MyLib = GetLib:RegisterLibrary("MyLibrary", "1.2.3", {})
if not MyLib then
    -- Library is already registered with this version.
    return
end

-- Add functions to your lib
function MyLib:DoSomething()
    -- Implementation
end
```

### Getting a Library

```lua
-- Get the latest version of a library
local MyLib = GetLib:GetLibrary("MyLibrary")

-- Get a specific version
local MyLib = GetLib:GetLibrary("MyLibrary", "1.2.3")

-- Get the latest version in a major version
local MyLib = GetLib:GetLibrary("MyLibrary", "1.*")

-- Get the latest version in a major.minor version
local MyLib = GetLib:GetLibrary("MyLibrary", "1.2.*")

-- Shorthand syntax
local MyLib = GetLib("MyLibrary", "1.2.*")
```

### Version Patterns

GetLib supports flexible version matching:

- `*` - Any version (latest available)
- `1.*` - Latest with major version 1
- `1.2.*` - Latest with major version 1 and minor version 2
- `1.2.3` - Exact version match

## Compatibility with LibStub

GetLib automatically converts all existing LibStub libraries upon initialization:

```lua
-- Existing LibStub code continues to work
local oldLib = LibStub("OldLibrary")

-- But can now also be accessed through GetLib
local sameLib = GetLib("OldLibrary")
```

When GetLib loads, it converts LibStub to use GetLib internally, so all libraries remain accessible through either API.

## Migration from LibStub

For addon developers using LibStub, simply replace:

```lua
local MyLib = LibStub("MyLibrary")
```

with:

```lua
local MyLib = GetLib("MyLibrary")
```

For library developers, you can start using semantic versioning:

```lua
-- Old LibStub way
local MyLib, oldMinor = LibStub:NewLibrary("MyLibrary", 3)

-- New GetLib way
local MyLib = GetLib:RegisterLibrary("MyLibrary", "1.2.3")
```

## Iterating Libraries

```lua
for name, library in GetLib:IterateLibraries() do
    -- Do something with each library
end
```

## License

This library is released under the [MIT License](https://opensource.org/licenses/MIT).

## Credits

GetLib is developed as part of the RasuForge addon framework. 
