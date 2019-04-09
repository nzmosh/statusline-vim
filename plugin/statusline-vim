scriptencoding utf-8

"" Status line
"" via https://github.com/noahfrederick/dots/blob/master/vim/init.vim#L207-L275

let &statusline  = ' %2*%{exists("*ObsessionStatus")?ObsessionStatus(StatuslineProject(), "[".StatuslineProject()."]"):""}'
let &statusline .= '%#StatusLineNC#%{exists("*ObsessionStatus")?ObsessionStatus("", "", StatuslineProject()):StatuslineProject()}'
let &statusline .= "%*%{empty(expand('%'))?'':expand('%')}"
let &statusline .= "%#StatusLineNC#%{StatuslineGit()}%* "
let &statusline .= '%1*%{&modified && !&readonly?"\u25cf ":""}%*'
let &statusline .= '%1*%{&modified && &readonly?"\u25cb ":""}%*'
let &statusline .= '%4*%{&modifiable?"":"\u25cb "}%*'
let &statusline .= '%3*%{&readonly && &modifiable && !&modified?"\u25cb ":""}%*'
let &statusline .= "%="
let &statusline .= "%#StatusLineNC#%{StatuslineIndent()}%* "
let &statusline .= '%#StatuslineNC#%{(strlen(&fileencoding) && &fileencoding !=# &encoding)?&fileencoding." ":""}'
let &statusline .= '%{&fileformat!="unix"?" ".&fileformat." ":""}%*'
let &statusline .= '%{strlen(&filetype)?&filetype." ":""}'
let &statusline .= '%#Error#%{exists("*neomake#statusline#QflistStatus")?neomake#statusline#QflistStatus().neomake#statusline#LoclistStatus():""}'
let &statusline .= "%{StatuslineTrailingWhitespace()}%*"

function! StatuslineGit()
  if !exists('*fugitive#head')
    return ''
  endif
  let l:out = fugitive#head(8)
  if !empty(l:out) && !empty(expand('%'))
    let l:out = '  @ ' . l:out
  elseif !empty(l:out)
    let l:out = ' ' . l:out
  endif
  return l:out
endfunction

function! StatuslineIndent()
  if !&modifiable || &buftype ==# 'terminal'
    return ''
  endif

  if &expandtab == 0 && &tabstop == 8
    " Sleuth.vim has detected mixed indentation
    return "!!"
  endif

  let l:symbol = &expandtab ? "\u2022" : "\u21e5 "
  let l:amount = exists('*shiftwidth') ? shiftwidth() : &shiftwidth
  return &expandtab ? repeat(l:symbol, l:amount) : l:symbol
endfunction

function! StatuslineProject()
  if empty(expand('%'))
    return ''
  endif
  let dir = getcwd() == $HOME ? '~' : fnamemodify(getcwd(), ':t')
  return dir . (expand('%')[0] !~# '\v[\[/]' && stridx(expand('%:p'), getcwd()) == 0 ? '/' : ' ')
endfunction

function! StatuslineTrailingWhitespace()
  if !exists("b:statusline_trailing_whitespace")
    if !&modifiable || search('\s\+$', 'nw') == 0
      let b:statusline_trailing_whitespace = ""
    else
      let b:statusline_trailing_whitespace = "  \u2334 "
    endif
  endif

  return b:statusline_trailing_whitespace
endfunction

augroup init_statusline
  autocmd!
  autocmd CursorHold,BufWritePost * unlet! b:statusline_trailing_whitespace
augroup END

