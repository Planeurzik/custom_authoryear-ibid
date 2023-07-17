# Fichier .bbx Other
Pour les autres articles, c'est quasiment identique à ceux de maths. La seule différence est donc dans le tri des références et pour respecter l'ordre d'apparition, on modifie le **`sorting=nyt`** du `\usepackage` en **`sorting=none`** :
```TeX
\usepackage[sorting=none, isbn=false, doi=false, style=numeric-comp, giveninits, maxbibnames=8]{biblatex}
```
De plus, on retire ce bout de code :
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