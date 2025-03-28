<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>Character Creation</title>
		<style>
			/* --------------------------------------
       1) RESET & BASE STYLES
       -------------------------------------- */
			* {
				box-sizing: border-box;
				margin: 0;
				padding: 0;
			}

			body {
				font-family: Arial, sans-serif;
				min-height: 100vh;
				background: linear-gradient(120deg, #5D0F6E 0%, #1F002B 100%);
				color: #e0e0e0;
				overflow-x: hidden;
				position: relative;
			}

			/* --------------------------------------
       New Language Selector
       -------------------------------------- */
			#languageSwitcherContainer {
				position: fixed;
				top: 10px;
				right: 10px;
				z-index: 1000;
			}

			#languageSwitcherButton {
				cursor: pointer;
				font-size: 16px;
				padding: 5px 10px;
			}

			#languageDropdown {
				display: none;
				position: absolute;
				top: 40px;
				right: 0;
				z-index: 1000;
			}

			/* --------------------------------------
       2) BACKGROUND STARS (FIREFLY EFFECT)
       -------------------------------------- */
			.shard-container {
				position: fixed;
				top: 0;
				left: 0;
				width: 100%;
				height: 100%;
				z-index: 0;
				pointer-events: none;
				overflow: hidden;
			}

			.shard {
				position: absolute;
				border-radius: 50%;
				/* round like fireflies */
				background: radial-gradient(circle, #fdd440 0%, transparent 70%);
				opacity: 0.8;
				animation-fill-mode: both;
			}

			@keyframes drift {
				0% {
					transform: translate(0, 0);
				}

				100% {
					transform: translate(var(--drift-x), var(--drift-y));
				}
			}

			@keyframes glow {

				0%,
				100% {
					opacity: 0.5;
					filter: brightness(0.8);
				}

				50% {
					opacity: 1;
					filter: brightness(1.2);
				}
			}

			/* --------------------------------------
       3) MAIN CONTAINER & HEADER
       -------------------------------------- */
			.container {
				position: relative;
				max-width: 1200px;
				margin: auto;
				padding: 20px;
				z-index: 1;
			}

			h1 {
				text-align: center;
				font-size: 3rem;
				color: #ffd700;
				margin-bottom: 30px;
				text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.6);
			}

			/* --------------------------------------
       4) FORM LABELS & PRE-MADE SELECT
       -------------------------------------- */
			.section label {
				font-size: 1.2rem;
				font-weight: bold;
				color: white;
				margin-right: 10px;
			}

			.section input[type="text"],
			.section select {
				font-size: 1rem;
				padding: 6px 10px;
				border: 1px solid #bb86fc;
				border-radius: 4px;
				background: #2a2a2a;
				color: #e0e0e0;
			}

			.section {
				margin-bottom: 15px;
				display: flex;
				align-items: center;
			}

			.section input[type="checkbox"] {
				width: auto;
				margin-right: 8px;
				transform: scale(1.2);
			}

			textarea {
				background: #2a2a2a;
				color: #fff;
				font-size: 1rem;
				padding: 6px 10px;
				border: 1px solid #bb86fc;
				border-radius: 4px;
			}

			.perk-container {
				display: flex;
				align-items: center;
			}

			.perk-number {
				width: 150px !important;
				background: #2a2a2a;
				color: #fff;
				font-size: 1.4em;
				padding: 8px 10px;
				border: 1px solid #bb86fc;
				border-radius: 4px;
				margin-right: 10px;
			}

			.perk-text {
				flex: 1;
				background: #2a2a2a;
				color: #fff;
				font-size: 1.4em;
				padding: 8px 10px;
				border: 1px solid #bb86fc;
				border-radius: 4px;
				margin-right: 10px;
			}

			/* --------------------------------------
       5) ASPECT BLOCKS
       -------------------------------------- */
			.aspect-block {
				margin-bottom: 30px;
				padding: 15px;
				border-radius: 8px;
				background-color: rgba(0, 0, 0, 0.2);
			}

			#egoBlock {
				background-color: rgba(88, 21, 135, 0.3);
			}

			#corpusBlock {
				background-color: rgba(135, 21, 21, 0.3);
			}

			#auraBlock {
				background-color: rgba(21, 135, 94, 0.3);
			}

			#animaBlock {
				background-color: rgba(135, 100, 21, 0.3);
			}

			.aspect-header {
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-bottom: 15px;
			}

			.aspect-title {
				font-size: 1.5em;
				color: #bb86fc;
				text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
			}

			/* --------------------------------------
       6) SKILL ITEMS with color-coded left border
       -------------------------------------- */
			.hyper-item {
				margin-left: 20px;
				margin-bottom: 10px;
				padding: 8px;
				background-color: #2a2a2a;
				border-left: 4px solid #bb86fc;
				position: relative;
				border-radius: 4px;
			}

			.macro-item {
				margin-left: 40px;
				margin-bottom: 8px;
				padding: 6px;
				background-color: #3a3a3a;
				border-left: 3px solid #03dac6;
				position: relative;
				border-radius: 4px;
			}

			.sub-item {
				margin-left: 60px;
				margin-bottom: 5px;
				padding: 4px;
				background-color: #4a4a4a;
				border-left: 2px solid #cf6679;
				position: relative;
				border-radius: 4px;
			}

			.perk-item {
				margin-bottom: 10px;
				padding: 8px;
				background-color: #2a2a2a;
				border-radius: 4px;
			}

			.flex-container {
				display: flex;
				gap: 10px;
				align-items: center;
				margin: 5px 0;
			}

			.level-input {
				flex: 0 0 60px;
				background: #1e1e1e;
				border: 1px solid #444;
				color: #fff;
				padding: 4px;
				border-radius: 4px;
			}

			/* --------------------------------------
       7) BUTTONS
       -------------------------------------- */
			button {
				padding: 8px 12px;
				background-color: #03dac6;
				color: #121212;
				border: none;
				cursor: pointer;
				border-radius: 4px;
				font-weight: bold;
			}

			button:hover {
				background-color: #018786;
			}

			.remove-btn {
				background-color: #cf6679;
			}

			.remove-btn:hover {
				background-color: #b00020;
			}

			/* --------------------------------------
       8) VALIDATION & LEFTOVER POINTS
       -------------------------------------- */
			.invalid {
				outline: 2px solid red;
			}

			.level-dropdown {
				margin-left: 10px;
				background: #1e1e1e;
				border: 1px solid #444;
				color: #fff;
				border-radius: 4px;
				padding: 4px;
			}

			.leftover-points {
				margin-left: 10px;
				font-weight: bold;
				color: #ffd700;
				text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
			}

			/* --------------------------------------
       9) SAVE/LOAD BUTTONS
       -------------------------------------- */
			.section:last-of-type {
				margin-top: 20px;
				justify-content: center;
				gap: 20px;
			}
		</style>
		<!-- Page Translations -->
		<script>
			const translations = {
				en: {
					ui: {
						title: "Character Creation",
						preMadeCharacterLabel: "Pre-made Character:",
						characterNameLabel: "Character Name:",
						characterTypeLabel: "Character Type:",
						startingModeLabel: "Starting Mode",
						backstoryLabel: "Back Story:",
						addHyperButton: "Add Hyper",
						addMacroButton: "Add Macro",
						addSubButton: "Add Sub",
						removeButton: "Remove",
						addPerkButton: "Add Perk",
						saveCharacterButton: "Save Character",
						loadCharacterButton: "Load Character",
						preMadeDefaultOption: "-- Select a Pre-made Character --"
					},
					preMadeCharacters: [{
						"name": "Empty Fields",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "Base Persona",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "Speak",
								"level": null,
								"macroSkills": [{
									"name": "Speak",
									"level": null,
									"subSkills": [{
										"name": "Intimidation",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Persuasion",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Bargaining",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Deception",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Diplomacy",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Mind",
								"level": null,
								"macroSkills": [{
									"name": "Mind",
									"level": null,
									"subSkills": [{
										"name": "Vision",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Meditation",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Logic",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Memory",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Will",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "Clash",
								"level": null,
								"macroSkills": [{
									"name": "Attack",
									"level": null,
									"subSkills": [{
										"name": "Hands",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Legs",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "One Hand",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Two Hands",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Dual Wield",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Archery",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Throwing",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Polearms",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Defense",
									"level": null,
									"subSkills": [{
										"name": "Skin",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Shield",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Light Armor",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Heavy Armor",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Crafting",
								"level": null,
								"macroSkills": [{
									"name": "Building",
									"level": null,
									"subSkills": [{
										"name": "Weapons",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Items",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Devices",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Engineering",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Alchemy",
								"level": null,
								"macroSkills": [{
									"name": "Natural",
									"level": null,
									"subSkills": [{
										"name": "Incantation",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Written",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Preparatory",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Arcane",
									"level": null,
									"subSkills": [{
										"name": "Incantation",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Written",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Preparatory",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Pactbound",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Movement",
								"level": null,
								"macroSkills": [{
									"name": "Visible",
									"level": null,
									"subSkills": [{
										"name": "Running",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Dodging",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Swimming",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Jumping",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Invisible",
									"level": null,
									"subSkills": [{
										"name": "Sliming",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Lockpicking",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Stealing",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Climbing",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Animal",
									"level": null,
									"subSkills": [{
										"name": "Riding",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "Elemental",
								"level": null,
								"macroSkills": [{
									"name": "Destruction",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Icy",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Acidic",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Percussive",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Galvanic",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Alteration",
									"level": null,
									"subSkills": [{
										"name": "Warm",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Cold",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Poisonous",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vibrational",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Electric",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Restoration",
								"level": null,
								"macroSkills": [{
									"name": "Physical",
									"level": null,
									"subSkills": [{
										"name": "Wounds",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Object",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Poison",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Ethereal",
									"level": null,
									"subSkills": [{
										"name": "Mind",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Spiritual",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Invocation",
								"level": null,
								"macroSkills": [{
									"name": "Physical",
									"level": null,
									"subSkills": [{
										"name": "Constructive",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Summoning",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Ethereal",
									"level": null,
									"subSkills": [{
										"name": "Aura",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Occult",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Divination",
								"level": null,
								"macroSkills": [{
									"name": "Divination",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Perception",
								"level": null,
								"macroSkills": [{
									"name": "Illusion",
									"level": null,
									"subSkills": [{
										"name": "Physical",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Mental",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Reminiscent",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Obscuring",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Clairvoyance",
									"level": null,
									"subSkills": [{
										"name": "Foreseeing",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Retro-cognition",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				it: {
					ui: {
						title: "Creazione del Personaggio",
						preMadeCharacterLabel: "Personaggio Predefinito:",
						characterNameLabel: "Nome del Personaggio:",
						characterTypeLabel: "Elementi Personaggio:",
						startingModeLabel: "Modalità Iniziale",
						backstoryLabel: "Storia:",
						addHyperButton: "Aggiungi Hyper",
						addMacroButton: "Aggiungi Macro",
						addSubButton: "Aggiungi Sub",
						removeButton: "Rimuovi",
						addPerkButton: "Aggiungi Perk",
						saveCharacterButton: "Salva Personaggio",
						loadCharacterButton: "Carica Personaggio",
						preMadeDefaultOption: "-- Seleziona un Personaggio Predefinito --"
					},
					preMadeCharacters: [{
						"name": "Selezione Vuota",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "Persona Base",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "Parlare",
								"level": null,
								"macroSkills": [{
									"name": "Parlare",
									"level": null,
									"subSkills": [{
										"name": "Intimidazione",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Persuasione",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Sconti",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Inganno",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Diplomazia",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Mente",
								"level": null,
								"macroSkills": [{
									"name": "Mente",
									"level": null,
									"subSkills": [{
										"name": "Visione",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Meditazione",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Logica",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Memoria",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Volontà",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "Scontro",
								"level": null,
								"macroSkills": [{
									"name": "Attacco",
									"level": null,
									"subSkills": [{
										"name": "Mani",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Gambe",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Una Mano",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Due Mani",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Doppio Maneggio",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Arcieria",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Lancio",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armi Lunghe",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Difesa",
									"level": null,
									"subSkills": [{
										"name": "Pelle",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Scudo",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armatura Leggera",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armatura Pesante",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Artigianato",
								"level": null,
								"macroSkills": [{
									"name": "Costruzione",
									"level": null,
									"subSkills": [{
										"name": "Armi",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Oggetti",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Dispositivi",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ingegneria",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Alchimia",
								"level": null,
								"macroSkills": [{
									"name": "Naturale",
									"level": null,
									"subSkills": [{
										"name": "Incantatrice",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Scritta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Preparatoria",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Arcana",
									"level": null,
									"subSkills": [{
										"name": "Incantatrice",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Scritta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Preparatoria",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Con Patto",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Movimento",
								"level": null,
								"macroSkills": [{
									"name": "Visibile",
									"level": null,
									"subSkills": [{
										"name": "Corsa",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Schivata",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Nuoto",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Balzare",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Invisibile",
									"level": null,
									"subSkills": [{
										"name": "Soppiatto",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Scassinare",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Rubare",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Scalare",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Animale",
									"level": null,
									"subSkills": [{
										"name": "Cavalcare",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "Elementale",
								"level": null,
								"macroSkills": [{
									"name": "Distruzione",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Gelida",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Acida",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Percussiva",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Galvanica",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Alterazione",
									"level": null,
									"subSkills": [{
										"name": "Calda",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Fredda",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Velenosa",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vibrazionale",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Elettrica",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Ristorazione",
								"level": null,
								"macroSkills": [{
									"name": "Fisica",
									"level": null,
									"subSkills": [{
										"name": "Ferite",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Oggetto",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Veleno",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Eterea",
									"level": null,
									"subSkills": [{
										"name": "Mente",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Spirituale",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Evocazione",
								"level": null,
								"macroSkills": [{
									"name": "Fisica",
									"level": null,
									"subSkills": [{
										"name": "Costruttiva",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Richiamo",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Eterea",
									"level": null,
									"subSkills": [{
										"name": "Aura",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Occulta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astrale",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Divinazione",
								"level": null,
								"macroSkills": [{
									"name": "Divinazione",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Percezione",
								"level": null,
								"macroSkills": [{
									"name": "Illusione",
									"level": null,
									"subSkills": [{
										"name": "Fisica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Mentale",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Reminiscente",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Oscuramento",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Chiaroveggenza",
									"level": null,
									"subSkills": [{
										"name": "Preveggenza",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Retrocognizione",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astrale",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				pt: {
					ui: {
						title: "Criação de Personagem",
						preMadeCharacterLabel: "Personagem Pré-definido:",
						characterNameLabel: "Nome do Personagem:",
						characterTypeLabel: "Tipo de Personagem:",
						startingModeLabel: "Modo Inicial",
						backstoryLabel: "História:",
						addHyperButton: "Adicionar Hiper",
						addMacroButton: "Adicionar Macro",
						addSubButton: "Adicionar Sub",
						removeButton: "Remover",
						addPerkButton: "Adicionar Perk",
						saveCharacterButton: "Salvar Personagem",
						loadCharacterButton: "Carregar Personagem",
						preMadeDefaultOption: "-- Selecione um Personagem Pré-definido --"
					},
					preMadeCharacters: [{
						"name": "Campos Vazios",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "Persona Base",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "Falar",
								"level": null,
								"macroSkills": [{
									"name": "Falar",
									"level": null,
									"subSkills": [{
										"name": "Intimidação",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Persuasão",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Negociação",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Engano",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Diplomacia",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Mente",
								"level": null,
								"macroSkills": [{
									"name": "Mente",
									"level": null,
									"subSkills": [{
										"name": "Visão",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Meditação",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Lógica",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Memória",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vontade",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "Confronto",
								"level": null,
								"macroSkills": [{
									"name": "Ataque",
									"level": null,
									"subSkills": [{
										"name": "Mãos",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Pernas",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Uma Mão",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Duas Mãos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Uso Duplo",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Arqueria",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Arremesso",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armas de Haste",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Defesa",
									"level": null,
									"subSkills": [{
										"name": "Pele",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Escudo",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armadura Leve",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armadura Pesada",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Artesanato",
								"level": null,
								"macroSkills": [{
									"name": "Construção",
									"level": null,
									"subSkills": [{
										"name": "Armas",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Itens",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Dispositivos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Engenharia",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Alquimia",
								"level": null,
								"macroSkills": [{
									"name": "Natural",
									"level": null,
									"subSkills": [{
										"name": "Do encantamento",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Da escrita",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Da reparação",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Arcana",
									"level": null,
									"subSkills": [{
										"name": "Do encantamento",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Da escrito",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Da reparação",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vinculado por Pacto",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Movimento",
								"level": null,
								"macroSkills": [{
									"name": "Visível",
									"level": null,
									"subSkills": [{
										"name": "Corrida",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Esquivada",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Natação",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Salto",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Invisível",
									"level": null,
									"subSkills": [{
										"name": "Deslizamento",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Arrombamento",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Roubo",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Escalada",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Animal",
									"level": null,
									"subSkills": [{
										"name": "Cavalgar",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "Elemental",
								"level": null,
								"macroSkills": [{
									"name": "Destruição",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Gélida",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ácida",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Percussiva",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Galvânica",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Alteração",
									"level": null,
									"subSkills": [{
										"name": "Quente",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Fria",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Venenosa",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vibracional",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Elétrica",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Restauração",
								"level": null,
								"macroSkills": [{
									"name": "Físico",
									"level": null,
									"subSkills": [{
										"name": "Feridas",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Objeto",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Veneno",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Etéreo",
									"level": null,
									"subSkills": [{
										"name": "Mente",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Espiritual",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Invocação",
								"level": null,
								"macroSkills": [{
									"name": "Físico",
									"level": null,
									"subSkills": [{
										"name": "Construtivo",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Convocação",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Etéreo",
									"level": null,
									"subSkills": [{
										"name": "Aura",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Oculto",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Divinação",
								"level": null,
								"macroSkills": [{
									"name": "Divinação",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Percepção",
								"level": null,
								"macroSkills": [{
									"name": "Ilusão",
									"level": null,
									"subSkills": [{
										"name": "Físico",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Mental",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Reminiscente",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Obscurante",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Clarividência",
									"level": null,
									"subSkills": [{
										"name": "Previsão",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Retrocognição",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				es: {
					"ui": {
						"title": "Creación de Personaje",
						"preMadeCharacterLabel": "Personaje Predefinido:",
						"characterNameLabel": "Nombre del Personaje:",
						"characterTypeLabel": "Tipo de Personaje:",
						"startingModeLabel": "Modo Inicial",
						"backstoryLabel": "Historia:",
						"addHyperButton": "Añadir Hyper",
						"addMacroButton": "Añadir Macro",
						"addSubButton": "Añadir Sub",
						"removeButton": "Eliminar",
						"addPerkButton": "Añadir Perk",
						"saveCharacterButton": "Guardar Personaje",
						"loadCharacterButton": "Cargar Personaje",
						"preMadeDefaultOption": "-- Selecciona un Personaje Predefinido --"
					},
					"preMadeCharacters": [{
						"name": "Campos Vacíos",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "Persona Base",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "Hablar",
								"level": null,
								"macroSkills": [{
									"name": "Hablar",
									"level": null,
									"subSkills": [{
										"name": "Intimidación",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Persuasión",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Negociación",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Engaño",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Diplomacia",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Mente",
								"level": null,
								"macroSkills": [{
									"name": "Mente",
									"level": null,
									"subSkills": [{
										"name": "Visión",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Meditación",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Lógica",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Memoria",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Voluntad",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "Confrontación",
								"level": null,
								"macroSkills": [{
									"name": "Ataque",
									"level": null,
									"subSkills": [{
										"name": "Manos",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Piernas",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Una Mano",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Dos Manos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Doble Empuñamiento",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Tiro con Arco",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Lanzamiento",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armas de asta",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Defensa",
									"level": null,
									"subSkills": [{
										"name": "Piel",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Escudo",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armadura Ligera",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Armadura Pesada",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Artesanía",
								"level": null,
								"macroSkills": [{
									"name": "Construcción",
									"level": null,
									"subSkills": [{
										"name": "Armas",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Objetos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Dispositivos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ingeniería",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Alquimia",
								"level": null,
								"macroSkills": [{
									"name": "Natural",
									"level": null,
									"subSkills": [{
										"name": "Encantación",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Escrita",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Preparatoria",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Arcana",
									"level": null,
									"subSkills": [{
										"name": "Encantación",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Escrita",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Preparatoria",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vinculado por Pacto",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Movimiento",
								"level": null,
								"macroSkills": [{
									"name": "Visible",
									"level": null,
									"subSkills": [{
										"name": "Correr",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Esquivar",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Nadar",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Saltar",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Invisible",
									"level": null,
									"subSkills": [{
										"name": "Deslizamiento",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Forzar Cerraduras",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Robar",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Escalar",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Animal",
									"level": null,
									"subSkills": [{
										"name": "Montar",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "Elemental",
								"level": null,
								"macroSkills": [{
									"name": "Destrucción",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Gélida",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ácida",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Percusiva",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Galvánica",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Alteración",
									"level": null,
									"subSkills": [{
										"name": "Cálida",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Fría",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Venenosa",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vibracional",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Eléctrica",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Restauración",
								"level": null,
								"macroSkills": [{
									"name": "Física",
									"level": null,
									"subSkills": [{
										"name": "Heridas",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Objeto",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Veneno",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Etérea",
									"level": null,
									"subSkills": [{
										"name": "Mente",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Espiritual",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Invocación",
								"level": null,
								"macroSkills": [{
									"name": "Física",
									"level": null,
									"subSkills": [{
										"name": "Constructiva",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Convocación",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Etéreo",
									"level": null,
									"subSkills": [{
										"name": "Aura",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Oculta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Adivinación",
								"level": null,
								"macroSkills": [{
									"name": "Adivinación",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Percepción",
								"level": null,
								"macroSkills": [{
									"name": "Ilusión",
									"level": null,
									"subSkills": [{
										"name": "Física",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Mental",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Reminiscente",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Oscurecedor",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Clarividencia",
									"level": null,
									"subSkills": [{
										"name": "Previsión",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Retrocognición",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				zh: {
					"ui": {
						"title": "角色创建",
						"preMadeCharacterLabel": "预设角色:",
						"characterNameLabel": "角色名称:",
						"characterTypeLabel": "角色类型:",
						"startingModeLabel": "初始模式",
						"backstoryLabel": "背景故事:",
						"addHyperButton": "添加 Hyper",
						"addMacroButton": "添加 Macro",
						"addSubButton": "添加 Sub",
						"removeButton": "删除",
						"addPerkButton": "添加 Perk",
						"saveCharacterButton": "保存角色",
						"loadCharacterButton": "加载角色",
						"preMadeDefaultOption": "-- 选择预设角色 --"
					},
					"preMadeCharacters": [{
						"name": "空白字段",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "基础角色",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "说话",
								"level": null,
								"macroSkills": [{
									"name": "说话",
									"level": null,
									"subSkills": [{
										"name": "恐吓",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "说服",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "谈判",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "欺骗",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "外交",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "心智",
								"level": null,
								"macroSkills": [{
									"name": "心智",
									"level": null,
									"subSkills": [{
										"name": "洞察力",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "冥想",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "逻辑",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "记忆",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "意志",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "对抗",
								"level": null,
								"macroSkills": [{
									"name": "攻击",
									"level": null,
									"subSkills": [{
										"name": "手",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "腿",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "单手",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "双手",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "双持",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "射箭",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "投掷",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "长柄武器",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "防御",
									"level": null,
									"subSkills": [{
										"name": "皮肤",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "盾牌",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "轻甲",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "重甲",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "工艺",
								"level": null,
								"macroSkills": [{
									"name": "建造",
									"level": null,
									"subSkills": [{
										"name": "武器",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "物品",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "装置",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "工程",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "炼金术",
								"level": null,
								"macroSkills": [{
									"name": "自然",
									"level": null,
									"subSkills": [{
										"name": "咒语",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "书写",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "预备",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "奥术",
									"level": null,
									"subSkills": [{
										"name": "咒语",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "书写",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "预备",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "契约绑定",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "移动",
								"level": null,
								"macroSkills": [{
									"name": "可见",
									"level": null,
									"subSkills": [{
										"name": "奔跑",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "躲闪",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "游泳",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "跳跃",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "隐形",
									"level": null,
									"subSkills": [{
										"name": "滑行",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "撬锁",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "偷窃",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "攀爬",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "动物",
									"level": null,
									"subSkills": [{
										"name": "骑乘",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "元素",
								"level": null,
								"macroSkills": [{
									"name": "毁灭",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "冰冷",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "酸性",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "打击",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "电击",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "变更",
									"level": null,
									"subSkills": [{
										"name": "温暖",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "寒冷",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "有毒",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "振动",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "电能",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "恢复",
								"level": null,
								"macroSkills": [{
									"name": "物理",
									"level": null,
									"subSkills": [{
										"name": "伤口",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "物体",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "毒",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "以太",
									"level": null,
									"subSkills": [{
										"name": "精神",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "灵性",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "召唤",
								"level": null,
								"macroSkills": [{
									"name": "物理",
									"level": null,
									"subSkills": [{
										"name": "构造",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "召唤",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "以太",
									"level": null,
									"subSkills": [{
										"name": "光环",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "隐藏",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "星界",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "占卜",
								"level": null,
								"macroSkills": [{
									"name": "占卜",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "感知",
								"level": null,
								"macroSkills": [{
									"name": "幻象",
									"level": null,
									"subSkills": [{
										"name": "物理",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "心理",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "追忆",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "暗化",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "透视",
									"level": null,
									"subSkills": [{
										"name": "预见",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "溯往",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "星界",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				ja: {
					"ui": {
						"title": "キャラクター作成",
						"preMadeCharacterLabel": "プリセットキャラクター:",
						"characterNameLabel": "キャラクター名:",
						"characterTypeLabel": "キャラクタータイプ:",
						"startingModeLabel": "初期モード",
						"backstoryLabel": "背景ストーリー:",
						"addHyperButton": "Hyperを追加",
						"addMacroButton": "Macroを追加",
						"addSubButton": "Subを追加",
						"removeButton": "削除",
						"addPerkButton": "Perkを追加",
						"saveCharacterButton": "キャラクターを保存",
						"loadCharacterButton": "キャラクターを読み込み",
						"preMadeDefaultOption": "-- プリセットキャラクターを選択 --"
					},
					"preMadeCharacters": [{
						"name": "空欄",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "ベースパーソナ",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "話す",
								"level": null,
								"macroSkills": [{
									"name": "話す",
									"level": null,
									"subSkills": [{
										"name": "威圧",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "説得",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "値切り",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "欺瞞",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "外交",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "心",
								"level": null,
								"macroSkills": [{
									"name": "心",
									"level": null,
									"subSkills": [{
										"name": "洞察",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "瞑想",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "論理",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "記憶",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "意志",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "対抗",
								"level": null,
								"macroSkills": [{
									"name": "攻撃",
									"level": null,
									"subSkills": [{
										"name": "手",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "足",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "片手",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "両手",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "二刀流",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "弓術",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "投擲",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "長柄武器",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "防御",
									"level": null,
									"subSkills": [{
										"name": "皮膚",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "盾",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "軽装甲",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "重装甲",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "工作",
								"level": null,
								"macroSkills": [{
									"name": "製作",
									"level": null,
									"subSkills": [{
										"name": "武器",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "アイテム",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "装置",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "工学",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "錬金術",
								"level": null,
								"macroSkills": [{
									"name": "自然",
									"level": null,
									"subSkills": [{
										"name": "呪文",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "書写",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "準備",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "奥術",
									"level": null,
									"subSkills": [{
										"name": "呪文",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "書写",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "準備",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "契約束縛",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "移動",
								"level": null,
								"macroSkills": [{
									"name": "可視",
									"level": null,
									"subSkills": [{
										"name": "走る",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "回避",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "泳ぐ",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "跳躍",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "不可視",
									"level": null,
									"subSkills": [{
										"name": "滑行",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "ロックピッキング",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "盗み",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "登攀",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "動物",
									"level": null,
									"subSkills": [{
										"name": "騎乗",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "元素",
								"level": null,
								"macroSkills": [{
									"name": "破壊",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "氷結",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "酸性",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "打撃",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "電撃",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "変化",
									"level": null,
									"subSkills": [{
										"name": "温暖",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "寒冷",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "毒性",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "振動",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "電気",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "回復",
								"level": null,
								"macroSkills": [{
									"name": "物理",
									"level": null,
									"subSkills": [{
										"name": "傷",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "物体",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "毒",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "霊的",
									"level": null,
									"subSkills": [{
										"name": "精神",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "霊性",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "召喚",
								"level": null,
								"macroSkills": [{
									"name": "物理",
									"level": null,
									"subSkills": [{
										"name": "構造",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "召喚",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "霊的",
									"level": null,
									"subSkills": [{
										"name": "オーラ",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "オカルト",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "アストラル",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "占い",
								"level": null,
								"macroSkills": [{
									"name": "占い",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "感知",
								"level": null,
								"macroSkills": [{
									"name": "幻影",
									"level": null,
									"subSkills": [{
										"name": "物理",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "精神",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "回想",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "隠蔽",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "透視",
									"level": null,
									"subSkills": [{
										"name": "予見",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "回顧",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "星界",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				nl: {
					"ui": {
						"title": "Karaktercreatie",
						"preMadeCharacterLabel": "Vooraf Gemaakt Karakter:",
						"characterNameLabel": "Karakternaam:",
						"characterTypeLabel": "Karaktertype:",
						"startingModeLabel": "Beginmodus",
						"backstoryLabel": "Achtergrondverhaal:",
						"addHyperButton": "Voeg Hyper Toe",
						"addMacroButton": "Voeg Macro Toe",
						"addSubButton": "Voeg Sub Toe",
						"removeButton": "Verwijderen",
						"addPerkButton": "Voeg Perk Toe",
						"saveCharacterButton": "Sla Karakter op",
						"loadCharacterButton": "Laad Karakter",
						"preMadeDefaultOption": "-- Selecteer een vooraf gemaakt karakter --"
					},
					"preMadeCharacters": [{
						"name": "Lege Velden",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "Basispersoon",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "Spreken",
								"level": null,
								"macroSkills": [{
									"name": "Spreken",
									"level": null,
									"subSkills": [{
										"name": "Intimidatie",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Overtuigen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Onderhandelen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Bedrog",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Diplomatie",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Geest",
								"level": null,
								"macroSkills": [{
									"name": "Geest",
									"level": null,
									"subSkills": [{
										"name": "Visie",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Meditatie",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Logica",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Geheugen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Wil",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "Botsing",
								"level": null,
								"macroSkills": [{
									"name": "Aanval",
									"level": null,
									"subSkills": [{
										"name": "Handen",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Benen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Eén Hand",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Twee Handen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Dubbel Wapengebruik",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Boogschieten",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Werpen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Stafwapens",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Verdediging",
									"level": null,
									"subSkills": [{
										"name": "Huid",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Schild",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Lichte Rüstung",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Zware Rüstung",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Ambacht",
								"level": null,
								"macroSkills": [{
									"name": "Bouwen",
									"level": null,
									"subSkills": [{
										"name": "Wapens",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Voorwerpen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Apparaten",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Techniek",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Alchemie",
								"level": null,
								"macroSkills": [{
									"name": "Natuurlijk",
									"level": null,
									"subSkills": [{
										"name": "Incantatie",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Geschreven",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Voorbereidend",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Arcane",
									"level": null,
									"subSkills": [{
										"name": "Incantatie",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Geschreven",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Voorbereidend",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Verbonden door Pact",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Beweging",
								"level": null,
								"macroSkills": [{
									"name": "Zichtbaar",
									"level": null,
									"subSkills": [{
										"name": "Rennen",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ontwijken",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Zwemmen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Springen",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Onzichtbaar",
									"level": null,
									"subSkills": [{
										"name": "Sluipen",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Slotenkraken",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Stelen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Klimmen",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Dierlijk",
									"level": null,
									"subSkills": [{
										"name": "Rijden",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "Elementair",
								"level": null,
								"macroSkills": [{
									"name": "Verwoesting",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ijsachtig",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Zuur",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Percussief",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Galvanisch",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Verandering",
									"level": null,
									"subSkills": [{
										"name": "Warm",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Koud",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Giftig",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vibrerend",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Elektrisch",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Herstel",
								"level": null,
								"macroSkills": [{
									"name": "Fysiek",
									"level": null,
									"subSkills": [{
										"name": "Wonden",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Object",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Gif",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Etherisch",
									"level": null,
									"subSkills": [{
										"name": "Geest",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Spiritueel",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Oproeping",
								"level": null,
								"macroSkills": [{
									"name": "Fysiek",
									"level": null,
									"subSkills": [{
										"name": "Constructief",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Oproepen",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Etherisch",
									"level": null,
									"subSkills": [{
										"name": "Aura",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Occult",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astrale",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Divinatie",
								"level": null,
								"macroSkills": [{
									"name": "Divinatie",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Waarneming",
								"level": null,
								"macroSkills": [{
									"name": "Illusie",
									"level": null,
									"subSkills": [{
										"name": "Fysiek",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Mentaal",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Herinnerend",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Verduisterend",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Helderziendheid",
									"level": null,
									"subSkills": [{
										"name": "Voorzien",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Retro-cognitie",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astrale",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				de: {
					"ui": {
						"title": "Charaktererstellung",
						"preMadeCharacterLabel": "Vorgefertigter Charakter:",
						"characterNameLabel": "Charaktername:",
						"characterTypeLabel": "Charaktertyp:",
						"startingModeLabel": "Startmodus",
						"backstoryLabel": "Hintergrundgeschichte:",
						"addHyperButton": "Hyper hinzufügen",
						"addMacroButton": "Macro hinzufügen",
						"addSubButton": "Sub hinzufügen",
						"removeButton": "Entfernen",
						"addPerkButton": "Perk hinzufügen",
						"saveCharacterButton": "Charakter speichern",
						"loadCharacterButton": "Charakter laden",
						"preMadeDefaultOption": "-- Wähle einen vorgefertigten Charakter --"
					},
					"preMadeCharacters": [{
						"name": "Leere Felder",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "Basispersona",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "Sprechen",
								"level": null,
								"macroSkills": [{
									"name": "Sprechen",
									"level": null,
									"subSkills": [{
										"name": "Einschüchterung",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Überredung",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Feilschen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Täuschung",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Diplomatie",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Verstand",
								"level": null,
								"macroSkills": [{
									"name": "Verstand",
									"level": null,
									"subSkills": [{
										"name": "Vision",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Meditation",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Logik",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Gedächtnis",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Wille",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "Kampf",
								"level": null,
								"macroSkills": [{
									"name": "Angriff",
									"level": null,
									"subSkills": [{
										"name": "Hände",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Beine",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Einhand",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Zweihand",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Beidhändig",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Bogenschießen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Werfen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Stangenwaffen",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Verteidigung",
									"level": null,
									"subSkills": [{
										"name": "Haut",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Schild",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Leichte Rüstung",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Schwere Rüstung",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Handwerk",
								"level": null,
								"macroSkills": [{
									"name": "Bauen",
									"level": null,
									"subSkills": [{
										"name": "Waffen",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Gegenstände",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Geräte",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ingenieurwesen",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Alchemie",
								"level": null,
								"macroSkills": [{
									"name": "Natürlich",
									"level": null,
									"subSkills": [{
										"name": "Beschwörung",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Geschrieben",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vorbereitung",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Arcane",
									"level": null,
									"subSkills": [{
										"name": "Beschwörung",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Geschrieben",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vorbereitung",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Paktgebunden",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Bewegung",
								"level": null,
								"macroSkills": [{
									"name": "Sichtbar",
									"level": null,
									"subSkills": [{
										"name": "Rennen",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ausweichen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Schwimmen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Springen",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Unsichtbar",
									"level": null,
									"subSkills": [{
										"name": "Rutschen",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Schlossknacken",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Stehlen",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Klettern",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Tierisch",
									"level": null,
									"subSkills": [{
										"name": "Reiten",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "Elementar",
								"level": null,
								"macroSkills": [{
									"name": "Zerstörung",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Eisig",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Säurehaltig",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Perkussiv",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Galvanisch",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Veränderung",
									"level": null,
									"subSkills": [{
										"name": "Warm",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Kalt",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Giftig",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Vibrierend",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Elektrisch",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Wiederherstellung",
								"level": null,
								"macroSkills": [{
									"name": "Physisch",
									"level": null,
									"subSkills": [{
										"name": "Wunden",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Objekt",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Gift",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Ätherisch",
									"level": null,
									"subSkills": [{
										"name": "Geist",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Spirituell",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Beschwörung",
								"level": null,
								"macroSkills": [{
									"name": "Physisch",
									"level": null,
									"subSkills": [{
										"name": "Konstruktiv",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Beschwörung",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Ätherisch",
									"level": null,
									"subSkills": [{
										"name": "Aura",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Okkult",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Wahrsagung",
								"level": null,
								"macroSkills": [{
									"name": "Wahrsagung",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "Wahrnehmung",
								"level": null,
								"macroSkills": [{
									"name": "Illusion",
									"level": null,
									"subSkills": [{
										"name": "Physisch",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Mental",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Erinnernd",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Verschleiern",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "Hellsehen",
									"level": null,
									"subSkills": [{
										"name": "Voraussicht",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Retro-Kognition",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Astral",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
				hi: {
					"ui": {
						"title": "चरित्र निर्माण",
						"preMadeCharacterLabel": "पूर्वनिर्मित चरित्र:",
						"characterNameLabel": "चरित्र का नाम:",
						"characterTypeLabel": "चरित्र प्रकार:",
						"startingModeLabel": "प्रारंभिक मोड",
						"backstoryLabel": "पृष्ठभूमि कथा:",
						"addHyperButton": "हाइपर जोड़ें",
						"addMacroButton": "मैक्रो जोड़ें",
						"addSubButton": "सब जोड़ें",
						"removeButton": "हटाएं",
						"addPerkButton": "पर्क जोड़ें",
						"saveCharacterButton": "चरित्र सहेजें",
						"loadCharacterButton": "चरित्र लोड करें",
						"preMadeDefaultOption": "-- एक पूर्वनिर्मित चरित्र चुनें --"
					},
					"preMadeCharacters": [{
						"name": "खाली क्षेत्र",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [],
							"corpus": [],
							"aura": [],
							"anima": []
						}
					}, {
						"name": "मूल व्यक्तित्व",
						"type": "BOUND ONE",
						"startingMode": true,
						"aspects": {
							"ego": [{
								"name": "बोलना",
								"level": null,
								"macroSkills": [{
									"name": "बोलना",
									"level": null,
									"subSkills": [{
										"name": "धमकाना",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "मनाना",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "मोलभाव",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "धोखा",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "कूटनीति",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "मन",
								"level": null,
								"macroSkills": [{
									"name": "मन",
									"level": null,
									"subSkills": [{
										"name": "दृष्टि",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "ध्यान",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "तर्क",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "स्मृति",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "इच्छाशक्ति",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"corpus": [{
								"name": "संघर्ष",
								"level": null,
								"macroSkills": [{
									"name": "आक्रमण",
									"level": null,
									"subSkills": [{
										"name": "हाथ",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "पैर",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "एक हाथ",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "दो हाथ",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "दोहरी हथियार",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "तीरंदाजी",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "फेंकना",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "लंबी छड़ी हथियार",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "रक्षा",
									"level": null,
									"subSkills": [{
										"name": "त्वचा",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "ढाल",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "हल्का कवच",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "भारी कवच",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "हस्तनिर्माण",
								"level": null,
								"macroSkills": [{
									"name": "निर्माण",
									"level": null,
									"subSkills": [{
										"name": "हथियार",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "वस्तुएं",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "उपकरण",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "अभियंत्रण",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "अल्केमी",
								"level": null,
								"macroSkills": [{
									"name": "प्राकृतिक",
									"level": null,
									"subSkills": [{
										"name": "मंत्रोच्चारण",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "लिखित",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "तैयारी",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "अलौकिक",
									"level": null,
									"subSkills": [{
										"name": "मंत्रोच्चारण",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "लिखित",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "तैयारी",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "पैक्ट-बद्ध",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "आंदोलन",
								"level": null,
								"macroSkills": [{
									"name": "दृश्य",
									"level": null,
									"subSkills": [{
										"name": "दौड़ना",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "बचना",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "तैरना",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "कूदना",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "अदृश्य",
									"level": null,
									"subSkills": [{
										"name": "फिसलना",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "ताला तोड़ना",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "चोरी करना",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "क्लाइम्बिंग",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "पशु",
									"level": null,
									"subSkills": [{
										"name": "सवारी करना",
										"level": 0,
										"dropdownValue": 4
									}]
								}]
							}],
							"aura": [{
								"name": "तत्वीय",
								"level": null,
								"macroSkills": [{
									"name": "विनाश",
									"level": null,
									"subSkills": [{
										"name": "Pirica",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "बर्फीला",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "अम्लीय",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "प्रहारात्मक",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "गैल्वैनिक",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "परिवर्तन",
									"level": null,
									"subSkills": [{
										"name": "गर्म",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "ठंडा",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "विषाक्त",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "कंपनशील",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "विद्युत",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "पुनर्स्थापन",
								"level": null,
								"macroSkills": [{
									"name": "भौतिक",
									"level": null,
									"subSkills": [{
										"name": "घाव",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "वस्तु",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "ज़हर",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "आध्यात्मिक",
									"level": null,
									"subSkills": [{
										"name": "मन",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "आत्मिक",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "आह्वान",
								"level": null,
								"macroSkills": [{
									"name": "भौतिक",
									"level": null,
									"subSkills": [{
										"name": "संरचनात्मक",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "पुकार",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "आध्यात्मिक",
									"level": null,
									"subSkills": [{
										"name": "अभा",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "गूढ़",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "अस्ट्रल",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "भविष्यवाणी",
								"level": null,
								"macroSkills": [{
									"name": "भविष्यवाणी",
									"level": null,
									"subSkills": [{
										"name": "Ad Arma",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "Ad Objecta",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Mechanismos",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Personam",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "Ad Caelum",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}, {
								"name": "अनुभूति",
								"level": null,
								"macroSkills": [{
									"name": "भ्रम",
									"level": null,
									"subSkills": [{
										"name": "भौतिक",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "मानसिक",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "स्मरण",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "छिपाना",
										"level": 0,
										"dropdownValue": null
									}]
								}, {
									"name": "दूरदर्शिता",
									"level": null,
									"subSkills": [{
										"name": "पूर्वानुमान",
										"level": 0,
										"dropdownValue": 4
									}, {
										"name": "रेट्रो-कॉग्निशन",
										"level": 0,
										"dropdownValue": null
									}, {
										"name": "अस्ट्रल",
										"level": 0,
										"dropdownValue": null
									}]
								}]
							}],
							"anima": []
						}
					}]
				},
			};
		</script>
	</head>
	<body>
		<!-- New Language Selector: Globe button with drop-down -->
		<div id="languageSwitcherContainer">
			<button id="languageSwitcherButton" onclick="toggleLanguageDropdown()">🌐 Change Language</button>
			<select id="languageDropdown" onchange="changeLanguage(this.value)">
				<option value="en">English</option>
				<option value="it">Italiano</option>
				<option value="pt">Português</option>
				<option value="es">Español</option>
				<option value="zh">中文</option>
				<option value="ja">日本語</option>
				<option value="nl">Nederlandse</option>
				<option value="de">Deutsche</option>
				<option value="hi">हिंदी</option>
			</select>
		</div>
		<!-- Background stars container (populated dynamically) -->
		<div class="shard-container"></div>
		<div class="container">
			<h1>Character Creation</h1>
			<!-- Pre-made Character Dropdown -->
			<div class="section">
				<label for="preMadeCharacterSelect">Pre-made Character:</label>
				<select id="preMadeCharacterSelect">
					<!-- Options populated via JS -->
				</select>
			</div>
			<div class="section">
				<label for="characterName">Character Name:</label>
				<input type="text" id="characterName" placeholder="Persona Name">
			</div>
			<div class="section">
				<label for="characterType">Character Type:</label>
				<select id="characterType">
					<option value="BOUND ONE">Bound One</option>
					<option value="MIND WISP">Mind Wisp</option>
					<option value="MIND SHADE">Mind Shade</option>
					<option value="WRAITH">Wraith</option>
					<option value="ECHO">Echo</option>
					<option value="EARTH BOUNDED">Earth Bounded</option>
					<option value="FRAGMENTED">Fragmented</option>
					<option value="GREYWALKER">Greywalker</option>
					<option value="HUSK">Husk</option>
					<option value="LIFELESS">Lifeless</option>
					<option value="HOLLOW">Hollow</option>
					<option value="SHELL">Shell</option>
					<option value="WISP">Wisp</option>
					<option value="PHANTOM">Phantom</option>
					<option value="SOUL SHART">Soul Shard</option>
				</select>
			</div>
			<!-- Starting Mode Checkbox -->
			<div class="section">
				<input type="checkbox" id="startingMode" onclick="validateAll()">
				<label for="startingMode">Starting Mode</label>
			</div>
			<!-- Aspect Blocks -->
			<div id="egoBlock" class="aspect-block">
				<div class="aspect-header">
					<div class="aspect-title">EGO</div>
					<button onclick="addHyper('ego')">Add Hyper Skill</button>
				</div>
				<div id="egoSkills"></div>
			</div>
			<div id="corpusBlock" class="aspect-block">
				<div class="aspect-header">
					<div class="aspect-title">CORPUS</div>
					<button onclick="addHyper('corpus')">Add Hyper Skill</button>
				</div>
				<div id="corpusSkills"></div>
			</div>
			<div id="auraBlock" class="aspect-block">
				<div class="aspect-header">
					<div class="aspect-title">AURA</div>
					<button onclick="addHyper('aura')">Add Hyper Skill</button>
				</div>
				<div id="auraSkills"></div>
			</div>
			<div id="animaBlock" class="aspect-block">
				<div class="aspect-header">
					<div class="aspect-title">ANIMA</div>
					<button onclick="addHyper('anima')">Add Hyper Skill</button>
				</div>
				<div id="animaSkills"></div>
			</div>
			<!-- Perks Section -->
			<div id="perksSection" class="aspect-block">
				<div class="aspect-header">
					<div class="aspect-title">Perks</div>
					<button onclick="addPerk()">Add Perk</button>
				</div>
				<div id="perksContainer"></div>
			</div>
			<!-- Back Story Note Box -->
			<div class="section">
				<label for="backstory">Back Story:</label>
				<textarea id="backstory" placeholder="Enter character back story here..." rows="5" style="width:100%;"></textarea>
			</div>
			<!-- Save/Load Buttons -->
			<div class="section">
				<button onclick="saveCharacter()">Save Character</button>
				<button onclick="loadCharacter()">Load Character</button>
			</div>
		</div>
		<!-- Restrict numeric input & paste -->
		<script>
			function onlyAllowIntegers(event) {
				if (
					(event.key < '0' || event.key > '9') && event.key !== 'Backspace' && event.key !== 'ArrowLeft' && event.key !== 'ArrowRight' && event.key !== 'Delete' && event.key !== 'Tab' && event.key !== 'Enter') {
					event.preventDefault();
				}
			}

			function blockNonNumericPaste(e) {
				const clipboardData = e.clipboardData.getData('text');
				if (!/^\d+$/.test(clipboardData)) {
					e.preventDefault();
				}
			}
		</script>
		<!-- Background stars and parallax -->
		<script>
			document.addEventListener('DOMContentLoaded', function() {
				const shardContainer = document.querySelector('.shard-container');
				const numShards = 150;
				shardContainer.innerHTML = '';
				for (let i = 0; i < numShards; i++) {
					const shard = document.createElement('div');
					shard.classList.add('shard');
					const posX = Math.random() * 100;
					const posY = Math.random() * 100;
					shard.style.left = posX + '%';
					shard.style.top = posY + '%';
					const size = 2 + Math.random() * 3;
					shard.style.width = size + 'px';
					shard.style.height = size + 'px';
					const angle = Math.random() * 2 * Math.PI;
					const driftDistance = 15 + Math.random() * 10;
					const driftX = driftDistance * Math.cos(angle);
					const driftY = driftDistance * Math.sin(angle);
					shard.style.setProperty('--drift-x', driftX + 'px');
					shard.style.setProperty('--drift-y', driftY + 'px');
					const duration = 10 + Math.random() * 10;
					const delay = Math.random() * 5;
					const glowDuration = 2 + Math.random() * 3;
					const glowDelay = Math.random() * 2;
					shard.style.animation = `drift ${duration}s linear ${delay}s infinite alternate, glow ${glowDuration}s ease-in-out ${glowDelay}s infinite alternate`;
					shardContainer.appendChild(shard);
				}
				window.addEventListener('scroll', function() {
					const scrollY = window.scrollY;
					const parallaxFactor = 0.025;
					shardContainer.style.transform = `translateY(${scrollY * parallaxFactor}px)`;
				});
			});
		</script>
		<!-- Language switching and core functions -->
		<script>
			let currentLang = 'en'; // start in English
			function toggleLanguageDropdown() {
				const dropdown = document.getElementById('languageDropdown');
				dropdown.style.display = (dropdown.style.display === 'none' || dropdown.style.display === '') ? 'block' : 'none';
			}

			function changeLanguage(lang) {
				currentLang = lang;
				// Hide the dropdown after selection
				document.getElementById('languageDropdown').style.display = 'none';
				// Update static UI text
				document.querySelector('h1').textContent = translations[currentLang].ui.title;
				document.querySelector('label[for="preMadeCharacterSelect"]').textContent = translations[currentLang].ui.preMadeCharacterLabel;
				document.querySelector('label[for="characterName"]').textContent = translations[currentLang].ui.characterNameLabel;
				document.querySelector('label[for="characterType"]').textContent = translations[currentLang].ui.characterTypeLabel;
				document.querySelector('label[for="startingMode"]').textContent = translations[currentLang].ui.startingModeLabel;
				document.querySelector('label[for="backstory"]').textContent = translations[currentLang].ui.backstoryLabel;
				document.querySelector('button[onclick="saveCharacter()"]').textContent = translations[currentLang].ui.saveCharacterButton;
				document.querySelector('button[onclick="loadCharacter()"]').textContent = translations[currentLang].ui.loadCharacterButton;
				// Update aspect block buttons
				document.getElementById('egoBlock').querySelector('button').textContent = translations[currentLang].ui.addHyperButton;
				document.getElementById('corpusBlock').querySelector('button').textContent = translations[currentLang].ui.addHyperButton;
				document.getElementById('auraBlock').querySelector('button').textContent = translations[currentLang].ui.addHyperButton;
				document.getElementById('animaBlock').querySelector('button').textContent = translations[currentLang].ui.addHyperButton;
				document.getElementById('perksSection').querySelector('button').textContent = translations[currentLang].ui.addPerkButton;
				// Re-populate the pre-made dropdown
				populatePreMadeDropdown();
			}

			function populatePreMadeDropdown() {
				const select = document.getElementById('preMadeCharacterSelect');
				select.innerHTML = '';
				translations[currentLang].preMadeCharacters.forEach((char, index) => {
					const opt = document.createElement('option');
					opt.value = index;
					opt.textContent = char.name;
					select.appendChild(opt);
				});
			}

			function loadPreMadeCharacter() {
				const select = document.getElementById('preMadeCharacterSelect');
				const index = select.value;
				if (index === "") return;
				const charData = translations[currentLang].preMadeCharacters[index];
				// Basic fields
				document.getElementById('characterName').value = charData.name || '';
				document.getElementById('characterType').value = charData.type || 'BOUND ONE';
				document.getElementById('startingMode').checked = !!charData.startingMode;
				updateAspectVisibility();
				// Clear existing aspects & perks
				['ego', 'corpus', 'aura', 'anima'].forEach(aspect => {
					document.getElementById(`${aspect}Skills`).innerHTML = '';
				});
				document.getElementById('perksContainer').innerHTML = '';
				// Rebuild aspects
				['ego', 'corpus', 'aura', 'anima'].forEach(aspect => {
					if (!charData.aspects || !charData.aspects[aspect]) return;
					const aspectBlock = document.getElementById(`${aspect}Skills`);
					charData.aspects[aspect].forEach(hyperData => {
						const hyper = document.createElement('div');
						hyper.className = 'hyper-item';
						hyper.innerHTML = `
            
					
					
					<div class="flex-container">
						<input type="text" value="${hyperData.name || ''}">
							<input type="number" class="level-input hyper-level"
                     value="${Number.isInteger(hyperData.level) ? hyperData.level : ''}"
                     placeholder="Lvl" min="0"
                     ${(!Number.isInteger(hyperData.level)) ? 'style="display:none"' : ''}>
								<button onclick="addMacro(this)">${translations[currentLang].ui.addMacroButton}</button>
								<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
							</div>
							<div class="macro-container"></div>
          `;
						aspectBlock.appendChild(hyper);
						const macroContainer = hyper.querySelector('.macro-container');
						(hyperData.macroSkills || []).forEach(macroData => {
							const macro = document.createElement('div');
							macro.className = 'macro-item';
							macro.innerHTML = `
              
							
							
							<div class="flex-container">
								<input type="text" value="${macroData.name || ''}">
									<input type="number" class="level-input macro-level"
                       value="${Number.isInteger(macroData.level) ? macroData.level : ''}"
                       placeholder="Lvl" min="0"
                       ${(!Number.isInteger(macroData.level)) ? 'style="display:none"' : ''}>
										<button onclick="addSub(this)">${translations[currentLang].ui.addSubButton}</button>
										<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
									</div>
									<div class="sub-container"></div>
            `;
							macroContainer.appendChild(macro);
							const subContainer = macro.querySelector('.sub-container');
							(macroData.subSkills || []).forEach((subData, idx) => {
								const sub = document.createElement('div');
								sub.className = 'sub-item';
								if (idx === 0) {
									sub.innerHTML = `
                  
									
									
									<div class="flex-container">
										<input type="text" value="${subData.name || ''}">
											<input type="number" class="level-input" value="${subData.level || ''}" min="0">
												<select class="level-dropdown">
													<option value="4">Scarso</option>
													<option value="14">Standard</option>
													<option value="24">Proficiente</option>
													<option value="34">Professionale</option>
													<option value="44">Esperto</option>
													<option value="50">Maestro</option>
												</select>
												<span class="leftover-points"></span>
												<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
											</div>
                `;
									const dd = sub.querySelector('.level-dropdown');
									if (dd && subData.dropdownValue) {
										dd.value = subData.dropdownValue;
									}
								} else {
									sub.innerHTML = `
                  
											
											
											<div class="flex-container">
												<input type="text" value="${subData.name || ''}">
													<input type="number" class="level-input" value="${subData.level || ''}" min="0">
														<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
													</div>
                `;
								}
								subContainer.appendChild(sub);
							});
							if ((macroData.subSkills || []).length > 0) {
								const macroLevel = macro.querySelector('.macro-level');
								if (macroLevel) macroLevel.style.display = 'none';
							}
						});
						if ((hyperData.macroSkills || []).length > 0) {
							const hyperLevel = hyper.querySelector('.hyper-level');
							if (hyperLevel) hyperLevel.style.display = 'none';
						}
					});
				});
				attachLevelChangeListeners();
				validateAll();
			}

			function updateAspectVisibility() {
				const type = document.getElementById('characterType').value;
				const aspectConfig = {
					"BOUND ONE": ["ego", "corpus", "aura", "anima"],
					"MIND WISP": ["ego"],
					"MIND SHADE": ["ego", "aura"],
					"WRAITH": ["ego", "aura", "anima"],
					"ECHO": ["ego", "anima"],
					"EARTH BOUNDED": ["ego", "corpus"],
					"FRAGMENTED": ["ego", "corpus", "aura"],
					"GREYWALKER": ["ego", "corpus", "anima"],
					"HUSK": ["corpus"],
					"LIFELESS": ["corpus", "aura"],
					"HOLLOW": ["corpus", "aura", "anima"],
					"SHELL": ["corpus", "anima"],
					"WISP": ["aura"],
					"PHANTOM": ["aura", "anima"],
					"SOUL SHART": ["anima"]
				};
				const aspects = aspectConfig[type] || [];
				['ego', 'corpus', 'aura', 'anima'].forEach(aspect => {
					const block = document.getElementById(`${aspect}Block`);
					block.style.display = aspects.includes(aspect) ? 'block' : 'none';
				});
			}
			document.getElementById('characterType').addEventListener('change', updateAspectVisibility);

			function addHyper(aspect) {
				const container = document.getElementById(`${aspect}Skills`);
				const hyper = document.createElement('div');
				hyper.className = 'hyper-item';
				hyper.innerHTML = `
        
													
													
													<div class="flex-container">
														<input type="text" placeholder="Hyper Skill">
															<input type="number" class="level-input hyper-level" placeholder="Lvl" min="0">
																<button onclick="addMacro(this)">${translations[currentLang].ui.addMacroButton}</button>
																<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
															</div>
															<div class="macro-container"></div>
      `;
				container.appendChild(hyper);
				attachLevelChangeListeners();
				validateAll();
			}

			function addMacro(button) {
				const hyperContainer = button.closest('.hyper-item');
				const macroContainer = hyperContainer.querySelector('.macro-container');
				const macro = document.createElement('div');
				macro.className = 'macro-item';
				macro.innerHTML = `
        
															
															
															<div class="flex-container">
																<input type="text" placeholder="Macro Skill">
																	<input type="number" class="level-input macro-level" placeholder="Lvl" min="0">
																		<button onclick="addSub(this)">${translations[currentLang].ui.addSubButton}</button>
																		<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
																	</div>
																	<div class="sub-container"></div>
      `;
				macroContainer.appendChild(macro);
				const hyperLevel = hyperContainer.querySelector('.hyper-level');
				if (hyperLevel) hyperLevel.style.display = 'none';
				attachLevelChangeListeners();
				validateAll();
			}

			function addSub(button) {
				const macroItem = button.closest('.macro-item');
				const subContainer = macroItem.querySelector('.sub-container');
				const subCount = subContainer.querySelectorAll('.sub-item').length;
				const sub = document.createElement('div');
				sub.className = 'sub-item';
				if (subCount === 0) {
					sub.innerHTML = `
          
																	
																	
																	<div class="flex-container">
																		<input type="text" placeholder="Sub Skill">
																			<input type="number" class="level-input" placeholder="Lvl" min="0">
																				<select class="level-dropdown">
																					<option value="4">Scarso</option>
																					<option value="14">Standard</option>
																					<option value="24">Proficiente</option>
																					<option value="34">Professionale</option>
																					<option value="44">Esperto</option>
																					<option value="50">Maestro</option>
																				</select>
																				<span class="leftover-points"></span>
																				<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
																			</div>
        `;
				} else {
					sub.innerHTML = `
          
																			
																			
																			<div class="flex-container">
																				<input type="text" placeholder="Sub Skill">
																					<input type="number" class="level-input" placeholder="Lvl" min="0">
																						<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
																					</div>
        `;
				}
				subContainer.appendChild(sub);
				const macroLevel = macroItem.querySelector('.macro-level');
				if (macroLevel) macroLevel.style.display = 'none';
				attachLevelChangeListeners();
				validateAll();
			}

			function removeSkill(button) {
				const item = button.closest('.hyper-item, .macro-item, .sub-item');
				if (item.classList.contains('hyper-item')) {
					item.remove();
					validateAll();
					return;
				}
				if (item.classList.contains('macro-item')) {
					const hyperItem = item.closest('.hyper-item');
					item.remove();
					if (hyperItem && hyperItem.querySelectorAll('.macro-item').length === 0) {
						const hyperLevel = hyperItem.querySelector('.hyper-level');
						if (hyperLevel) hyperLevel.style.display = 'block';
					}
					validateAll();
					return;
				}
				if (item.classList.contains('sub-item')) {
					const macroItem = item.closest('.macro-item');
					item.remove();
					if (macroItem && macroItem.querySelectorAll('.sub-item').length === 0) {
						const macroLevel = macroItem.querySelector('.macro-level');
						if (macroLevel) macroLevel.style.display = 'block';
					}
					validateAll();
				}
			}

			function addPerk() {
				const perksContainer = document.getElementById('perksContainer');
				const perk = document.createElement('div');
				perk.className = 'perk-item';
				perk.innerHTML = `
        
																					
																					
																					<div class="perk-container">
																						<input type="number" class="perk-number" placeholder="Lvl" min="0">
																							<input type="text" class="perk-text" placeholder="Perk Description">
																								<button class="remove-btn" onclick="removePerk(this)">${translations[currentLang].ui.removeButton}</button>
																							</div>
      `;
				perksContainer.appendChild(perk);
				attachLevelChangeListeners();
			}

			function removePerk(button) {
				const perk = button.closest('.perk-item');
				if (perk) perk.remove();
			}

			function validateAll() {
				const startingMode = document.getElementById('startingMode').checked;
				document.querySelectorAll('.level-input').forEach(input => {
					let val = parseInt(input.value, 10);
					if (isNaN(val) || val < 0) {
						val = 0;
					}
					input.value = val;
				});
				document.querySelectorAll('.perk-number').forEach(input => {
					let val = parseInt(input.value, 10);
					if (isNaN(val) || val < 0) {
						val = 0;
					}
					input.value = val;
				});
				if (!startingMode) {
					document.querySelectorAll('.macro-item').forEach(macro => {
						macro.classList.remove('invalid');
					});
					document.querySelectorAll('.leftover-points').forEach(span => {
						span.textContent = '';
					});
					return;
				}
				document.querySelectorAll('.macro-item').forEach(macro => {
					macro.classList.remove('invalid');
					const subItems = macro.querySelectorAll('.sub-item');
					if (subItems.length === 0) return;
					const firstSub = subItems[0];
					const dropdown = firstSub.querySelector('.level-dropdown');
					const leftoverSpan = firstSub.querySelector('.leftover-points');
					if (!dropdown || !leftoverSpan) return;
					const levelType = parseInt(dropdown.value) || 0;
					let sumOfLevels = 0;
					subItems.forEach(sub => {
						const levelInput = sub.querySelector('.level-input');
						if (levelInput) {
							let currentVal = parseInt(levelInput.value) || 0;
							if (currentVal > levelType) {
								levelInput.value = levelType;
								currentVal = levelType;
							}
							sumOfLevels += currentVal;
						}
					});
					const countOfSubs = subItems.length;
					const maxPoints = Math.floor(levelType + (countOfSubs - 1) * levelType * 0.65);
					const leftover = maxPoints - sumOfLevels;
					if (leftover < 0) {
						macro.classList.add('invalid');
						leftoverSpan.textContent = `Exceeded by ${Math.abs(leftover)}`;
					} else {
						leftoverSpan.textContent = `${leftover} points left`;
					}
				});
			}

			function attachLevelChangeListeners() {
				const inputs = document.querySelectorAll('.level-input, .level-dropdown, .perk-number');
				inputs.forEach(input => {
					input.removeEventListener('input', validateAll);
					input.removeEventListener('change', validateAll);
					input.removeEventListener('keydown', onlyAllowIntegers);
					input.removeEventListener('paste', blockNonNumericPaste);
					input.addEventListener('input', validateAll);
					input.addEventListener('change', validateAll);
					if (input.classList.contains('level-input') || input.classList.contains('perk-number')) {
						input.addEventListener('keydown', onlyAllowIntegers);
						input.addEventListener('paste', blockNonNumericPaste);
					}
				});
			}

			function saveCharacter() {
				const character = {
					name: document.getElementById('characterName').value,
					type: document.getElementById('characterType').value,
					startingMode: document.getElementById('startingMode').checked,
					aspects: {}
				};
				['ego', 'corpus', 'aura', 'anima'].forEach(aspect => {
					const container = document.getElementById(`${aspect}Skills`);
					const hyperItems = Array.from(container.querySelectorAll('.hyper-item'));
					character.aspects[aspect] = hyperItems.map(hyper => {
						const hyperName = hyper.querySelector('input[type="text"]').value;
						const hyperLevelInput = hyper.querySelector('.hyper-level');
						const hyperLevel = (hyperLevelInput && hyperLevelInput.style.display !== 'none') ? parseInt(hyperLevelInput.value) || 0 : null;
						const macroItems = Array.from(hyper.querySelectorAll('.macro-item'));
						const macros = macroItems.map(macro => {
							const macroName = macro.querySelector('input[type="text"]').value;
							const macroLevelInput = macro.querySelector('.macro-level');
							const macroLevel = (macroLevelInput && macroLevelInput.style.display !== 'none') ? parseInt(macroLevelInput.value) || 0 : null;
							const subItems = Array.from(macro.querySelectorAll('.sub-item'));
							const subs = subItems.map((sub) => {
								const subName = sub.querySelector('input[type="text"]').value;
								const subLevel = parseInt(sub.querySelector('.level-input').value) || 0;
								let subDropdownValue = null;
								const subDropdown = sub.querySelector('.level-dropdown');
								if (subDropdown) {
									subDropdownValue = parseInt(subDropdown.value) || 0;
								}
								return {
									name: subName,
									level: subLevel,
									dropdownValue: subDropdownValue
								};
							});
							return {
								name: macroName,
								level: macroLevel,
								subSkills: subs
							};
						});
						return {
							name: hyperName,
							level: hyperLevel,
							macroSkills: macros
						};
					});
				});
				const perks = [];
				document.querySelectorAll('#perksContainer .perk-item').forEach(perk => {
					const numberVal = perk.querySelector('.perk-number').value;
					const textVal = perk.querySelector('.perk-text').value;
					perks.push({
						level: numberVal ? parseInt(numberVal) : 0,
						description: textVal
					});
				});
				character.perks = perks;
				character.backstory = document.getElementById('backstory').value;
				const blob = new Blob([JSON.stringify(character, null, 2)], {
					type: 'application/json'
				});
				const url = URL.createObjectURL(blob);
				const a = document.createElement('a');
				a.href = url;
				a.download = `${character.name || 'character'}.json`;
				a.click();
				URL.revokeObjectURL(url);
			}

			function loadCharacter() {
				const input = document.createElement('input');
				input.type = 'file';
				input.accept = '.json';
				input.onchange = (e) => {
					const file = e.target.files[0];
					const reader = new FileReader();
					reader.onload = (e) => {
						try {
							const data = JSON.parse(e.target.result);
							document.getElementById('characterName').value = data.name;
							document.getElementById('characterType').value = data.type;
							document.getElementById('startingMode').checked = !!data.startingMode;
							updateAspectVisibility();
							['ego', 'corpus', 'aura', 'anima'].forEach(aspect => {
								document.getElementById(`${aspect}Skills`).innerHTML = '';
							});
							document.getElementById('perksContainer').innerHTML = '';
							document.getElementById('backstory').value = '';
							for (let aspect of ['ego', 'corpus', 'aura', 'anima']) {
								if (!data.aspects || !data.aspects[aspect]) continue;
								const aspectBlock = document.getElementById(`${aspect}Skills`);
								data.aspects[aspect].forEach(hyperData => {
									const hyper = document.createElement('div');
									hyper.className = 'hyper-item';
									hyper.innerHTML = `
                  
																							
																							
																							<div class="flex-container">
																								<input type="text" value="${hyperData.name || ''}">
																									<input type="number" class="level-input hyper-level"
                           value="${hyperData.level || ''}"
                           placeholder="Lvl" min="0"
                           ${(!Number.isInteger(hyperData.level)) ? 'style="display:none"' : ''}>
																										<button onclick="addMacro(this)">${translations[currentLang].ui.addMacroButton}</button>
																										<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
																									</div>
																									<div class="macro-container"></div>
                `;
									aspectBlock.appendChild(hyper);
									const macroContainer = hyper.querySelector('.macro-container');
									(hyperData.macroSkills || []).forEach(macroData => {
										const macro = document.createElement('div');
										macro.className = 'macro-item';
										macro.innerHTML = `
                    
																									
																									
																									<div class="flex-container">
																										<input type="text" value="${macroData.name || ''}">
																											<input type="number" class="level-input macro-level"
                             value="${macroData.level || ''}"
                             placeholder="Lvl" min="0"
                             ${(!Number.isInteger(macroData.level)) ? 'style="display:none"' : ''}>
																												<button onclick="addSub(this)">${translations[currentLang].ui.addSubButton}</button>
																												<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
																											</div>
																											<div class="sub-container"></div>
                  `;
										macroContainer.appendChild(macro);
										const subContainer = macro.querySelector('.sub-container');
										(macroData.subSkills || []).forEach((subData, idx) => {
											const sub = document.createElement('div');
											sub.className = 'sub-item';
											if (idx === 0) {
												sub.innerHTML = `
                        
																											
																											
																											<div class="flex-container">
																												<input type="text" value="${subData.name || ''}">
																													<input type="number" class="level-input" value="${subData.level || ''}" min="0">
																														<select class="level-dropdown">
																															<option value="4">Scarso</option>
																															<option value="14">Standard</option>
																															<option value="24">Proficiente</option>
																															<option value="34">Professionale</option>
																															<option value="44">Esperto</option>
																															<option value="50">Maestro</option>
																														</select>
																														<span class="leftover-points"></span>
																														<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
																													</div>
                      `;
												const dd = sub.querySelector('.level-dropdown');
												if (dd && subData.dropdownValue) {
													dd.value = subData.dropdownValue;
												}
											} else {
												sub.innerHTML = `
                        
																													
																													
																													<div class="flex-container">
																														<input type="text" value="${subData.name || ''}">
																															<input type="number" class="level-input" value="${subData.level || ''}" min="0">
																																<button class="remove-btn" onclick="removeSkill(this)">${translations[currentLang].ui.removeButton}</button>
																															</div>
                      `;
											}
											subContainer.appendChild(sub);
										});
										if ((macroData.subSkills || []).length > 0) {
											const macroLevel = macro.querySelector('.macro-level');
											if (macroLevel) macroLevel.style.display = 'none';
										}
									});
									if ((hyperData.macroSkills || []).length > 0) {
										const hyperLevel = hyper.querySelector('.hyper-level');
										if (hyperLevel) hyperLevel.style.display = 'none';
									}
								});
							}
							if (data.perks) {
								data.perks.forEach(perkData => {
									const perk = document.createElement('div');
									perk.className = 'perk-item';
									perk.innerHTML = `
                  
																															
																															
																															<div class="perk-container">
																																<input type="number" class="perk-number" placeholder="Lvl" min="0" value="${perkData.level || ''}">
																																	<input type="text" class="perk-text" placeholder="Perk Description" value="${perkData.description || ''}">
																																		<button class="remove-btn" onclick="removePerk(this)">${translations[currentLang].ui.removeButton}</button>
																																	</div>
                `;
									document.getElementById('perksContainer').appendChild(perk);
								});
							}
							if (data.backstory) {
								document.getElementById('backstory').value = data.backstory;
							}
							attachLevelChangeListeners();
							validateAll();
						} catch (error) {
							alert('Invalid file format');
						}
					};
					reader.readAsText(file);
				};
				input.click();
			}
			// On page load
			window.addEventListener('DOMContentLoaded', () => {
				populatePreMadeDropdown();
				document.getElementById('preMadeCharacterSelect').addEventListener('change', loadPreMadeCharacter);
				updateAspectVisibility();
				attachLevelChangeListeners();
				validateAll();
				// Optionally, you could load the first pre-made automatically:
				document.getElementById('preMadeCharacterSelect').selectedIndex = 0;
				loadPreMadeCharacter();
			});
		</script>
	</body>
</html>
