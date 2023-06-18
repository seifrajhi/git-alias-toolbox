# Intro

Bellow follows a list of aliases, split between [beginner](#git-aliases-beginner) and [advanced](#git-aliases-advanced).

To start using them right away you should place the aliases you like most in a file like `~/.zsh/git`. Now all that remains is to add the following line to your `~/.zshrc` or `~/.bashrc`:

```bash
. ~/.zsh/git # git aliases
```


# Git aliases: beginner

```bash
# Most used git command should be short.
alias s='git status -sb'

alias ga='git add -A'
alias gap='ga -p'

alias gbr='git branch -v'

gc() {
  git diff --cached | grep '\btap[ph]\b' >/dev/null &&
    echo "\e[0;31;29mOops, there's a #tapp or similar in that diff.\e[0m" ||
    git commit -v "$@"
}

alias gch='git cherry-pick'

alias gcm='git commit -v --amend'

alias gco='git checkout'

alias gd='git diff -M'
alias gd.='git diff -M --color-words="."'
alias gdc='git diff --cached -M'
alias gdc.='git diff --cached -M --color-words="."'

alias gf='git fetch'

# Helper function.
git_current_branch() {
  cat "$(git rev-parse --git-dir 2>/dev/null)/HEAD" | sed -e 's/^.*refs\/heads\///'
}

alias glog='git log --date-order --pretty="format:%C(yellow)%h%Cblue%d%Creset %s %C(white) %an, %ar%Creset"'
alias gl='glog --graph'
alias gla='gl --all'

alias gm='git merge --no-ff'
alias gmf='git merge --ff-only'

alias gp='git push'
alias gpthis='gp origin $(git_current_branch)'
alias gpthis!='gp --set-upstream origin $(git_current_branch)'

alias grb='git rebase -p'
alias grba='git rebase --abort'
alias grbc='git rebase --continue'
alias grbi='git rebase -i'

alias gr='git reset'
alias grh='git reset --hard'
alias grsh='git reset --soft HEAD~'

alias grv='git remote -v'

alias gs='git show'
alias gs.='git show --color-words="."'

alias gst='git stash'
alias gstp='git stash pop'

alias gup='git pull'
```

# Git aliases: advanced

```bash
# Use gh instead of git (fast GitHub command line client).
#
# More info: https://github.com/jingweno/gh
#
if type gh >/dev/null; then
  alias git=gh
  if type compdef >/dev/null 2>/dev/null; then
     compdef gh=git
  fi
fi

# Redefine alias. Allow to pass more args to `s`.
s() {
  git status -sb "$@"
  return 0
}

git-new() {
  [ -d "$1" ] || mkdir "$1" &&
  cd "$1" &&
  git init &&
  touch .gitignore &&
  git add .gitignore &&
  git commit -m "Add .gitignore."
}

# Query `glog` with regex query.
gls() {
  query="$1"
  shift
  glog --pickaxe-regex "-S$query" "$@"
}

# Checkout git-smart gem for more infos.
#
# More infos: https://github.com/geelen/git-smart
#
alias gup='git smart-pull'

# Create your own git alias, reusing other git shortcuts.
alias gpstaging='export PREVIOUS_BRANCH=$(git_current_branch) ; gco -B staging ; gpthis -f ; gco $PREVIOUS_BRANCH ; gbr -D staging'

# Create pull-request from command line (uses gh gem).
alias gpr='gpthis; git pull-request'
```


Recommended video: Ben Hoskings on [Advanced Git](http://pluralsight.com/training/courses/TableOfContents/advanced-git).
