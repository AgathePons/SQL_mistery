# SQL CLUEDO

Voir toutes les tables :

```sql
SHOW tables;
```

## Dossier de police

```sql
SELECT * FROM rapport_police WHERE date=11122019 AND type='caricature abusive';
```
**2 témoins :**
- Beth Rave
- la personne habitant au dernier numéro de la rue Sadi Carnot

------------------

### Piste Beth Rave

```sql
SELECT * FROM personne WHERE nom='Beth Rave';
```

- id : `416`
- nom : **Beth Rave**
- adresse : 643 rue de Groussay
- promo_id : 17 ➡ **Quasar**
- projet_id : 27 ➡ **O'rdure**
- role : 1

### Piste personne rue Sadi Carnot

``` sql
SELECT * FROM personne WHERE rue='rue Sadi Carnot' ORDER BY numero_rue DESC LIMIT 1;
```

- id : `549`
- nom : **Alex Trémité**
- adresse : 995 rue Sadi Carnot
- promo_id : 9 ➡ **Infinity**
- projet_id : 36 ➡ **O'ctet**
- role : 1

#### Infos sur les témoins

```sql
SELECT  personne.id,  personne.nom,  personne.projet_id,  personne.role_id, promo.nom_promo, promo.couleur 
FROM personne
JOIN promo 
ON personne.promo_id = promo.id 
WHERE personne.id=416 OR personne.id=549;
```

### Témoignages des témoins

```sql
SELECT * FROM temoignage WHERE personne_id=416 OR personne_id= 549;
```
------------------

#### Pour aller plus vite :

```sql
SELECT * FROM temoignage 
JOIN personne ON temoignage.personne_id=personne.id
WHERE personne.nom='Beth Rave';
```

**et**

```sql
SELECT * FROM temoignage JOIN personne ON temoignage.personne_id=personne.id WHERE rue='rue Sadi Carnot' ORDER BY numero_rue DESC LIMIT 1;
```

**Ou** (pour aller encore plus vite...)

```sql
SELECT * FROM temoignage 
JOIN personne ON temoignage.personne_id=personne.id
WHERE personne.nom='Beth Rave'
OR personne.nom=(
  SELECT nom FROM personne 
  WHERE personne.rue='rue Sadi Carnot' 
  ORDER BY personne.numero_rue DESC LIMIT 1
);
```
*une requête dans une requête*

------------------

**Témoignage de Beth :**

> Je n'ai pas vu le coupable, mais il faisait partie de la promo de couleur pourpre.

**Témoignage de Alex**

>J'ai vu le coupable mais je ne connais pas son nom ! En revanche, je sais qu'il a travaillé sur le projet O'asis !

### Enquête d'après témoignages

```sql
SELECT personne.nom, personne.projet_id, promo.nom_promo, promo.couleur, projet.id, projet.nom_projet
FROM personne
JOIN promo ON personne.promo_id = promo.id
JOIN projet ON personne.projet_id = projet.id
WHERE promo.couleur='pourpre' AND projet.nom_projet='O\'asis';
```

------------------

## Cyril Hique coupable ?

```sql
SELECT  * FROM personne WHERE nom='Cyril Hique';
```

- id : `145`
- nom : **Cyril Hique**
- adresse : 454 rue Goya
- promo_id : 20 ➡ **Tardis**
- projet_id : 15 ➡ **O'asis**
- role : 1

### L'enquête continue...

**Témoignage de Cyril:**

```sql
SELECT * FROM temoignage WHERE personne_id=145;
```

> C'est bien moi qui ai fait les caricatures mais à la demande de quelqu'un de l'équipe. C'est un helper qui a entre 40 et 45 ans et fait entre 1m80 et 1m90.

```sql
SELECT * FROM role;
```

Rôle **helper** : `3`

#### Pour aller plus vite

```sql
SELECT * FROM temoignage 
JOIN personne ON temoignage.personne_id=personne.id
WHERE personne.id=(
  SELECT personne.id from personne
  JOIN promo on personne.promo_id=promo.id
  Join projet ON personne.projet_id=projet_id
  WHERE promo.couleur='pourpre'
  AND projet.nom_projet='O\'asis'
);
```

### Enfin le coupable...?

```sql
SELECT * FROM personne 
WHERE role_id=3
AND age BETWEEN 40 AND 45
AND taille BETWEEN 180 AND 190;
```

```sql
SELECT  * FROM personne WHERE nom='Gédéon Groidenmabaignoire';
```

#### Pour aller plus vite

```sql
SELECT * FROM personne
JOIN role ON personne.role_id=role.id
WHERE role.nom_role='helper'
AND (personne.age BETWEEN 40 AND 45)
AND (taille BETWEEN 180 AND 190);
```

- id : `561`
- nom : **Gédéon Groidenmabaignoire**
- adresse : 813 place de la Madeleine
- promo_id : 3 ➡ **Comet**
- projet_id : 21 ➡ **O'zone**
- role : 3