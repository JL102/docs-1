---
id: v3_migration
title: Migration to v3.0
---

# Migrating from v2.x to v3.0

:::danger

AstroNvim requires the migration to Neovim v0.9 to use the new APIs and features. Please refer to the Neovim v0.9 changelog and review the breaking changes to see if any of the Lua that you have in your user configuration for AstroNvim needs to be updated to follow the new APIs as well.

:::

- **Plugin Manager Change:** With v3 we have moved away from Packer and to the new [lazy.nvim](https://github.com/folke/lazy.nvim). The options for lazy can be configured with the `lazy` user option. We have also removed all abstraction away from the plugin specifications. So the lazy.nvim docs can be referred to for the format of adding new plugins. You can also check the updated [Customizing Plugins Documentation](../Recipes/custom_plugins.md) for defining new plugins as well as overriding core plugins.

  - Lazy also handles overriding options and setup functions automatically so we have removed all of the `plugins.X` user options for overriding the setup tables for the core provided plugins. These can be set up, extended, and configured similar to any other plugin that you are adding.
  - **Note:** The default options for lazy sets `lazy = true` for each plugin. This means plugins should either be configured appropriately for lazy loading or use `lazy = false` if you do not want a plugin to be lazy loaded
  - The `user/plugins/` folder is added to the Lazy plugin specifications to be imported. This allows you to add lists of plugins to any files in the `user/plugins/` folder and they will be used appropriately. This will allow you to organize your plugins in any way you would prefer.

- We have removed Bufferline and are now using Heirline and `astronvim.status` for our own performant and custom tabline.

- `:AstroReload` has been removed. There are a couple reasons for this, it was never very reliable and hard to maintain and lazy.nvim strictly does not support hot reloading neovim configurations.

- The `astronvim.status.component.macro_recording` status component has been removed. Please use the improved `astronvim.status.component.cmd_info` component.

- `lsp.server-settings` has been renamed to `lsp.config`. If you have the `["server-settings"]` table in your `user/init.lua` file, just rename it to `config`. If you have the folder `user/lsp/server-settings`, just rename the folder to `user/lsp/config`.

- `luasnip` options table has been removed. Please see the updated [Custom Snippets Documentation](../Recipes/snippets.md) for the new way to extend the default configuration of LuaSnip to add new loaders.

- `which-key` options table has been removed. Which-key menu titles can now be easily added into the `mappings` table by setting a binding to a table with the `name` property set and it will be passed to `which-key`. For example, you can add the following to the `mappings` table: `["<leader>b"] = { name = "Buffer" }`.

- `nvim-autopairs` options table has been removed. Please see the updated [Customize Autopairs Documentation](../Recipes/autopairs.md) for the new way to extend the default configuration of autopairs and adding more rules.

- `cmp` options table has been removed. Please see the updated [Customize cmp Completion Documentation](../Recipes/cmp.md) for the new way to extend the default configuration of cmp and running more `cmp` setup functions.

- `mason-lspconfig`, `mason-null-ls`, and `mason-nvim-dap` has been removed, please use the new plugin notation for extending these options like adding custom setup handlers. This is described in the [Extending Core Plugin Config Functions Documentation](../Recipes/custom_plugins.md#extending-core-plugin-config-functions).

- `default_theme` has been migrated to a dedicated plugin that can be used outside of AstroNvim as well at [AstroNvim/astrotheme](https://github.com/AstroNvim/astrotheme). This can be customized and configured the same as any other plugin, check the README for details on the `setup` function.

- The bindings in `cmp` to scroll the preview window for a completion item has moved to `<c-u>` and `<c-d>`

- `<leader>p` mappings for package and plugin management have been cleaned up to follow a common format amonst each other. `<leader>ps` is now for checking Plugin Status and `<leader>pS` is for syncing plugins. Mason mappings have been moved to `<leader>pm` and `<leader>pM` for Mason Status and Mason Update respectively.

- The dashboard mapping has been changed from `<leader>d` to `<leader>h` for the "Home Screen"

- The debugging menu has been moved from `<leader>D` to `<leader>d` for quicker and more comfortable usage.

- `H` and `L` have been changed to `[b` and `]b` respectively for changing tabs in the UI. This is for both switching buffers as well as neo-tree sources in the explorer. This can be changed in the your user configuration by adding the following entries to your `mappings.n` table (This uses an internal `nav_buf` function that follows the tab positioning and also allows for using a number to move by multiple tabs at once):

```lua
    L = { function() astronvim.nav_buf(vim.v.count > 0 and vim.v.count or 1) end, desc = "Next buffer" },
    H = { function() astronvim.nav_buf(-(vim.v.count > 0 and vim.v.count or 1)) end, desc = "Previous buffer" },
```

- `header` option has been removed in favor of decreasing abstractions. Check the updated [Dashboard Customizations Documentation](../Recipes/alpha.md)

- `<leader>s` has been unified with the `<leader>f` menu rather than spreading the Telescope mappings out across two menus. Please check the new mappings by pressing `<leader>f` or in the updated [Mappings Documentation](../Basic%20Usage/mappings.md)