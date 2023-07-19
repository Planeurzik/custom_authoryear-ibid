# custom_authoryear-ibid
Compte-rendu de l'Académie des Sciences

Copier les fichiers .bbx dans le dossier de l'installation TeX

> **/!\ Attention**: Bien commenter la ligne `\AtBeginDocument{\RequirePackage{cite}}` dans le cedram-CR*.sty
## Changer le titre par défaut "References" dans la bibliographie

```TeX
\usepackage[english]{babel}

\addto\captionsenglish{\renewcommand{\refname}{References / Références}}
```
**Testé avec cedram.cls sur un article des CRMATHS**

/!\ A mettre dans le préambule du fichier TeX 
