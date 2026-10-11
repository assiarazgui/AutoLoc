# Stratégie de chargement (Fetch) et de propagation (Cascade) — AutoLoc

Ce document synthétise les choix d'architecture de persistance JPA pour les différentes associations du projet AutoLoc, en justifiant les stratégies de chargement (`FetchType`) et de propagation (`CascadeType` / `orphanRemoval`) retenues.

| Association | Fetch | Cascade | Justification |
| :--- | :--- | :--- | :--- |
| **Contrat → Paiement** | LAZY | ALL + orphanRemoval | Un paiement n'existe que rattaché à son contrat ; supprimer le contrat supprime ses paiements[cite: 1]. |
| **Agence → Vehicule** | LAZY | Aucune | Un véhicule survit à la suppression de son agence[cite: 1]. |
| **Agence → Employe** | LAZY | Aucune | La suppression d'une agence ne doit pas entraîner la suppression en cascade de ses employés[cite: 1]. |
| **Vehicule ↔ Equipement** | LAZY | Aucune | Les équipements sont des entités partagées entre plusieurs véhicules ; utilisation d'un `Set`[cite: 1]. |
| **Client → Reservation** | LAZY | PERSIST | Permet de propager la persistance des réservations lors de l'enregistrement d'un client[cite: 1]. |
| **Reservation → Vehicule** | LAZY | Aucune | Une réservation référence un véhicule existant sans modifier son cycle de vie[cite: 1]. |
| **Reservation ↔ Contrat** | LAZY | ALL (côté Reservation) | Le contrat et la réservation partagent un cycle de vie étroitement lié (relation One-to-One)[cite: 1]. |
| **Vehicule → Maintenance** | LAZY | PERSIST | Permet de persister les interventions de maintenance associées à un véhicule sans cascade destructive[cite: 1]. |