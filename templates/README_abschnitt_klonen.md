## Klonen (Submodul erforderlich)

Das Regelwerk und Domänenwissen für KI-Assistenten liegt als Git-Submodul unter
`docs/shared_rules_knowledge/` (Quelle: <https://github.com/gdwbdd/shared_rules_knowledge>).
Ohne das Submodul ist der Ordner leer und `CLAUDE.md`/`.github/copilot-instructions.md`
laufen ins Leere.

```powershell
# Neu klonen, Submodul direkt mit
git clone --recurse-submodules <projekt-url>

# Bereits geklont, Ordner docs/shared_rules_knowledge ist leer
git submodule update --init

# Geteilten Stand nachziehen (nur auf Absprache, erzeugt einen Commit)
git submodule update --remote docs/shared_rules_knowledge
```
