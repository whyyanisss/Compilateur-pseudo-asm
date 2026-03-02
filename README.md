# Compilateur Pseudo-Assembleur BOLT-16

## 📝 Description du projet
Ce script Python est une application externe développée pour compléter le projet de microprocesseur BOLT-16. Il agit comme un compilateur permettant de traduire un programme écrit en pseudo-assembleur vers le code binaire compréhensible par le processeur.

Le script génère un format de sortie prêt à être intégré dans la mémoire d'instructions (ROM), structurant chaque ligne pour une assignation directe en VHDL (`MEM(index) <= ...`).

## ⚙️ Fonctionnalités

1. Prend en charge les 16 opérations arithmétiques, logiques et de contrôle de l'architecture BOLT-16.
2. Sépare et formate correctement l'instruction selon la norme du CPU : un Opcode sur 4 bits et une Adresse/Valeur immédiate sur 12 bits.
3. Support des variations de nommage (`SUBSTARCT` en `SUB`, `SHITF_LEFT` en `SHL`, etc.).
4. Accepte les adresses et valeurs immédiates en format décimal, hexadécimal classique (`0xF`) ou hexadécimal abrégé (`xF`).
5. Force automatiquement le masque à `x000` pour les instructions qui ne nécessitent pas d'adresse (comme `HALT`, `NOT`, `SHL`, `SHR`).
6. Génération de Bitstream : Crée un fichier texte binaire (`bitstream.txt`) incluant l'en-tête de la taille du code suivi des instructions sur 16 bits, prêt à être transmis au Bootloader via UART.

## 🛠️ Exemple de compilation

**Fichier d'entrée (`program.txt`) :**
```assembly
Assembly
INIT 5
STORE x001
HALT

Sortie 1 : VHDL ('rom_init.txt')
VHDL
MEM(1) <= x"0" & x"005";        --  INIT 5
MEM(2) <= x"2" & x"001";        --  STORE x001
MEM(3) <= x"F" & x"000";        --  HALT

Sortie 2 : Bitstream ('bitstream.txt')
Plaintext
0000000000000011
0000000000000101
0010000000000001
1111000000000000
(Remarque : La première ligne du bitstream correspond à la taille totale du programme, exigée par le protocole du Bootloader).
