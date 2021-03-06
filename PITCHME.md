# Workshop Git

HOGENT IT-lab, 2019

+++

@color[#FF0000](Heb je een vraag? Onderbreek ons!)

+++

*There is more than one way to do it*

Wat volgt zijn aanbevelingen

+++

## Agenda

- CLI vs GUI
- Hoe werkt Git?
- Merge vs rebase
- Samenwerken in team
- Typische fouten rechtzetten
- Ask Me Anything

+++

### Ik (Bert) gebruik Git voor (bijna) alles

- [Scripts](https://github.com/bertvv/scripts), programmacode
- Handleidingen, syllabi (LaTeX FTW!)
- Presentaties ([Reveal.js](https://revealjs.com/#/)/[Gitpitch](https://gitpitch.com/))
- Documentatie/nota's ([Markdown](https://guides.github.com/features/mastering-markdown/))
- Verspreiden/opvolgen studentenwerk
- Samenwerken met collega's

+++

```console
$ find ~ -type d -name '.git' | wc -l
860
```

+++

### Regelmatig repo's om zeep geholpen

+++

### Alles kan hersteld worden

(of toch zo goed als alles...)

---

## CLI vs GUI

+++

Gebruik Git vanop de command line

+++

Tenminste, totdat je begrijpt wat je doet...

+++

### GUI

- Verbergt complexiteit
- Verbergt details
- Beperkt mogelijkheden
- Bemoeilijkt troubleshooting
- **Je begrijpt niet wat je aan het doen bent**

+++

### CLI

- Leercurve, juiste commando's leren gaat niet vanzelf
- Geen beperkingen op mogelijkheden
- Instructies zijn éénduidig en compact
- Makkelijker reproduceerbaar

+++?image=https://www.gitkraken.com/images/og-image.jpg&size=contain

+++

### `git status` FTW!

- huidige toestand
- volgende stap
- stap terugzetten

+++

### Werk met SSH sleutels

- Zorg dat Git Bash geïnstalleerd is (Windows)
- Maak een SSH sleutelpaar aan (`ssh-keygen`)
- Registreer publieke sleutel (`~/.ssh/id_rsa.pub`) op Github

<https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/>

+++

### Git basisconfiguratie

```console
$ git config --global user.name "VOORNAAM NAAM"
$ git config --global user.email "VOORNAAM.NAAM@EXAMPLE.COM"
$ git config --global push.default simple
$ git config --global pull.rebase true
$ git config --global core.autocrlf input
```

+++

of

```console
$ git config --global --edit
```

Voorbeeld: <https://github.com/bertvv/dotfiles/blob/master/.gitconfig>

+++

### Eenvoudige workflow (solo)

Opstart:

- Repo aanmaken op Github, initialiseer met README
- Clone with SSH: `git clone git@github.com:user/repo.git`

+++

### Eenvoudige workflow (solo)

```console
[Bestanden bewerken]
$ git add .
$ git commit -m "Beschrijving aanpassingen"
$ git push
```

![Eenvoudige workflow voor één persoon](assets/workflow-solo.png)

+++

### Gebruik na elke stap `git status`

- gewijzigde/toegevoegde bestanden: rood
- bestanden in "staging": groen
- commando voor de volgende stap
- commando om stap ongedaan te maken

+++

### Aliases

Voeg toe aan `~/.bashrc`:

```bash
alias s='git status'
alias a='git add'
alias c='git commit -m'
alias d='git diff'
alias g='git'
alias h='git log --pretty="format:%C(yellow)%h %C(blue)%ad %C(reset)%s%C(red)%d %C(green)%an%C(reset), %C(cyan)%ar" --date=short --graph --all'
alias p='git push && git push --tags'
```

+++

```bash
alias gp='git pull --rebase'
alias pr='git pull --rebase'
alias pt='git push -u origin --tags'
# Git author stats
alias gs='git ls-tree -r -z --name-only HEAD | xargs -0 -n1 git blame --line-porcelain | grep  "^author "|sort|uniq -c|sort -nr'
```

Zie: <https://github.com/bertvv/dotfiles/blob/master/.bash.d/aliases.sh>

---

## Hoe werkt Git?

- Visual Git cheat sheet: <http://ndpsoftware.com/git-cheatsheet.html#loc=stash;>
- Visualizing Git Concepts with D3: <https://onlywei.github.io/explain-git-with-d3/>

---

## Merge vs Rebase

<https://onlywei.github.io/explain-git-with-d3/>

+++

### Rebase workflow

```console
[Bestanden bewerken]
$ git add .
$ git commit -m "Beschrijving wijzigingen"
$ git pull --rebase
[Eventuele conflicten oplossen]
$ git push
```

+++

### Conflicten oplossen

```console
$ git push
To github.com:bertvv/git-demo.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:bertvv/git-demo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

+++

### Stap 1. Rebase

```console
$ git pull --rebase
```

+++

### Stap 2. Status!

```console
$ git status
rebase in progress; onto e5bd2df
You are currently rebasing branch 'master' on 'e5bd2df'.
  (fix conflicts and then run "git rebase --continue")
  (use "git rebase --skip" to skip this patch)
  (use "git rebase --abort" to check out the original branch)

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

	both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

+++

### Stap 3. Bewerk bestand(en)

- Zoek naar markeringen
- Sommige editors ondersteunen dit!

```
If you have questions, please
<<<<<<< HEAD
open an issue
=======
ask your question in IRC.
>>>>>>> branch-a
```

+++

### Stap 4. Mark resolution

```console
$ git add .
$ git status
rebase in progress; onto e5bd2df
You are currently rebasing branch 'master' on 'e5bd2df'.
  (all conflicts fixed: run "git rebase --continue")

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
$ git rebase --continue
```

+++

### Stap 5. Push!

```console
$ git status
$ git push
$ git status
```

---

## Samenwerken in team

+++

### Trunk based development

- Geen branches op centrale repo!
- Toegepast bij Continuous Integration/Delivery/Deployment
- Feature flags

+++

### Pull requests

- Voor medewerkers die geen schrijftoegang hebben
- Complexer op te zetten
- Altijd committen op topic branch
- Synchroniseren met "upstream"

+++

### GitFlow

- Software met discrete releases
- Master is altijd "proper" -> kan zo in productie
- Complexer!
- Mogelijke bottlenecks

+++?image=assets/gitflow.png&size=contain

+++

### Main branches

* master: product-worthy
* develop: laatste aanpassingen voor de nieuwe versie

+++

### Supporting branches

* feature
* branches van develop
* heeft nieuwe features van de software

+++

```
git checkout -b myfeature develop
git checkout develop
Switched to 'develop' branch
git merge --no-ff myfeature
Updating ea1b82a..05e9557
(Summary of changes)
git branch -d myfeature
Deleted branch myfeature (was 05e9557).
git push origin developer
```

+++

### Wat met die -no-ff?

- Fast forward treedt op wanneer de branch waarom je merged geen aanpassing heeft en er dus een lineair verband bestaat
- Fast forward verplaatst dan gewoon de head, zonder merge commit te maken
- `no-ff` verplicht om een merge commit te maken

+++

![no-ff](https://nvie.com/img/merge-without-ff@2x.png)

+++

### Praktisch 1

Nieuwe feature?

- Nieuwe branch van dev
- Kies goede naam voor branch (bv. issue naam Jira, prefix met taak Bv. layout/proj-127)

+++

### Praktisch 2

- Maak aanpassingen
- Commit
- Atomaire commits

Pro-tip: gebruik .gitmessage (zie tips)

+++

### Praktisch 3

- Open een pull request
- @mention gebruiken om collega's te refereren
- Bespreek de code voordat je merged/rebased

+++

### Praktisch 4

- Bespreek de code
    - Code style oké?
    - Unit testen geschreven?
    - Of geef positieve feedback
- Gebruik markdown om tekst te stijlen

+++

### Praktisch 5 (optioneel)

Gebruik je een CI/CD pipeline: laat het zijn werk doen voordat je merged.
- CI/CD pipeline is the single source of truth

+++

### Praktisch 6

- Merge of Rebase de branch
- Vergeet feat-branch niet te verwijderen

---

## Tips, aanbevelingen

+++

### KISS <sup> 1 </sup>

Maak workflow niet ingewikkelder dan **strikt** noodzakelijk

+++

### Schrijf goede commit-boodschappen <sup> 2 </sup>

- voor je teamleden
- voor je toekomstige zelf

<https://chris.beams.io/posts/git-commit/>

+++

### How to write a `git commit` Message

1. Separate subject from body with a blank line
2. Limit the subject line to 50 characters
3. Start with a capital letter for the subject
4. Do not end the subject line with a dot
5. Use the imperative in the subject line
6. Max body characters: 72
7. Use body to explain what and why and how (if necessary)

+++

Commit messages met een body

[https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration](Customize Git configuration.)

+++

### Tips and tricks <sup> 3 </sup>

Gebruik jouw favoriete editor (eg TextMate):

```console
git config --global core.editor "mate -w"
```

Andere: [example](https://help.github.com/articles/associating-text-editors-with-git/)

+++

### Tips and tricks <sup> 4 </sup>

Gebruik git template zodat iedereen dezelfde structuur hanteert.

```bash
git config --global commit.template ~/.gitmessage.txt
git commit
```

+++

### Atomaire commits <sup> 5 </sup>

- Elke commit heeft precies één reden/doel
- Voeg individuele bestanden toe aan staging

+++

### Git diff <sup> 6 </sup>

Bekijk lokale wijzigingen voordat je add/commit

+++

### Nooit publieke historiek overschrijven <sup> 7 </sup>

Doe dit niet:

```console
$ git reset --hard
$ git push --force
```

Gebruik in plaats daarvan `git revert`

+++

### Regelmatig pushen <sup> 8 </sup>

Hoe langer je wacht, hoe meer merge-conflicten!

+++

### Read The Fine Error/Info Message!

---

## Typische fouten rechtzetten

<https://ohshitgit.com/>

## Oefeningen

- <https://gitexercises.fracz.com/>

- <https://github.com/rferri-gr8/git-flow-exercise>

---

## Q&A

Ask us anything!

(about Git...)

---

## Bedankt!

Meer info:

* [`giteveryday`](http://git-scm.com/docs/giteveryday) (basiscommando's)
* [Visual Git Cheat Sheet](http://ndpsoftware.com/git-cheatsheet.html)
* [Visualizing Git Concepts with D3](https://onlywei.github.io/explain-git-with-d3/)
* [Git Reference](http://git-scm.com/docs) ("man pages")
* <https://github.com/bertvv>
