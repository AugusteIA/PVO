# Liquidation PVO

Application locale de préparation mensuelle de la liquidation PVO. Elle analyse un fichier DHE et une table ZVO, isole les agents concernés et produit une copie enrichie du fichier ZVO avec un onglet par agent, une synthèse et des contrôles d’anomalies.

Les fichiers sont traités uniquement dans le navigateur. Aucune donnée n’est envoyée vers un serveur.

## Utilisation sous Windows

1. Téléchargez le dossier complet depuis GitHub avec **Code** puis **Download ZIP**.
2. Décompressez l’archive, par exemple dans `Documents\\Liquidation-PVO`.
3. Vérifiez que le dossier `vendor` est présent à côté du fichier `index.html`. Ne déplacez pas le fichier HTML seul.
4. Ouvrez `index.html` avec Microsoft Edge ou Google Chrome. Un double-clic suffit généralement.
5. Dans la page, importez votre fichier DHE et votre fichier ZVO.
6. Sélectionnez la date de traitement et, si nécessaire, la feuille qui contient la table ZVO `tempMain` ainsi que la colonne de mois.
7. Cliquez sur **Analyser les fichiers**, contrôlez la synthèse et les éventuelles lignes `ERREUR`.
8. Cliquez sur **Télécharger le fichier ZVO complété**. Le classeur téléchargé est une copie. Les fichiers importés ne sont pas modifiés.

## Test rapide avec les fichiers fournis

Les fichiers fictifs se trouvent dans `outputs/pvo_dummy_2026/` :

- `DHE – Suivi de campagne Départ temporaire ou définitif_2026.xlsx`
- `ZVO JANVIER A AOUT 2026.xlsx`

Pour reproduire le test prévu, entrez le **4 septembre 2026** comme date de traitement. L’application doit retenir cinq matricules uniques. Quatre onglets doivent signaler une erreur volontaire et le cas valide `MARTIN Bernard` doit calculer une PVO de **2 688,00 €**.

## Règles appliquées

- DHE : colonnes B, C, D, F, Q, R, X et Y contrôlées.
- Ligne éligible : `Montant` vide et mois de paiement compris entre le 1er janvier N et le dernier jour du mois N-1.
- Une valeur `0` dans `Montant` est exclue.
- ZVO : rapprochement sur le matricule en colonne C de la table `tempMain`.
- Calcul : somme des montants salariaux en colonne U × taux d’atteinte calculé × taux PVO cible.
- Les anomalies sont signalées explicitement dans l’onglet concerné et dans la synthèse.

## Structure du dossier

```text
index.html                         Application à ouvrir localement
vendor/xlsx.full.min.js            Bibliothèque Excel incluse, sans connexion Internet
outputs/pvo_dummy_2026/            Fichiers fictifs de test
```

## Précautions

Cette application prépare des calculs pour revue humaine. Elle ne doit pas déclencher un paiement ni remplacer les contrôles métier et de paie.

## Composant tiers

L’application embarque [SheetJS Community Edition](https://sheetjs.com/), utilisé pour lire et générer des classeurs Excel dans le navigateur.
