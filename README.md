# Signatures LUNIPRIM'S — README

Images des signatures e-mail de LUNIPRIM'S, hébergées sur GitHub et servies par
le **Worker Cloudflare partagé avec GREYPE** (préfixe `/lp`). Même fonctionnement
que le dépôt `adorex5278/signature` : l'URL collée dans Outlook ne change jamais,
on remplace seulement le fichier ici.

## URL utilisées dans les signatures Outlook

```
Bandeau :     https://greype-sig.greype.workers.dev/lp/bandeau
Nominative :  https://greype-sig.greype.workers.dev/lp/n/PRENOM
```

Le Worker va chercher les fichiers dans ce dépôt, en essayant `.gif`, puis `.jpg`,
puis `.png` : on peut changer le format d'une image sans toucher aux signatures.
(Code du Worker : dépôt `adorex5278/signature`, dossier `worker/`.)

## Contenu du dépôt

```
luniprims-signature/
├── banniere_evenement_1108x277.gif   ← fichier SERVI au bandeau, ne pas renommer
│                                        (= copie de la veille OU du bandeau courant)
├── bandeau/
│   ├── veille_1108x1.gif             ← GIF transparent 1 px = bandeau éteint
│   └── evenement_courant.gif         ← (à créer) le bandeau événement en cours
├── nominatives/                      ← prenom.jpg (visuel fixe) ou prenom.gif (animé)
└── tools/                            ← mêmes outils que GREYPE
```

**État actuel du bandeau : ÉTEINT** (aucun événement). Les gabarits contiennent déjà
l'image du bandeau : le jour où on l'allume, **aucune signature n'est à modifier**.

## Allumer / éteindre le bandeau événementiel

Délai : jusqu'à ~10 min (2 caches de 5 min), plus long dans les mails déjà ouverts.

| Je veux… | Avec le script (clone + Python) | Directement sur github.com |
|---|---|---|
| **Nouveau** bandeau (1er événement) | `python tools/bandeau.py nouveau visuel.gif --push` | déposer le GIF (1108 px de large, < 2 Mo) sous le nom exact `banniere_evenement_1108x277.gif` |
| **Éteindre** | `python tools/bandeau.py off --push` | déposer `bandeau/veille_1108x1.gif` renommé `banniere_evenement_1108x277.gif` |
| **Rallumer** | `python tools/bandeau.py on --push` | idem avec `bandeau/evenement_courant.gif` renommé |
| État | `python tools/bandeau.py status` | — |

Le nom `banniere_evenement_1108x277.gif` ne change **jamais** (c'est un nom d'URL).
Pour fabriquer un bandeau 1108 × 277 à partir d'une vidéo/image : `python tools/normalise_banniere.py visuel.mp4`.

## Publier / modifier la signature d'une personne

1. `python tools/normaliser_nominatives.py dossier_recu/ -o nominatives/`
   (source fixe JPEG/PNG → `prenom.jpg` ; animation → `prenom.gif`).
2. **Si le format change** (ex. `.gif` → `.jpg`), supprimer l'ancien fichier : le Worker
   sert `.gif` en premier.
3. Pousser sur `main`. La signature se met à jour chez tout le monde (~10 min).

## Gabarits HTML à donner aux collaborateurs

```
python tools/generer_gabarits.py --worker https://greype-sig.greype.workers.dev --prefixe lp ^
    --titre "Signature LUNIPRIM'S" --alt "LUNIPRIM'S" -o gabarits/
```

Chaque gabarit : « Cordialement, », le bandeau (éteint = 1 px, sans vide), puis la
signature. **Aucune hauteur imposée** : la signature suit l'image servie.

## Points de vigilance
- Dépôt public : n'y mettre que des visuels publicitaires (coordonnées professionnelles).
- Activer le 2FA sur GitHub et Cloudflare ; ne jamais renommer `banniere_evenement_1108x277.gif`.
- Si le Worker ou le compte Cloudflare tombe, **toutes** les signatures (GREYPE et LUNIPRIM'S) cassent.
