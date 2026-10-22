L'ensemble du jeu est dans le fichier ?.html le graphique du plateau est donné dans le fichier plateau ?.png

Les règles classiques du jeu de l'oie ont été complétées par l'ajout de nouvelles actions et un système de cartes actions mobilisable par le joueur au moment de son tour de jeu.

De nouvelles actions peuvent être ajoutées en utilisant la structure suivante.

--Spécification technique pour faire développer de nouvelles actions (par un tiers ou une IA)

Pour permettre à une autre personne ou à une IA de créer de nouvelles actions compatibles avec votre jeu (pour les cases du plateau, les cartes Chance et les cartes Action en main), voici le contrat exact utilisé par le moteur :

    Langage : JavaScript (ES6 standard)
    Enregistrement : registerGooseAction(actionObject) (vous pouvez aussi coller directement l'objet dans le bouton « API Actions (IA / Dev) » présent en haut du jeu pour l'injecter à chaud).

Format de l'objet Action :

    id (String) : Identifiant unique en snake_case (ex. "swap_last_player").
    name (String) : Nom court de l'action affiché sur la case et sur la carte (ex. "Coup de pouce").
    description (String) : Explication de la règle affichée au survol de la case ou lors du tirage de la carte.
    icon (String) : Emoji ou caractère visuel représentant l'action (ex. "🚀").
    theme (String) : Couleur de la case parmi 'green', 'blue', 'yellow', 'purple', 'red'.
    defaultWeight (Number) : Poids relatif par défaut (de 0 à 100) déterminant sa fréquence d'apparition.
    canBeOnBoard, canBeChanceCard, canBePlayerCard (Boolean) : Définit si l'action peut apparaître sur une case du plateau, en Carte Chance et/ou en Carte Action distribuée aux joueurs.
    execute(gameState, playerId, context) (Function) :
        Paramètres d'entrée :
            gameState (Object) : État complet de la partie contenant players (tableau de { id, name, position (1 à 63), skipTurns, hand }), board (les 63 cases { number, actionId }), turnIndex, round, et maxCell (63).
            playerId (String) : Identifiant du joueur concerné par l'action.
            context (Object) : { source: 'CELL' | 'CHANCE_CARD' | 'PLAYER_CARD', cellNumber: Number, lastRoll: Number }.
        Valeur de sortie (Return) : Un objet { gameState, message: String, extraTurn: Boolean, triggerChanceCard: Boolean }
