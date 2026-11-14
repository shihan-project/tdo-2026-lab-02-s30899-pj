# Raport z laboratorium CI (LaTeX)

Własny dokument `engineer.tex` z opisem eksperymentu CI, składany klasą `sprz` z publicznego szablonu PJATK.

## Pochodzenie szablonu

- Upstream: <https://github.com/pantadeusz/pjatk_latex>
- Autor klasy `sprz.cls`: Tadeusz Puźniakowski (szkic strony tytułowej: Ian Canals-Wąsik), zgodnie z nagłówkiem pliku klasy.
- Przypięta rewizja: `e660b9ee1e637a843c9084774abdc9b3ecdaae4c`
- Licencja: ta rewizja upstream **nie zawiera wyraźnej licencji**. Klasa, przykładowa praca i logo **nie są** częścią tego repozytorium i nie otrzymują tu żadnej licencji.
- Klasa jest pobierana osobno na runnerze CI (checkout do `upstream-latex/`) i kopiowana bez zmian obok dokumentu wyłącznie na czas budowania.

## Własne pliki źródłowe

- `thesis/engineer.tex`: cały tekst dokumentu jest autorski.
- Dokument nie używa przykładowej pracy, logo, ilustracji ani bibliografii z upstream. Nie wywołuje makr `\maketitle`, `\makeprojectcard`, `\makedeclaration` ani `\makethesisattachments`.

## Budowanie

Na Ubuntu 24.04 (tak jak w workflow `.github/workflows/thesis.yml`):

```bash
sudo apt-get update
sudo apt-get install -y latexmk biber texlive-latex-extra \
  texlive-bibtex-extra texlive-fonts-recommended \
  texlive-lang-polish texlive-science

git clone https://github.com/pantadeusz/pjatk_latex.git upstream-latex
git -C upstream-latex checkout e660b9ee1e637a843c9084774abdc9b3ecdaae4c

cp upstream-latex/sprz.cls thesis/sprz.cls
cd thesis
rm -f engineer.pdf
latexmk -norc -pdf -interaction=nonstopmode -halt-on-error -file-line-error -no-shell-escape engineer.tex
test -s engineer.pdf
```

Robocza kopia `thesis/sprz.cls`, katalog `upstream-latex/` i wyniki kompilacji są ignorowane przez `.gitignore` i nie trafiają do historii.
