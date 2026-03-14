# miniRT - Ray Tracer Minimaliste

![42 School](https://img.shields.io/badge/42-School-000000?style=flat-square&logo=42)
![C](https://img.shields.io/badge/C-00599C?style=flat-square&logo=c&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)

## Démonstration

Voici une démonstration d'une scène basique réalisée avec miniRT :

<p align="center">
  <video src="https://raw.githubusercontent.com/antoninpicard/miniRT/main/assets/miniRT.mp4" width="600" autoplay loop muted playsinline>
    Votre navigateur ne supporte pas la balise vidéo.
  </video>
</p>

*Cette vidéo montre une scène basique illustrant les capacités de rendu du ray tracer.*

---

**miniRT** est un projet de ray tracing développé dans le cadre du cursus de l'école 42. Il permet de générer des images 3D réalistes à partir de scènes décrites dans des fichiers `.rt`, en utilisant les principes fondamentaux du ray tracing.

## Table des matières

- [Fonctionnalités](#-fonctionnalités)
- [Prérequis](#-prérequis)
- [Installation](#-installation)
- [Utilisation](#-utilisation)
- [Format des fichiers de scène](#-format-des-fichiers-de-scène)
- [Exemples](#-exemples)
- [Contrôles](#-contrôles)
- [Algorithmes implémentés](#-algorithmes-implémentés)
- [Auteurs](#-auteurs)

## Fonctionnalités

- **Ray Tracing** : Calcul précis des intersections rayon-objet
- **Objets 3D supportés** :
  - Sphères
  - Plans
  - Cylindres (avec caps)
- **Modèle d'éclairage Phong** :
  - Lumière ambiante
  - Lumière diffuse
  - Gestion des ombres
- **Caméra configurable** :
  - Position et orientation personnalisables
  - Champ de vision (FOV) ajustable
- **Rendu en temps réel** avec la MiniLibX

## Prérequis

- **Système d'exploitation** : Linux (testé sur Ubuntu/Debian)
- **Compilateur** : `gcc` ou `cc`
- **Bibliothèques** :
  - MiniLibX (incluse dans `mlx_linux/`)
  - X11 et Xext
  - Libft (incluse dans `libft/`)
  - Math library (`-lm`)

### Installation des dépendances (Ubuntu/Debian)

```bash
sudo apt-get update
sudo apt-get install gcc make xorg libxext-dev libbsd-dev
```

## Installation

1. **Cloner le dépôt** :
```bash
git clone https://github.com/antoninpicard/miniRT.git
cd miniRT
```

2. **Compiler le projet** :
```bash
make
```

Le Makefile compile automatiquement :
- La libft
- La MiniLibX
- Le projet miniRT

3. **Nettoyer les fichiers objets** (optionnel) :
```bash
make clean    # Supprime les .o
make fclean   # Supprime les .o et l'exécutable
make re       # Recompile tout
```

## Utilisation

### Lancer une scène

```bash
./miniRT <fichier_scene.rt>
```

### Exemples

```bash
# Scène basique avec une sphère
./miniRT scene/test_basic_sphere.rt

# Scène complexe avec plusieurs objets
./miniRT scene/test_complex_scene.rt

# Scène avec cylindres
./miniRT scene/test_cylinder_orientations.rt

# Scène avec ombres
./miniRT scene/test_shadows.rt
```

## Format des fichiers de scène

Les fichiers `.rt` décrivent la scène en utilisant une syntaxe simple. Chaque ligne représente un élément.

### Éléments obligatoires (une seule fois)

#### **A** - Lumière ambiante
```
A <intensité> <couleur_RGB>
```
- **Intensité** : `0.0` à `1.0`
- **Couleur** : `R,G,B` (0-255)

**Exemple** :
```
A 0.2 255,255,255
```

#### **C** - Caméra
```
C <position_x,y,z> <orientation_x,y,z> <FOV>
```
- **Position** : Coordonnées x,y,z
- **Orientation** : Vecteur normalisé (-1.0 à 1.0)
- **FOV** : Champ de vision en degrés (0-180)

**Exemple** :
```
C 0,5,-15 0,-0.3,1 70
```

#### **L** - Lumière ponctuelle
```
L <position_x,y,z> <intensité> <couleur_RGB>
```
- **Position** : Coordonnées x,y,z
- **Intensité** : `0.0` à `1.0`
- **Couleur** : `R,G,B` (0-255)

**Exemple** :
```
L -10,20,0 0.7 255,255,255
```

### Objets (plusieurs instances possibles)

#### **sp** - Sphère
```
sp <centre_x,y,z> <diamètre> <couleur_RGB>
```

**Exemple** :
```
sp 0,3,20 4 255,0,0
```

#### **pl** - Plan
```
pl <point_x,y,z> <normale_x,y,z> <couleur_RGB>
```
- **Normale** : Vecteur normalisé (-1.0 à 1.0)

**Exemple** :
```
pl 0,0,0 0,1,0 150,150,150
```

#### **cy** - Cylindre
```
cy <centre_x,y,z> <axe_x,y,z> <diamètre> <hauteur> <couleur_RGB>
```
- **Axe** : Vecteur normalisé (-1.0 à 1.0)

**Exemple** :
```
cy 0,1.5,30 0,1,0 2 3 100,255,100
```

### Exemple de scène complète

```
A 0.2 255,255,255
C 0,5,-15 0,-0.3,1 70
L -10,20,0 0.7 255,255,255

pl 0,0,0 0,1,0 150,150,150
pl 0,0,50 0,0,-1 200,200,255

sp -5,3,20 3 255,0,0
sp 5,3,20 3 0,0,255
sp 0,8,25 2 255,255,0

cy 0,1.5,30 0,1,0 2 3 100,255,100
cy -8,1.5,25 1,0,0 1.5 6 255,100,255
```

## Contrôles

| Touche | Action |
|--------|--------|
| `ESC` | Quitter le programme |
| `X` (croix) | Fermer la fenêtre |

## Algorithmes implémentés

### Ray Tracing
- **Génération de rayons** : Calcul des rayons primaires depuis la caméra
- **Intersection rayon-sphère** : Résolution d'équation quadratique
- **Intersection rayon-plan** : Calcul vectoriel
- **Intersection rayon-cylindre** : Équation quadratique + gestion des caps

### Éclairage (Modèle de Phong)
```
Couleur_finale = Ambiante + Diffuse
```

- **Composante ambiante** : `objet_couleur × intensité_ambiante`
- **Composante diffuse** : `objet_couleur × cos(angle) × intensité_lumière`
- **Ombres** : Ray casting vers la source lumineuse

### Calcul des normales
- **Sphère** : `normale = (point_intersection - centre) / rayon`
- **Plan** : `normale = vecteur_normal_du_plan`
- **Cylindre** : Projection sur l'axe perpendiculaire

## Structure du projet

```
miniRT/
├── include/          # Fichiers d'en-tête
│   ├── display.h
│   ├── elements.h
│   ├── objects.h
│   ├── parsing.h
│   ├── utils.h
│   └── vectors.h
├── src/              # Code source
│   ├── core/         # Fonctions principales
│   ├── display/      # Rendu et ray tracing
│   ├── maths/        # Opérations mathématiques
│   └── parsing/      # Lecture des fichiers .rt
├── scene/            # Fichiers de scène exemples
├── libft/            # Bibliothèque libft
├── mlx_linux/        # MiniLibX
├── Makefile          # Compilation
└── README.md         # Ce fichier
```

## Makefile

Le Makefile est optimisé pour éviter les recompilations inutiles :

- **Pas de relink** si aucun fichier n'a changé
- **Recompilation automatique** si libft ou mlx sont modifiées
- **Recompilation** si le Makefile est modifié
- **Gestion des dépendances** avec les headers

```bash
make          # Compile le projet
make clean    # Supprime les fichiers objets
make fclean   # Supprime tout (objets + exécutable)
make re       # Recompile tout
```

## Gestion des erreurs

Le programme gère les erreurs suivantes :
- ✅ Fichier `.rt` invalide ou inexistant
- ✅ Format de scène incorrect
- ✅ Valeurs hors limites
- ✅ Éléments obligatoires manquants
- ✅ Vecteurs non normalisés

En cas d'erreur, un message explicite est affiché sur `stderr`.

## Ressources

- [Ray Tracing in One Weekend](https://raytracing.github.io/)
- [Scratchapixel - Ray Tracing](https://www.scratchapixel.com/)
- [Phong Reflection Model](https://en.wikipedia.org/wiki/Phong_reflection_model)

## Auteurs

- **Antonin Picard** - [@antoninpicard](https://github.com/antoninpicard)
- **Alexandre Lefranc** - [@Alexlechat](https://github.com/alexlechat)

---

*Projet réalisé dans le cadre du cursus de l'école 42*
