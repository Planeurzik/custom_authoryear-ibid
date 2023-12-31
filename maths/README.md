# Fichier .bbx Maths

## A mettre dans le préambule du fichier TeX
Malheureusement, pour le moment, j'ai un bout de code qui ne peut pas être mis dans le bbx mais dans le fichier TeX.

Il y a d'abord l'importation du package biblatex avec les arguments convenants :
```TeX
% Sorting trie selon le nom puis l'année, puis le titre
% Maxbibnames dit qu'au-delà de 8 noms, on garde le premier puis et al.
% Maxcitenames dit que dans les citations au fil du texte, on ne cite pas plus de 2 noms
% Giveninits pour avoir seulement avec les initials
% DOI et ISBN sont masqués
\usepackage[sorting=nyt, isbn=false, doi=false, style=numeric-comp, giveninits, maxbibnames=8]{biblatex}
```
Enfin, pour éviter le mauvais tri dans l'ordre des références, on déclare un tri spécifique :
```TeX
\DeclareSortingScheme{mathssort}{
  \sort{
    \field{author}
    \field{title}
    \field{sorttitle}
    \field{editor}
  }
}

\ExecuteBibliographyOptions{sorting=mathssort}
```

Rappel : pour utiliser la bibliographie en BibLaTeX, on rajoute dans le préambule
```TeX
\addbibresource{le_nom_du_fichier.bib}
```
Et à l'endroit où l'on veut mettre la biblio :
```TeX
\printbibliography
```
## Commande `\truncatedtitle`
La nouvelle commande déclarée au début est `\truncatedtitle` et permet, dans les appels de citation, de garder au plus 3 mots et de rajouter des points de suspensions s'il y en a plus de 3.

```TeX
\newcommand{\truncatedtitle}[1]{%
  \StrCount{#1}{\space}[\titlewords]%
  \ifnum \titlewords<3
    #1%
   \else
      \StrBefore[3]{#1}{ }[\truncated]%
       \IfEq{\titlewords}{2}
        {\truncated}
        {\truncated\dots}
    \fi}
```
## Changement du format dans la bibliogrpahie
En supposant que `type` est le type de référence que l'on souhaite changer (article, book, etc...) :
```TeX
% Si on veut retirer des guillemets qui sont là par défaut
\DeclareFieldFormat[type]{citetitle}{#1}
\DeclareFieldFormat[type]{title}{#1} 

\DeclareBibliographyDriver{type_custom}{%
% Afficher le nom de l'auteur s'il existe
  \ifnameundef{author}{}{\printnames{author}, }
% Afficher le titre
  \printfield{title},
% Inverser l'italique du titre,
  \textit{\printfield{title}}
% Rajouter des guillemets au titre
  \mkbibquote{\printfield{title}}
% S'il existe, afficher les éditeurs ou l'édition
  \iffieldundef{edition}{\ifnameundef{editor}{}{(\printnames{editor}, ed.)}}{
  \printfield{edition}, }
% Afficher les autres champs
  \printfield{number}, \printfield{autre_field}, %
% Afficher seulement l'année
  \printfield{year}
% Afficher la date
  \printdate
%Afficher l'url et la date de visite si elles existent
  \iffieldundef{url}{}{Online at \printfield{url} \usebibmacro{urldate}}
% Fin de référence
\finentry}
```
Enfin, on rajoute après le `\DeclareBibliographyDriver`,
```TeX
\DeclareBibliographyAlias{type}{type_custom}
```


### Format de la urldate
Afin d'avoir la date sous la bonne forme,
```TeX
\DeclareFieldFormat{urldate}{\mkbibmonth{\thefield{urlmonth}} \printfield{urlday}, \thefield{urlyear}}
%Renew de la bib macro afin de changer 'visited on' par 'accessed'
\renewbibmacro{urldate}{(accessed \printurldate)}
```