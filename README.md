## vim-prettier [![Travis CI Build Status](https://travis-ci.org/prettier/vim-prettier.svg?branch=master)](https://travis-ci.org/prettier/vim-prettier) [![Discord](https://img.shields.io/discord/435481502113857536.svg)](https://discord.gg/9bWM9PH)

A vim plugin wrapper for prettier, pre-configured with custom default prettier
settings.

---

By default it will auto format **javascript**, **typescript**, **less**,
**scss**, **css**, **json**, **graphql** and **markdown** files if they
have/support the "@format" pragma annotation in the header of the file.

![vim-prettier](/media/vim-prettier.gif?raw=true 'vim-prettier')

### INSTALL

Install with [vim-plug](https://github.com/junegunn/vim-plug), assumes node and
yarn|npm installed globally.

```vim
" post install (yarn install | npm install) then load plugin only for editing supported files
Plug 'prettier/vim-prettier', {
  \ 'do': 'yarn install',
  \ 'for': ['javascript', 'typescript', 'css', 'less', 'scss', 'json', 'graphql', 'markdown', 'vue'] }
```

or simply enable for all formats by:

```vim
" post install (yarn install | npm install) then load plugin only for editing supported files
Plug 'prettier/vim-prettier', { 'do': 'yarn install' }
```

For those using [vim-pathogen](https://github.com/tpope/vim-pathogen), you can run the following in a terminal:

```
cd ~/.vim/bundle
git clone https://github.com/prettier/vim-prettier
```

If using other vim plugin managers or doing manual setup make sure to have
`prettier` installed globally or go to your vim-prettier directory and either do
`npm install` or `yarn install`

### Prettier Executable resolution

When installed via vim-plug, a default prettier executable is installed inside
vim-prettier.

vim-prettier executable resolution:

1.  Look for user defined prettier cli path from vim configuration file
2.  Traverse parents and search for Prettier installation inside `node_modules`
3.  Look for a global prettier installation
4.  Use locally installed vim-prettier prettier executable

### USAGE

Prettier by default will run on auto save but can also be manually triggered by:

```vim
<Leader>p
```

or

```vim
:Prettier
```

If your are on vim 8+ you can also trigger async formatting by:

```vim
:PrettierAsync
```

You can check what is the `vim-prettier` plugin version by:

```vim
:PrettierVersion
```

You can send commands to the resolved `prettier` cli by:

```
:PrettierCli <q-args>
```

You can check what is the resolved `prettier` cli path by:

```vim
:PrettierCliPath
```

You can check what is the resolved `prettier` cli version by:

```vim
:PrettierCliVersion
```

### Configuration

Change the mapping to run from the default of `<Leader>p`

```vim
nmap <Leader>py <Plug>(Prettier)
```

Disable auto formatting of files that have "@format" tag

```vim
let g:prettier#autoformat = 0
```

Set the prettier CLI executable path

```vim
let g:prettier#exec_cmd_path = "~/path/to/cli/prettier"
```

The command `:Prettier` by default is synchronous but can also be forced async

```vim
let g:prettier#exec_cmd_async = 1
```

By default parsing errors will open the quickfix but can also be disabled

```vim
let g:prettier#quickfix_enabled = 0
```

By default we auto focus on the quickfix when there are errors but can also be disabled

```vim
let g:prettier#quickfix_auto_focus = 0
```

To enable vim-prettier to run in files without requiring the "@format" doc tag.
First disable the default autoformat, then update to your own custom behaviour

Running before saving sync:

```vim
let g:prettier#autoformat = 0
autocmd BufWritePre *.js,*.jsx,*.mjs,*.ts,*.tsx,*.css,*.less,*.scss,*.json,*.graphql,*.md,*.vue Prettier
```

Running before saving async (vim 8+):

```vim
let g:prettier#autoformat = 0
autocmd BufWritePre *.js,*.jsx,*.mjs,*.ts,*.tsx,*.css,*.less,*.scss,*.json,*.graphql,*.md,*.vue PrettierAsync
```

Running before saving, changing text or leaving insert mode:

```vim
" when running at every change you may want to disable quickfix
let g:prettier#quickfix_enabled = 0

let g:prettier#autoformat = 0
autocmd BufWritePre,TextChanged,InsertLeave *.js,*.jsx,*.mjs,*.ts,*.tsx,*.css,*.less,*.scss,*.json,*.graphql,*.md,*.vue PrettierAsync
```

### Overwrite default prettier configuration

**Note:** vim-prettier default settings differ from prettier intentionally.
However they can be configured by:

```vim
" Max line length that prettier will wrap on: a number or 'auto' (use
" textwidth).
" default: 'auto'
let g:prettier#config#print_width = 'auto'

" number of spaces per indentation level: a number or 'auto' (use
" softtabstop)
" default: 'auto'
let g:prettier#config#tab_width = 'auto'

" use tabs instead of spaces: true, false, or auto (use the expandtab setting).
" default: 'auto'
let g:prettier#config#use_tabs = 'auto'

" flow|babylon|typescript|css|less|scss|json|graphql|markdown or empty string
" (let prettier choose).
" default: ''
let g:prettier#config#parser = ''

" cli-override|file-override|prefer-file
" default: 'cli-override'
let g:prettier#config#config_precedence = 'cli-override'

" always|never|preserve
" default: 'preserve'
let g:prettier#config#prose_wrap = 'preserve'
```

### REQUIREMENT(S)

If prettier installation can't be found no code formatting will happen
