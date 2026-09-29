# Simulateur d'investissement locatif — colocation & studios étudiants

Simulateur financier en une seule page web pour évaluer un projet d'investissement locatif meublé destiné aux étudiants (colocation ou immeuble découpé en *n* studios) : achat, crédit, loyers, charges, fiscalité LMNP, inflation et rentabilité.

Toutes les valeurs se recalculent instantanément à chaque modification, sans serveur ni installation.

## Fonctionnalités

- **Saisie des hypothèses** regroupées par thème : achat, financement, location, charges, inflation, fiscalité personnelle.
- **6 indicateurs clés** (volontairement limités à l'essentiel) :
  | Indicateur | Ce qu'il mesure |
  |---|---|
  | Coût total du projet | Prix + notaire + travaux + mobilier + frais bancaires |
  | Mensualité du prêt | Remboursement mensuel, assurance comprise, et coût total du crédit |
  | Cash-flow net / mois | Ce qu'il reste (ou manque) chaque mois après crédit, charges et impôts |
  | Rentabilité nette | (Loyers − charges) / coût total, avec la rentabilité brute en rappel |
  | Taux d'endettement | Calcul bancaire HCSF, comparé au plafond de 35 % |
  | Enrichissement net | Valeur du bien − dette − apport + cash-flows cumulés, à l'horizon choisi |
- **3 graphiques dynamiques** : décomposition mensuelle du loyer (cascade), cash-flow annuel et cumulé, construction du patrimoine.
- **Tableau année par année** (loyers, charges, crédit, intérêts, impôts, cash-flow, capital restant dû…).
- **Inflation** : charges indexées sur l'inflation, loyers sur l'IRL, mensualité fixe ; bascule d'affichage en **euros constants**.
- **Comparaison fiscale** micro-BIC / LMNP réel (amortissements, report des déficits et des amortissements).
- **Info-bulles** ⓘ expliquant chaque notion technique au survol.
- **Export PDF** (hypothèses, indicateurs, graphiques, tableau) en un clic.
- **Sauvegarde automatique** des saisies dans le navigateur (`localStorage`), bouton de remise à zéro.
- Thème clair / sombre automatique, responsive (utilisable sur mobile).

## Utilisation

### En local
Téléchargez `simulateur_colocation.html` et ouvrez-le dans un navigateur (Chrome, Firefox, Edge, Safari).
Une connexion internet est nécessaire pour charger les librairies de graphiques et d'export PDF.

### En ligne avec GitHub Pages
1. Renommez le fichier en `index.html` (ou gardez son nom et utilisez l'URL complète).
2. Dans le dépôt : **Settings → Pages → Source : Deploy from a branch**, branche `main`, dossier `/ (root)`.
3. Le simulateur est accessible à l'adresse `https://<utilisateur>.github.io/<depot>/`.

## Modèle de calcul

| Élément | Hypothèse retenue |
|---|---|
| Crédit | Prêt amortissable à mensualités constantes, assurance calculée sur le capital initial |
| Loyers | Loyer × nombre de logements × mois loués par an (intègre la vacance estivale), indexés sur l'IRL |
| Charges | Postes fixes indexés sur l'inflation + postes proportionnels aux loyers (entretien, gestion) |
| Micro-BIC | Abattement forfaitaire de 50 % sur les loyers |
| LMNP réel | Déduction des charges, intérêts, assurance et frais bancaires ; amortissements : bâti (hors terrain) + notaire sur 30 ans, travaux sur 15 ans, mobilier sur 7 ans ; les amortissements ne créent pas de déficit et sont reportés |
| Impôt | Base imposable × (TMI + prélèvements sociaux) |
| Endettement | (Mensualités + autres crédits) / (revenus + 70 % des loyers) |
| Euros constants | Montants de l'année *n* divisés par (1 + inflation)ⁿ |

### Limites
Non modélisés : fiscalité et frais de revente (plus-value, réintégration des amortissements LMNP depuis 2025), passage au statut LMP, différé de prêt, travaux imprévus. Les taux fiscaux sont modifiables et doivent être vérifiés selon la législation en vigueur.

> ⚠️ Simulation indicative : elle ne remplace pas l'avis d'un banquier, d'un conseiller en gestion de patrimoine ou d'un expert-comptable.

## Technique

- Un seul fichier HTML autonome : HTML, CSS et JavaScript natif, sans build.
- [Chart.js](https://www.chartjs.org/) 4.4 pour les graphiques.
- [jsPDF](https://github.com/parallax/jsPDF) 2.5 et [jsPDF-AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable) 3.8 pour l'export PDF.
- Librairies chargées depuis le CDN cdnjs.

### Personnaliser
- **Valeurs par défaut** : tableau `GROUPS` en tête du script (`def` de chaque champ).
- **Textes des info-bulles** : objet `TIPS`.
- **Calculs** : fonction `simulate(p)`, qui renvoie les indicateurs et la projection annuelle.
