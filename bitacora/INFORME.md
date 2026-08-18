# Informe de la investigación — ARCHIVO 2031, equipo Q

| Hallazgo | Dónde estaba | Técnica de Git | Comando exacto | Referencia |
|---|---|---|---|---|
| FRAG-01 | `bitacora/frag-01.txt` en la rama principal de la cinta, borrado en el commit `8fa55a0` "limpieza de archivos temporales" | Localizar el commit que borró el archivo con `--diff-filter=D` y leer el blob del commit padre, donde el archivo todavía existe | `git log --diff-filter=D --name-only --oneline refs/cinta/heads/main` y luego `git show 8fa55a0^:bitacora/frag-01.txt > bitacora/frag-01.txt` | `47e4330` (padre de `8fa55a0`) |
| FRAG-02 | En el mensaje del objeto tag anotado `respaldo/pre-incidente`. El texto no vive dentro de ningún archivo: vive en la referencia misma | Inspeccionar el objeto tag con `cat-file`. `git show` del árbol no lo encuentra porque no es contenido versionado, es el cuerpo del tag | `git cat-file -t refs/cinta/tags/respaldo/pre-incidente` (devuelve `tag`) y luego `git cat-file -p refs/cinta/tags/respaldo/pre-incidente` | `723e638` (objeto tag) |
| Glifo `assets/sello.svg` | En el árbol del commit apuntado por ese mismo tag de respaldo, al que la rama principal nunca volvió | Restaurar una ruta concreta desde una referencia arbitraria sin mover `HEAD` | `git checkout refs/cinta/tags/respaldo/pre-incidente -- assets/sello.svg` | `dfd66ed` (commit del respaldo) |

## Cómo se montó la cinta

```
git bundle verify incidente/equipo-Q.bundle
git bundle list-heads incidente/equipo-Q.bundle
git fetch incidente/equipo-Q.bundle '+refs/*:refs/cinta/*'
```

La cinta trae **3 referencias**, de las cuales **2 no son ramas**: el tag anotado
`refs/tags/respaldo/pre-incidente` y `HEAD`. La única rama es `refs/heads/main`.

Su rama principal tiene **21 commits**, todos firmados por
**S. Rivas `<s.rivas@archivo2031.invalid>`**:

```
git rev-list --count refs/cinta/heads/main
git log --format='%an <%ae>' refs/cinta/heads/main | sort -u
```

La historia de la cinta es **independiente** de la del equipo: no comparten
ningún ancestro común, como confirma que `git merge-base` no devuelva nada.

```
git merge-base main refs/cinta/heads/main
```

## Sello

| Ranura | Palabra |
|---|---|
| FRAG-01 | `QUICKSORT` |
| FRAG-02 | `PALINDROMO` |
| **SELLO** | **`QUICKSORT-PALINDROMO`** |
