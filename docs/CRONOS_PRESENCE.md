# V2 — The Living Singularity (23 novembre 2026)

## Direction artistique
- Fond noir spatial (#01040a) et lattice hexagonale filaire subtile.
- Noyau à deux lobes asymétriques en obsidienne, avec pont organique lumineux.
- Raymarching WebGL2 en GLSL 300 ES : SDF avec union lissée, micro-plis,
  crêtes ferrofluides, reflets chrome, Fresnel, éclairages cyan/violet/états.
- Orbites elliptiques, guide orbital, particules, treillis de perspective et HUD discret.
- Rendu 2D Canvas de secours **complet** quand WebGL2 est indisponible.
- Résolution interne plafonnée, qualité auto/haute/basse, gestion mouvement réduit.
- Tout tient dans un seul HTML autonome, sans CDN, compte ou coût.

Le nouveau rendu remplace la V1 dans `public/cronos-presence.html`; l'ancien
parcours narratif React reste intact et le lien depuis `src/App.tsx` fonctionne.

## Tests réalisés en environnement de développement
- Chromium headless, écran 1440×900 : page rendue, interactions des boutons,
  aucun exception JavaScript, capture de vérification.
- Chromium headless, écran 390×844 : page responsive rendue, aucun exception JavaScript.
- Le navigateur de test ne propose pas WebGL2 : **seul le fallback Canvas 2D a été
  vérifié visuellement**. La compilation et les performances du shader WebGL2
  restent à confirmer sur la GTX 1060 et les téléphones.
- Aucune passerelle CRONOS/Hermes n'a été connectée : les états du panneau sont
  des simulations. Le contrat `cronos:presence` a été conservé.

## Prochaines tâches
1. Tester WebGL2 en navigateur réel, inspecter FPS et consommation mémoire.
2. Raccorder un adaptateur événements read-only CRONOS Core/Hermes, avec TTL
   d'activité et indicateur clair des sources simulées/réelles.
3. Brancher voix réelle (analyse d'amplitude et états TTS/ASR) sans
   capturer le micro sans consentement.
4. Ajouter mode écran seul/overlay OBS et synchronisation LAN avec authentification.
5. Travailler les contraintes de performance smartphone et le respect du
   mouvement réduit.

---

# CRONOS — Présence ferrofluide permanente (prototype V1)

## Objectif
Une incarnation continue de CRONOS, destinée à rester visible comme une interface JARVIS : le noyau reste vivant même sans conversation. La cinématique d'éveil React est conservée.

## Références
- Morphologie de la source : src/data/stages.ts (verre ovoïde, obsidienne, deux lobes asymétriques, fissure longitudinale, filaments continus, pont bleu-blanc).
- Composants antérieurs : src/components/ChronosBubble.tsx et FullScreenFluidCanvas.tsx.
- Cosmogonie historique : ETAT-DE-LART-PSYCHOLOGIE sur la branche feat/sathyr-optional-interjections-2026-11-12, docs/COSMOS.md, cosmos/olympus.py.
- Divergence à résoudre avant branchement : le code historique donne l'orchestration à SOL ; la vision plus récente donne le rôle du temps/des flux et la façade d'orchestration à CRONOS.

## Lancer
Installer les dépendances avec npm install puis démarrer avec npm run dev. Ouvrir http://localhost:3000/cronos-presence.html.
Vite copie automatiquement public/cronos-presence.html lors du build. HTML autonome, sans CDN, clé API ni permission spéciale.

## Contrat de pilotage visuel
Dans la même fenêtre :

    window.dispatchEvent(new CustomEvent("cronos:presence", {
      detail: {
        state: "dispatching",
        agent: "LAPLACE",
        progress: 0.43,
        amplitude: 0.25,
        label: "Transmission d'une mission à Laplace",
        source: "live"
      }
    }));

Dans une fenêtre de même origine :

    targetWindow.postMessage({
      type: "cronos:presence",
      payload: {state: "working", agent: "MARS", progress: 0.6, source: "live"}
    }, location.origin);

Autre API : window.CronosPresence.setState({state:"thinking"}).

États acceptés : idle, listening, thinking, dispatching, working, speaking, warning, offline.
Options : agent (texte limité à 32 caractères), label (140), progress (ratio 0..1),
amplitude (ratio 0..1), source ("live" = événement reçu, pas preuve de passerelle active).
Sans progress réel : aucun pourcentage affiché. Les messages hors origine sont rejetés.

## Correspondances
| Etat | Sens | Rendu |
| --- | --- | --- |
| idle | Disponible | Respiration lente |
| listening | Ecoute | Ondes et tension |
| thinking | Cognition | Résonance bilatérale |
| dispatching | Délégation | Impulsions périphériques |
| working | Exécution | Circulation interne |
| speaking | Parole | Ondes acoustiques simulées |
| warning | Incident | Halo et pointes orange |
| offline | Dégradation | Noyau sombre, toujours visible |

## Réalisé / reste à faire
- Réalisé : rendu Canvas 2D continu, morphologie organique, huit états,
  réglages, pointer/parallaxe, respect du mouvement réduit, pause en onglet masqué,
  contrat d'événements et navigation depuis l'application React.
- Non réalisé : physique MHD réelle, objet 3D WebGL, tests de performances sur appareils,
  TTS/micro réels, abonnement à un vrai bus Hermes/CRONOS, authentification et
  synchronisation inter-appareils.
- Le moteur ne doit pas inventer d'événements réels : panneau marqué DEMONSTRATION
  tant qu'une passerelle n'a pas émis de signal.
- Suite proposée : adaptateur lecture seule (source, task_id, horodatage, statut, TTL)
  avec validation d'origine et authentification via la passerelle locale.
  Les permissions d'action sont indépendantes de l'animation.
