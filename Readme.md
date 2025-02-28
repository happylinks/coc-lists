# coc-lists

Some basic list sources for [coc.nvim](https://github.com/neoclide/coc.nvim/)

Including:

- [x] `files` search files from current cwd.
- [x] `mru` most recent used files.
- [x] `grep` grep text from current cwd.
- [x] `words` search word in current buffer.
- [x] `locationlist` items from vim's location list.
- [x] `quickfix` items from vim's quickfix list.
- [x] `buffers` current buffer list.
- [x] `helptags` helptags of vim.
- [x] `tags` search tag files.
- [x] `filetypes` file types.
- [x] `colors` colors schemes.
- [x] `marks` marks of vim.
- [x] `windows` windows of vim.
- [x] `vimcommands` available vim commands.
- [x] `maps` key mappings.
- [x] `cmdhistory` history of commands.
- [x] `searchhistory` history of search.
- [x] `sessions` session list.

For snippets list, use [coc-snippets](https://github.com/neoclide/coc-snippets).

For git related list, use [coc-git](https://github.com/neoclide/coc-git).

For yank history, use [coc-yank](https://github.com/neoclide/coc-yank).

## Install

In your vim/neovim, run command:

```
:CocInstall coc-lists
```

Checkout `:h coc-lsit` for usage.

## Options

**Tip:** type `?` on normal mode to get detail help of current list.

Available options for coc-lists:

- `session.directory` directory for session files, default to `~/.vim/sessions`
- `session.restartOnSessionLoad` Restart vim with cwd changed on session load, support neovim on iTerm2 only.
- `lists.disabledLists`: List names to disable form load., default: `[]`
- `list.source.files.command`: Command used for search for files, default: `""`
- `list.source.files.args`: Arguments for search command, default: `[]`
- `list.source.files.excludePatterns`: Minimatch patterns that should be excluded., default: `[]`
- `list.source.mru.maxLength`: Max length of mru list., default: `1000`
- `list.source.mru.ignoreGitIgnore`: Ignore git ignored files., default: `false`
- `list.source.mru.excludePatterns`: Minimatch patterns that should be excluded., default: `["**/.git/*","/tmp/*"]`
- `list.source.grep.useLiteral`: Use literal match unless specified regex options, default: true., default: `true`
- `list.source.grep.command`: Command used for grep, default to 'rg'., default: `"rg"` could be `rg` or `ag`.
- `list.source.tags.command`: Command used for generate tags., default: `"ctags -R ."`
- `list.source.grep.args`: Arguments for grep command, always used for grep, default: `[]`
- `list.source.grep.excludePatterns`: Minimatch patterns of files that should be excluded, use .ignore file is recommended., default: `[]`

## Commands

- `mru.validate` remove none exists files from mru list.
- `tags.generate` generate tags of current project (in current cwd).
- `session.save` save current vim state to session file.
- `session.load` load exists session file.

## F.A.Q

Q: How to make grep easier?

A: Create custom command like:

```vim
command! -nargs=+ -complete=custom,s:GrepArgs Rg exe 'CocList grep '.<q-args>

function! s:GrepArgs(...)
  let list = ['-S', '-smartcase', '-i', '-ignorecase', '-w', '-word',
        \ '-e', '-regex', '-u', '-skip-vcs-ignores', '-t', '-extension']
  return join(list, "\n")
endfunction
```

Q: How to grep by motion?

A: Create custom keymappings like:

```vim
vnoremap <leader>g :<C-u>call <SID>GrepFromSelected(visualmode())<CR>
nnoremap <leader>g :<C-u>set operatorfunc=<SID>GrepFromSelected<CR>g@

function! s:GrepFromSelected(type)
  let saved_unnamed_register = @@
  if a:type ==# 'v'
    normal! `<v`>y
  elseif a:type ==# 'char'
    normal! `[v`]y
  else
    return
  endif
  let word = substitute(@@, '\n$', '', 'g')
  let word = escape(word, '| ')
  let @@ = saved_unnamed_register
  execute 'CocList grep '.word
endfunction
```

Q: How to grep current word in current buffer?

A: Create kep-mapping like:

```vim
nnoremap <silent> <space>w  :exe 'CocList -I --normal --input='.expand('<cword>').' words'<CR>
```

## License

MIT
