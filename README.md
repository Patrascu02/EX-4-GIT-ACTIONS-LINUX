# EX-4-GIT-ACTIONS-LINUX

Problema – Workflow GitHub Actions pentru Python
1️⃣ Pregătirea codului

Creează în repository-ul nou următoarele fișiere și structuri:

Fișier app.py – conține o funcție simplă care primește un nume și returnează un mesaj de salut.
Folder tests cu fișierul test_app.py – conține un test care verifică funcția din app.py.
2️⃣ Cerințe pentru fișierul Workflow .yml
🔹 1. Declanșator automat

Workflow-ul trebuie să pornească automat:

ori de câte ori cineva face push pe branch-ul main
ori de câte ori se deschide un pull_request

Nu trebuie folosit workflow_dispatch.

🔹 2. Job-ul 1: quality-gate
Rulează pe ubuntu-latest.
Pașii job-ului:
Adu codul din repository.
Instalează un program de tip linter pentru Python.
Rulează verificarea calității codului.
Job-ul trebuie să eșueze dacă sunt erori de stil sau probleme de cod, oprind procesul.
🔹 3. Job-ul 2: test-matrix
Rulează tot pe ubuntu-latest.
Trebuie să pornească doar dacă Job-ul 1 a trecut cu succes.
Matricea de testare trebuie să includă trei versiuni de Python: 3.10, 3.11, 3.12.
Pașii job-ului:
Adu codul din repository.
Instalează versiunea de Python din matrice.
Instalează un framework de testare pentru Python.
Rulează testele definite în folderul tests.
