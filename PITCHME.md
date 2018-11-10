# Workshop Git

HOGENT IT-lab, 2018-11-13

Bert Van Vreckem <bert.vanvreckem@gmail.com>

---

@color[#FF0000](Heb je een vraag? Onderbreek mij!)

---

## Agenda

- CLI vs GUI
- Interne werking van Git
- Merge vs rebase
- Samenwerken in team
- Typische fouten rechtzetten

---

## CLI vs GUI

---

Gebruik Git vanop de command line

---

Tenminste, totdat je begrijpt wat je doet...

---

### GUI

- Verbergt complexiteit
- Verbergt details
- Beperkt mogelijkheden
- Bemoeilijkt troubleshooting
- **Je begrijpt niet wat je aan het doen bent**

---

### CLI

- Leercurve, juiste commando's leren is moeilijk
- Geen beperkingen op mogelijkheden
- instructies zijn éénduidig en compact
- makkelijker reproduceerbaar
- `git status` toont:
    - huidige toestand
    - volgende stap
    - stap terugzetten

---

### Werk met SSH sleutels

- Zorg dat Git Bash geïnstalleerd is (Windows)
- Maak een SSH sleutelpaar aan (`ssh-keygen`)
- Registreer publieke sleutel (`~/.ssh/id_rsa.pub`) op Github

<https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/>

---

### Git basisconfiguratie

```console
$ git config --global user.name "VOORNAAM NAAM"
$ git config --global user.email "VOORNAAM.NAAM@EXAMPLE.COM"
$ git config --global push.default simple
$ git config --global core.autocrlf input
```

of

```console
git config --global --edit
```

---

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

---

```bash
alias gp='git pull --rebase'
alias pr='git pull --rebase'
alias pt='git push -u origin --tags'
# Git author stats
alias gs='git ls-tree -r -z --name-only HEAD | xargs -0 -n1 git blame --line-porcelain | grep  "^author "|sort|uniq -c|sort -nr'
```

Zie: <https://github.com/bertvv/dotfiles/blob/master/.bash.d/aliases.sh>

### Eenvoudige workflow (solo)

Opstart:

- Repo aanmaken op Github, initialiseer met README
- Clone with SSH: `git clone git@github.com:user/repo.git`

Workflow:

```console
[Bestanden bewerken]
$ git add .
$ git commit -m "Beschrijving aanpassingen"
$ git push
```

### Gebruik na elke stap `git status`

- gewijzigde/toegevoegde bestanden: rood
- bestanden in "staging": groen
- commando voor de volgende stap
- commando om stap ongedaan te maken

---

## Interne werking van Git

- Visual Git cheat sheet: <http://ndpsoftware.com/git-cheatsheet.html#loc=stash;>
- Visualizing Git Concepts with D3: <https://onlywei.github.io/explain-git-with-d3/>


