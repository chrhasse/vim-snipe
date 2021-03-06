# vim-snipe

> Fast linewise motions and edits

![gif](https://media.giphy.com/media/UXzQDxF7TB1fO/giphy.gif)

Provides

1. Extensions of `f`, `F`, `t`, `T`, `w`, `W`, `b`, `B` and friends to navigate with one keystroke and minimal cognitive load
    * for example, instead of `fA;;;;`, `3T` or `5e`, you can [press a single highlighted key to reach your target](https://github.com/yangmillstheory/vim-snipe#character-motions), avoiding prefix counting
2. Fast ways to fix common typos on the same line, using swaps (`xp`), replacements (`r`), and cuts (`x`)

Read [Motivation](https://github.com/yangmillstheory/vim-snipe#motivation) and [FAQ](https://github.com/yangmillstheory/vim-snipe#faq) for background.

Inspired by:

* [tmux-fingers](https://github.com/Morantron/tmux-fingers)
* [vimium](https://github.com/philc/vimium)
* [vim-easymotion](https://github.com/easymotion/vim-easymotion/)

### Installing

If you use a plugin manager this is as simple as

```vim
Plug 'yangmillstheory/vim-snipe'
```

The above example uses [vim-plug](https://github.com/junegunn/vim-plug); tweak accordingly for your plugin manager.

### Usage and examples

The plugin API is exposed via "named key sequences"; see [this write-up](http://whileimautomaton.net/2008/09/27022735) on why this is a good idea. For more information, do `:h snipe.txt`.

#### Character motions

Use in Visual, Normal, or Operator-pending mode.

Example mappings:

```vim
map <leader><leader>F <Plug>(snipe-F)
map <leader><leader>f <Plug>(snipe-f)
map <leader><leader>T <Plug>(snipe-T)
map <leader><leader>t <Plug>(snipe-t)
```

Example usage: We want to jump to the last "o" in front of the cursor, but there are several other "o"'s in the way.

![f](https://user-images.githubusercontent.com/2729079/33584714-80346e28-d915-11e7-875d-fa01d60389a7.gif)

#### Word motions

Use in Visual, Normal, or Operator-pending mode.

Example mappings:

```vim
map <leader><leader>w <Plug>(snipe-w)
map <leader><leader>W <Plug>(snipe-W)
map <leader><leader>e <Plug>(snipe-e)
map <leader><leader>E <Plug>(snipe-E)
map <leader><leader>b <Plug>(snipe-b)
map <leader><leader>B <Plug>(snipe-B)
map <leader><leader>ge <Plug>(snipe-ge)
map <leader><leader>gE <Plug>(snipe-gE)
```

Example usage: We want to jump to the end of "to".

![ge](https://user-images.githubusercontent.com/2729079/33569952-2e13b444-d8e0-11e7-950b-ad49c8b55eac.gif)

#### Edits

Use in Normal mode.

##### Swap `xp`

Example mappings:

```vim
nmap <leader><leader>] <Plug>(snipe-f-xp)
nmap <leader><leader>[ <Plug>(snipe-f-xp)
```

Usage: Change the typo "smlal" to "small" by swapping a previous instance of "l".

![xp](https://user-images.githubusercontent.com/2729079/33570040-6f51f8c6-d8e0-11e7-935b-627ce9197bef.gif)

##### Cut `x`

Example mappings:

```vim
nmap <leader><leader>x <Plug>(snipe-f-x)
nmap <leader><leader>X <Plug>(snipe-F-x)
```

Example usage: Change the typo "smoall" to "small" by cutting an instance of "o".

![x](https://user-images.githubusercontent.com/2729079/33570110-a36d2e1e-d8e0-11e7-9dc4-4f70f13be3d6.gif)

##### Replace `r`

```vim
nmap <leader><leader>r <Plug>(snipe-f-r)
nmap <leader><leader>R <Plug>(snipe-F-r)
```

Example usage: Change the typo "smbll" to "small" by replacing an instance of "b".

![r](https://user-images.githubusercontent.com/2729079/33586877-69c799a2-d920-11e7-8286-55470dbbdb3c.gif)

### Configuration

By default, the jump tokens are ordered starting with the home row 'asdfghjkl', then 'qwertyuiop', then 'zxcvbnm'. You can provide your own sequence by setting a global variable `g:snipe_jump_tokens`. For Dvorak users, e.g.

```vim
let g:snipe_jump_tokens = 'aoeuidhtns'
```

You can also provide your own highlighting via 

```vim
let g:snipe_highlight_gui_color = '#fabd2f'
let g:snipe_highlight_cterm256_color = '200'
let g:snipe_highlight_cterm_color = 7'
```

These are used to build the highlighting group in [highlight.vim](https://github.com/yangmillstheory/vim-snipe/blob/master/autoload/snipe/highlight.vim) used when highlighting a jump.

### Motivation

**1. Automate fixing typos on the same line.**

I make mistakes often enough to make this worth automating - in fact, I made a few mistakes while writing this line.

**2. No more `;` and `,` repetition when using linewise motions.**

I almost always know exactly where I want to go, but I'm forced to navigate incrementally to that place, or manually count the prefix of the motion (i.e. `5e`). Other times, I want to jump somewhere far in one direction, and it's more efficient to see the possible jump markers, instead of manually exploring (`wwwww`).

**3. There aren't any plugins containing or focused on the same feature set.**

The two plugins with the most overlap in functionality are

* [Stupid-EasyMotion](https://github.com/joequery/Stupid-EasyMotion)
* [vim-easymotion](https://github.com/easymotion/vim-easymotion)

Stupid-Easymotion constrains motions to `line('.')`, but it only provides mappings for `f`, `w`, and `W`. It also hasn't seen any activity in a long time - 4+ years at the time of this writing.

[vim-easymotion has a similarly incomplete API](https://github.com/easymotion/vim-easymotion/issues/354). It's also
[sprawling](https://www.reddit.com/r/vim/comments/1v9qyu/actively_developed_and_maintained_fork_of/ceq7lcf/), and (IMHO)
painful to extend.

However, I'd like to give credit to vim-easymotion, having borrowed some core functionality and logic:

* highlighting initialization
* clever algorithm for building the jump tree


### FAQ

> Why did you constrain to `line('.')`?

There's no need to scan the whole buffer, given `set relativenumber`. Thus, this approach is more performant and IMHO more natural.

> But I've been getting by just fine without this!

Cool, then you shouldn't use it. I use it everyday, so it serves me well, and hopefully I'm not the only one.

> Should I always use the mappings?

No, in some cases (i.e. a single hop to an adjacent word, or when the destination character is unique on the path to the cursor), it's probably faster to use `f`, `F`, `t`, `T`, etc. Pick the right tool for the right job, I'd say.

### Contributing

Pull requests are welcome; no special process is required.
