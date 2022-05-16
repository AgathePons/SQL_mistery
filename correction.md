## Requêtes 

### Trouver le rapport de police 
```SELECT * FROM rapport_police WHERE date="11122019" AND type="caricature abusive"```

### Trouver le témoignage de Beth Rave

```SELECT * FROM temoignage JOIN personne ON temoignage.personne_id=personne.id WHERE personne.nom = "Beth Rave"```

### Trouver le second témoignage
```SELECT * FROM temoignage JOIN personne ON temoignage.personne_id=personne.id WHERE personne.rue = "rue Sadi Carnot" ORDER BY personne.numero_rue DESC LIMIT 1```


### Trouver le coubpable

Deux jointures, l'une pour requeter sur la promo l'autre le projet 

``` SELECT * FROM personne 
JOIN promo ON personne.promo_id = promo.id
JOIN projet ON personne.projet_id = projet.id
WHERE promo.couleur = 'pourpre'
AND projet.nom_projet = "O'asis"
```

### Trouver le témoignage de Cyril
```
SELECT * FROM temoignage
JOIN personne ON temoignage.personne_id=personne.id
WHERE personne.nom = "Cyril Hique"
```

### Trouver le VRAI Coupable 

```
SELECT * FROM personne
JOIN role ON personne.role_id = role.id
WHERE role.nom_role = 'Helper'
AND (personne.age BETWEEN 40 AND 45)
AND (personne.taille BETWEEN 180 AND 190)
```

### Optimisation : trouver les 2 témoignages d'un seul coup !!
SELECT * FROM temoignage
JOIN personne ON temoignage.personne_id=personne.id
WHERE personne.nom ='Beth Rave'
OR personne.id = (
    SELECT id FROM personne
    WHERE personne.rue="rue Sadi Carnot"
    ORDER BY personne.numero_rue DESC
    LIMIT 1
)



``` SELECT id FROM personne 
JOIN promo ON personne.promo_id = promo.id
JOIN projet ON personne.projet_id = projet.id
WHERE promo.couleur = 'pourpre'
AND projet.nom_projet = "O'asis"
```

### Requête imbriqués 
```
SELECT * FROM temoignage
JOIN personne ON temoignage.personne_id=personne.id
WHERE personne.id = (
    SELECT personne.id FROM personne 
    JOIN promo ON personne.promo_id = promo.id
    JOIN projet ON personne.projet_id = projet.id
    WHERE promo.couleur = 'pourpre'
    AND projet.nom_projet = "O'asis"
)
```