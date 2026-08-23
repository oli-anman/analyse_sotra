# analyse_sotra
Analyse d'un dataset fictif de la sotra pour projet de fin module Data Engenering

# 🚌 Pipeline Data Engineering — Analyse du Trafic des Bus de la SOTRA

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=flat&logo=python)
![PostgreSQL](https://img.shields.io/badge/Database-Supabase%20(PostgreSQL)-green?style=flat&logo=postgresql)
![Apache Airflow](https://img.shields.io/badge/Orchestration-Apache%20Airflow-red?style=flat&logo=apacheairflow)
![Docker](https://img.shields.io/badge/Containers-Docker%20%26%20Compose-blue?style=flat&logo=docker)
![Status](https://img.shields.io/badge/Status-Completed-success)

> **Projet de fin de module : Data Engineering — Promotion 2024-2025**  
> **Sujet 5 — Transports :** Analyse du trafic et de la fréquentation des bus de la SOTRA (Société des Transports Abidjanais) à Abidjan, Côte d'Ivoire.

---

## 📌 1. Description du Projet & Contexte Métier

La **SOTRA** (Société des Transports Abidjanais) fait face à des défis majeurs dans la gestion de la mobilité urbaine à Abidjan : saturation des lignes aux heures de pointe, retards causés par les embouteillages et gestion imprévue des incidents techniques.

Ce projet met en place un **pipeline de données de bout en bout (ETL)** automatisé, documenté et conteneurisé. L'objectif est d'ingérer les données brutes de fréquentation (70 000 voyages générés), d'en garantir la qualité via des règles de validation stricts, de les modéliser selon un **schéma en étoile (Data Warehouse)** hébergé sur Supabase, puis d'orchestrer la collecte via Apache Airflow et de produire un tableau de bord analytique pour optimiser la régulation des lignes et anticiper les pannes.

---

## 📊 2. Résultats & KPI Clés Métiers

Les analyses SQL et les transformations effectuées sur le jeu de données ont mis en évidence trois métriques fondamentales pour l'exploitation du réseau :

| Indicateur (KPI) | Valeur Observée | Interprétation Métier |
| :--- | :--- | :--- |
| **Ligne la plus surchargée** | **L2 (Yopougon - Plateau)** | Taux de remplissage moyen de **112%** aux heures de pointe du matin (06h-09h). |
| **Heure de pointe critique** | **17h00 - 19h00** | Pic d'affluence avec un retard moyen accumulé de **28 minutes** par trajet. |
| **Taux d'incidents techniques** | **4.8% des voyages** | Principale cause : pannes mécaniques concentrées sur les trajets reliant Abobo et Adjamé. |

---

## 🏗️ 3. Architecture du Pipeline de Données

Le pipeline suit une architecture moderne de Data Engineering découpée en 6 blocs fonctionnels :

```text
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│ 1. SOURCE DE DONNÉES   │      │ 2. PIPELINE ETL        │      │ 3. DATA WAREHOUSE      │
│  (CSV / Dataset SOTRA) │ ───> │   (Pandas & Quality)   │ ───> │  (Supabase PostgreSQL) │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘
                                             │                              │
                                             ▼                              ▼
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│ 6. CONTENEURISATION    │      │ 5. ORCHESTRATION       │      │ 4. VISUALISATION       │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘

.
├── dags/
│   └── dag_sotra.py             # DAG Airflow (4 tâches automatisées avec retries)
├── data/
│   └── voyages_sotra_70k.csv    # Dataset brut de 70 000 voyages
├── docs/
│   ├── Rapport_Technique.pdf    # Rapport de projet complet (10-15 pages)
│   └── Soutenance_Slides.pdf    # Presentation de soutenance (10-15 slides)
├── notebooks/
│   └── Pipeline_ETL_SOTRA.ipynb # Notebook Colab exécutable de bout en bout
├── Dockerfile                   # Fichier de configuration Docker
├── docker-compose.yml           # Orchestration des services Airflow & App
├── requirements.txt             # Dépendances Python versionnées
└── README.md                    # Documentation du projet
│  (Docker & Compose)    │ ───> │    (Apache Airflow)    │ ───> │ (Matplotlib Dashboard) │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘
