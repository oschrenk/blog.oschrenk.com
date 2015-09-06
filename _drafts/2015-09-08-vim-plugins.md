
## Automatic plug installation


" Automatic plug installation:
if empty(glob('~/.vim/autoload/plug.vim'))
		silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
		\ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
		autocmd VimEnter * PlugInstall
endif


## Text objects

" Text objects
Plug 'kana/vim-textobj-user'              " creste your own text-objects
Plug 'gilligan/textobj-gitgutter'         " ih for change-hunk text object
Plug 'kana/vim-textobj-indent'            " ai/ii/aI/iI for block of indented lines
Plug 'kana/vim-textobj-line'              " al/il for current line
Plug 'sgur/vim-textobj-parameter'         " i,/a, for function parameter
Plug 'Julian/vim-textobj-variable-segment' " iv/av change variable segments
Plug 'saihoooooooo/vim-textobj-space'     " aS/iS for space regions
Plug 'reedes/vim-textobj-sentence',  { 'for': 'markdown' } " as/is for a sentence of prose
Plug 'mattn/vim-textobj-url'              " au/iu for url
Plug 'kana/vim-textobj-entire'            " ae/ie for entire buffer
Plug 'rhysd/vim-textobj-anyblock'         " ib/ab for Quotes, Parenthesis and braces


## Gigutter

resetting and staging hunks
