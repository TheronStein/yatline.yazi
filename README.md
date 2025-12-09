# yatline.yazi

A customizable status line and header line plugin for Yazi file manager.

> **Note**: This is a fork updated for **Yazi nightly** API compatibility.

## Installation

Add to your `~/.config/yazi/package.toml`:

```toml
[plugin]
prepend = [
    { use = "TheronStein/yatline" }
]
```

Then run:
```bash
ya pack -i
```

## Configuration

Add to your `~/.config/yazi/init.lua`:

### Minimal Configuration

```lua
require("yatline"):setup({})
```

### Basic Configuration

```lua
require("yatline"):setup({
    show_background = false,

    display_header_line = true,
    display_status_line = true,

    -- Component layout order (default includes native Tabs component)
    component_positions = { "header", "tabs", "tab", "status" },

    header_line = {
        left = {
            section_a = {},
            section_b = {},
            section_c = {
                { type = "string", custom = false, name = "tab_path" },
            },
        },
        right = {
            section_a = {},
            section_b = {},
            section_c = {
                { type = "coloreds", custom = false, name = "count" },
            },
        },
    },

    status_line = {
        left = {
            section_a = {
                { type = "string", custom = false, name = "tab_mode" },
            },
            section_b = {
                { type = "string", custom = false, name = "hovered_size" },
            },
            section_c = {
                { type = "string", custom = false, name = "hovered_name" },
            },
        },
        right = {
            section_a = {
                { type = "string", custom = false, name = "cursor_position" },
            },
            section_b = {
                { type = "string", custom = false, name = "cursor_percentage" },
            },
            section_c = {
                { type = "coloreds", custom = false, name = "permissions" },
            },
        },
    },
})
```

### Full Configuration with Styling

```lua
require("yatline"):setup({
    section_separator = { open = "", close = "" },
    part_separator = { open = "", close = "" },
    inverse_separator = { open = "", close = "" },

    style_a = {
        fg = "black",
        bg_mode = {
            normal = "#7aa2f7",
            select = "#e0af68",
            un_set = "#f7768e",
        },
    },
    style_b = { bg = "#3b4261", fg = "#c0caf5" },
    style_c = { bg = "#1a1b26", fg = "#a9b1d6" },

    permissions_t_fg = "#9ece6a",
    permissions_r_fg = "#e0af68",
    permissions_w_fg = "#f7768e",
    permissions_x_fg = "#7dcfff",
    permissions_s_fg = "#c0caf5",

    tab_width = 20,

    selected = { icon = "󰻭", fg = "#e0af68" },
    copied = { icon = "", fg = "#9ece6a" },
    cut = { icon = "", fg = "#f7768e" },

    total = { icon = "󰮍", fg = "#e0af68" },
    succ = { icon = "", fg = "#9ece6a" },
    fail = { icon = "", fg = "#f7768e" },
    found = { icon = "󰮕", fg = "#7aa2f7" },
    processed = { icon = "󰐍", fg = "#9ece6a" },

    show_background = true,

    display_header_line = true,
    display_status_line = true,

    component_positions = { "header", "tabs", "tab", "status" },

    header_line = {
        left = {
            section_a = {},
            section_b = {},
            section_c = {
                { type = "string", custom = false, name = "tab_path" },
            },
        },
        right = {
            section_a = {},
            section_b = {},
            section_c = {
                { type = "coloreds", custom = false, name = "count" },
            },
        },
    },

    status_line = {
        left = {
            section_a = {
                { type = "string", custom = false, name = "tab_mode" },
            },
            section_b = {
                { type = "string", custom = false, name = "hovered_size" },
            },
            section_c = {
                { type = "string", custom = false, name = "hovered_name" },
            },
        },
        right = {
            section_a = {
                { type = "string", custom = false, name = "cursor_position" },
            },
            section_b = {
                { type = "string", custom = false, name = "cursor_percentage" },
            },
            section_c = {
                { type = "coloreds", custom = false, name = "permissions" },
            },
        },
    },
})
```

## Available Components

### String Components (`type = "string"`)

| Name | Description | Params |
|------|-------------|--------|
| `tab_mode` | Current mode (NORMAL/SELECT/UN-SET) | - |
| `tab_path` | Current directory path | `{ trimed, max_length, trim_length }` |
| `tab_num_files` | Number of files in current directory | - |
| `hovered_name` | Name of hovered file | `{ trimed, max_length, trim_length, show_symlink }` |
| `hovered_path` | Full path of hovered file | `{ trimed, max_length, trim_length }` |
| `hovered_size` | Size of hovered file | - |
| `hovered_mime` | MIME type of hovered file | - |
| `hovered_ownership` | User:group of hovered file | - |
| `hovered_file_extension` | Extension of hovered file | `{ show_icon }` |
| `cursor_position` | Cursor position (N/M) | - |
| `cursor_percentage` | Scroll percentage | - |
| `date` | Current date/time | `{ format }` (e.g., `"%Y-%m-%d"`) |

### Coloreds Components (`type = "coloreds"`)

| Name | Description | Params |
|------|-------------|--------|
| `permissions` | File permissions with colors | - |
| `count` | Selected/yanked file counts | `{ show_files }` |
| `task_states` | Task total/success/failed | - |
| `task_workload` | Task progress percentage | - |

### Line Components (`type = "line"`)

| Name | Description | Params |
|------|-------------|--------|
| `tabs` | Tab list (if using yatline tabs instead of native) | `{ "left" }` or `{ "right" }` |

### Custom Components

You can add custom text:

```lua
{ type = "string", custom = true, name = "My Custom Text" }
```

## Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `show_background` | boolean | `false` | Show background colors |
| `display_header_line` | boolean | `true` | Show header line |
| `display_status_line` | boolean | `true` | Show status line |
| `tab_width` | number | `20` | Max width for tab names |
| `tab_name_format` | string/function | `nil` | Tab name display format (see below) |
| `component_positions` | table | `{"header", "tabs", "tab", "status"}` | Layout order |
| `section_separator` | table | `{ open = "", close = "" }` | Section separators |
| `part_separator` | table | `{ open = "", close = "" }` | Part separators |
| `inverse_separator` | table | `{ open = "", close = "" }` | Inverse separators |

### Tab Name Format

Control how tab names are displayed in the tab bar:

| Value | Description |
|-------|-------------|
| `nil` or `"default"` | Use Yazi's default tab name |
| `"basename"` | Show only the directory basename |
| `"index"` | Show only the tab index number |
| `"index:basename"` | Show index followed by basename (e.g., `1:projects`) |
| `function(tab, index)` | Custom function for full control |

**Custom function example:**

```lua
require("yatline"):setup({
    tab_name_format = function(tab, index)
        local cwd = tostring(tab.cwd)
        -- Show a custom icon based on path
        if cwd:match("/projects/") then
            return "" .. " " .. cwd:match("([^/]+)/?$")
        end
        return tab.name
    end,
    -- ... other config
})
```

## Sections

Each line (header/status) has left and right sides, each with three sections:

```
[ section_a | section_b | section_c ... ... section_c | section_b | section_a ]
```

- `section_a`: Primary section (uses `style_a`, mode-aware background)
- `section_b`: Secondary section (uses `style_b`)
- `section_c`: Tertiary section (uses `style_c`)

## Credits

- Original: [imsi32/yatline.yazi](https://github.com/imsi32/yatline.yazi)
- [Lualine](https://github.com/nvim-lualine/lualine.nvim) for design inspiration
- [Yazi](https://github.com/sxyazi/yazi)

## License

MIT
