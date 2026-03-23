# EX-4-GIT-ACTIONS-LINUX / DEVOPS PIPELINE TRAINING

Acest repository este un mediu de antrenament pentru concepte esențiale și avansate de DevOps, folosit pentru a construi un pipeline complet de CI/CD (Continuous Integration / Continuous Deployment).

---

## Problema 1 – Workflow GitHub Actions pentru Python (CI)

### 1️⃣ Pregătirea codului

Creează în repository-ul nou următoarele fișiere și structuri:

- **Fișier `app.py`** – conține o funcție simplă care primește un nume și returnează un mesaj de salut.  
- **Folder `tests`** cu fișierul `test_app.py` – conține un test care verifică funcția din `app.py`.

---

### 2️⃣ Cerințe pentru fișierul Workflow `.yml`

#### 🔹 1. Declanșator automat

Workflow-ul trebuie să pornească automat:  
- Ori de câte ori cineva face **push** pe branch-ul `main`  
- Ori de câte ori se deschide un **pull_request** > Nu trebuie folosit `workflow_dispatch`.

---

#### 🔹 2. Job-ul 1: `quality-gate`

- Rulează pe `ubuntu-latest`  
- Pașii job-ului:  
  1. Adu codul din repository.  
  2. Instalează un program de tip linter pentru Python (ex: `flake8`).  
  3. Rulează verificarea calității codului.  

> Job-ul trebuie să eșueze dacă sunt erori de stil sau probleme de cod, oprind procesul.

---

#### 🔹 3. Job-ul 2: `test-matrix`

- Rulează tot pe `ubuntu-latest`  
- Trebuie să pornească **doar dacă Job-ul 1 a trecut cu succes** - Matricea de testare trebuie să includă trei versiuni de Python: `3.10`, `3.11`, `3.12`  

Pașii job-ului:  
1. Adu codul din repository.  
2. Instalează versiunea de Python din matrice.  
3. Instalează un framework de testare pentru Python (ex: `pytest`).  
4. Rulează testele definite în folderul `tests`.

---
---

## Problema 2 – Pipeline de Release (CD), Securitate și Optimizare

### 1️⃣ Pregătirea repository-ului (Securitate)

Înainte de a scrie codul pentru workflow, trebuie să protejăm datele sensibile:
- Mergi în setările repository-ului pe GitHub (**Settings** -> **Secrets and variables** -> **Actions**).
- Creează un secret nou numit `PAROLA_SECRETA`.
- Setează valoarea secretului cu orice text dorești (aceasta simulează o parolă pentru baza de date sau un token de acces).

---

### 2️⃣ Cerințe pentru fișierul Workflow de Release (`release.yml`)

#### 🔹 1. Declanșator automat bazat pe etichete (Tags)

Workflow-ul trebuie să pornească automat **doar** atunci când:
- Se face push la un **Git Tag** care începe cu litera "v" (ex: `v1.0.0`, `v2.1.3`).

> Nu trebuie să pornească la un simplu push pe branch-ul `main`.

---

#### 🔹 2. Job-ul unic: `build-and-release`

- Rulează pe `ubuntu-latest`.
- Trebuie să aibă **permisiuni de scriere** (`permissions: contents: write`) pentru a putea publica oficial release-ul pe GitHub.

Pașii job-ului:
1. **Adu codul** din repository.
2. **Instalează Python** și activează funcția de **caching** pentru managerul de pachete (`pip`), pentru a optimiza viteza de instalare la rulările viitoare.
3. **Instalează dependențele** necesare scriptului tău.
4. **Testarea securității:** Afișează în consolă (folosind `echo`) valoarea variabilei salvate în GitHub Secrets, trăgând-o în mediu (`env`). Observă cum GitHub va masca automat parola cu `***` în log-uri.
5. **Împachetarea aplicației:** Folosește o comandă de Linux pentru a crea o arhivă `.zip` care să conțină codul aplicației.
6. **Crearea Release-ului:** Folosește utilitarul preinstalat GitHub CLI (`gh release create`) pentru a genera automat pagina de lansare oficială, atașând arhiva `.zip` creată anterior.

> **Atenție:** Pentru ultimul pas, utilitarul `gh` are nevoie de un token de autentificare. Vei folosi secretul generat automat de sistem, transmițând la `env` variabila `GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}`.

---

### 3️⃣ Execuția și Testarea Finală

Pentru a declanșa acest workflow de lansare, deschide terminalul și rulează:
1. `git tag v1.0.0`
2. `git push origin v1.0.0`

Verifică tab-ul **Actions** pentru a urmări pipeline-ul. Dacă are succes, pe pagina principală a repository-ului, în secțiunea **Releases** din dreapta, va apărea versiunea lansată oficial, gata de descărcare!
