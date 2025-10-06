import random


def choisir_bornes():
    """
    Permet à l'utilisateur de définir les bornes minimale et maximale du jeu.
    Retourne (borne_min, borne_max)
    """
    print("\n--- Configuration du jeu ---")

    # Demander la borne minimale
    while True:
        try:
            min_input = input("Entrez la borne minimale (par défaut 0) : ")
            borne_min = int(min_input) if min_input else 0
            break
        except ValueError:
            print("❌ Veuillez entrer un nombre entier valide pour la borne minimale.")
        except EOFError:
            # Gère les problèmes d'environnement de saisie
            print("\nFin de l'entrée détectée. Utilisation de la valeur par défaut (0).")
            return 0, 1000

    # Demander la borne maximale
    while True:
        try:
            max_input = input(f"Entrez la borne maximale (par défaut 1000) : ")
            borne_max = int(max_input) if max_input else 1000

            if borne_max <= borne_min:
                print(f"La borne maximale ({borne_max}) doit être supérieure à la borne minimale ({borne_min}).")
                continue
            break
        except ValueError:
            print("❌ Veuillez entrer un nombre entier valide pour la borne maximale.")
        except EOFError:
            # Gère les problèmes d'environnement de saisie
            print("\nFin de l'entrée détectée. Utilisation de la valeur par défaut (1000).")
            # Si le min par défaut est 0, on retourne 0, 1000
            if borne_min == 0:
                return 0, 1000
            else:
                # Si l'utilisateur a déjà entré un min valide, on met le max à min + 100
                return borne_min, borne_min + 100

    return borne_min, borne_max


def jouer_partie(borne_min, borne_max):
    """
    Contient la logique principale d'une seule partie de devinettes.
    """
    nombre_choisi = random.randint(borne_min, borne_max)
    nb_essais = 0
    trouve = False

    # Introduction
    print("\n" + "=" * 40)
    print(f"    Jeu de Devinettes ({borne_min} à {borne_max})")
    print("=" * 40)
    print(f"J'ai choisi un nombre au hasard entre {borne_min} et {borne_max}.")
    print("À vous de le deviner...")

    while not trouve:
        nb_essais += 1

        # Saisie et gestion des erreurs (y compris l'EOFError)
        try:
            essai_input = input("Entrez votre essai : ")

            if not essai_input:
                print("Veuillez entrer un nombre.")
                nb_essais -= 1
                continue

            essai = int(essai_input)

        except EOFError:
            print("\n\n--- Erreur de lecture ---")
            print("Le programme s'est arrêté car il n'a pas pu lire votre entrée.")
            return  # Arrête la partie si EOF est rencontré
        except ValueError:
            print("❌ Entrée non valide. Veuillez entrer un nombre entier.")
            nb_essais -= 1
            continue

        # Logique de comparaison et indices
        if essai < nombre_choisi:
            print(f"Mauvais choix, le nombre est plus grand que {essai}.")
        elif essai > nombre_choisi:
            print(f"Mauvaise réponse, le nombre est plus petit que {essai}.")
        else:
            # Succès
            trouve = True
            print("\n🎉 Bravo! Bonne réponse! 🎉")
            print(f"Vous avez réussi en : {nb_essais} essai(s).")
            return  # Fin de la partie


def main():
    """
    Fonction principale pour gérer le cycle de jeu (rejouer ou quitter).
    """
    jouer_encore = 'o'

    while jouer_encore == 'o':
        # Permet à l'usager de choisir les bornes à chaque nouvelle partie
        borne_min, borne_max = choisir_bornes()

        # Lance la partie
        jouer_partie(borne_min, borne_max)

        # Demander si l'usager veut rejouer
        choix = ''
        while choix not in ('o', 'n'):
            try:
                choix = input("\nVoulez-vous faire une autre partie (o/n)? ").lower().strip()
                if choix not in ('o', 'n'):
                    print("Veuillez répondre par 'o' (oui) ou 'n' (non).")
            except EOFError:
                # Gère l'erreur si l'environnement ne permet pas de rejouer
                print("\nFin de l'entrée détectée. Arrêt du jeu.")
                return

        jouer_encore = choix

    # Message de fin
    print("\nMerci et au revoir… 👋")


if __name__ == "__main__":
    main()
