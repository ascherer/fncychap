# Fancy Chapter Headings

Recently I tried to re-compile an old LaTeX document of mine and failed,
because the KOMA `scrbook` document class produced [an unexpected
error](https://komascript.de/node/2071).

Closer inspection showed that the `fncychap` macro package was guilty of
committing the horrendous crime of using oldstyle `\rm`, `\sf`, and `\tt`
font selectors in several of its _style_ definitions.

After fixing the macros [in the obvious
way](https://github.com/ascherer/fncychap/commit/5c5d5ab81a57177430a6f76cf6ebef1f17d0501b),
I also applied some spit and polish to the package documentation, which I used
for detailed tests.

The creator of [the `fncychap` package](https://ctan.org/pkg/fncychap) has been
notified of this project, but I have not received feedback from him.

If _you_ find anything noteworthy in this code, feel free to open an issue a/o
pull request. Please, do _not_ bother Ulf Lindgren with my work (yet). It's for
him to decide if he wants to use this stuff in a future release.

## Resolution

In the end I purged `fncychap.sty` from my application completely and replaced
it with this simple set of KOMA-related macros:
```
\setkomafont{chapter}{\normalfont\Huge}

\renewcommand{\hrulefill}[1]{\leavevmode\leaders\hrule\@height#1\hfill\kern\z@}
\renewcommand\chapterlinesformat[3]{%
  \parbox[b]{\textwidth}{\hrulefill{2pt}#2}\par%
  \centerline{\MakeUppercase{#3}}\par}
\RedeclareSectionCommand[afterskip=6\baselineskip,
  beforeskip=3\baselineskip]{chapter}
```
This recreates the layout in my two texts quite faithfully.

